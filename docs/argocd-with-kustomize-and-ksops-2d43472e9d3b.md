# 与 Kustomize 和 ksops 的 ArgoCD

> 原文：<https://blog.devgenius.io/argocd-with-kustomize-and-ksops-2d43472e9d3b?source=collection_archive---------0----------------------->

![](img/df6503770e191e8ad1d8d1242346c259.png)

锁定在 Git Repo 中的秘密:)

# 介绍

Kustomize 让您自定义原始的，无模板的 YAML 文件的多种用途，留下原来的 YAML 原封不动和可用。当与 ArgoCD for gitops 结合使用时，该组合可以真正成为一个优秀的设置。

# 描述

我们不能将私钥或密码作为秘密推送给 git repo 来使用 gitops。这可以通过诸如 Vault、Keycloak、sop 等秘密管理工具来解决。最简单的是 SOPS，因为它用 PGP 密钥加密内容，而秘密由 kustomize 在集群内用相同的 PGP 密钥解密。

# 问题

与 FluxCD 不同，ArgoCD 没有内置的机密管理。但是 ArgoCD 提供了灵活性，可以与任何秘密管理工具集成。这里[解释了一些支持的解决方案](https://argo-cd.readthedocs.io/en/stable/operator-manual/secret-management/):大部分的文档都是通过云提供的秘密解决方案来设置 sop 与 ArgoCD 一起工作。

# 解决办法

有两种方法可以用 sop 设置 ArgoCD。

*   使用 kustomize 和 sops 创建自定义 ArgoCD docker 映像，并使用自定义 docker 映像。问题是 ArgoCD 的每个新版本都需要更新图像。所以我更喜欢第二种选择
*   在 ArgoCD repo 服务器部署中创建一个 init 容器，以获得带有 sops 的 kustomize 插件，如这里的[中所述，并在 pod 中使用它。即使 ArgoCD 版本更新了，插件也不需要更新，除非插件版本和 ArgoCD 版本存在兼容性问题。](https://github.com/viaduct-ai/kustomize-sops#argo-cd-integration)

因此，按照选项 2，考虑 ArgoCD 已经在集群中的`argocd`名称空间中运行。

首先生成一个 GPG 键:

```
export GPG_NAME="k3s.argotest.cluster"
export GPG_COMMENT="argocd secrets"

gpg --batch --full-generate-key <<EOF
%no-protection
Key-Type: 1
Key-Length: 4096
Subkey-Type: 1
Subkey-Length: 4096
Expire-Date: 0
Name-Comment: ${GPG_COMMENT}
Name-Real: ${GPG_NAME}
EOF
```

上面的代码将创建一个 4096 位的 RSA 密钥。有关创建 GPG 键的完整配置，请检查[此](https://www.gnupg.org/documentation/manuals/gnupg/Unattended-GPG-key-generation.html)链接

检索密钥名称

```
gpg --list-secret-keys "${GPG_NAME}"sec   rsa4096 2021-12-23 [SCEA]
      7AD1E449E7DDD762C7FCFAFA0D177809067B3970
uid           [ultimate] k3s.argotest.cluster (argocd secrets)
```

将 GPG 键指纹存储为环境变量

```
export GPG_ID=7AD1E449E7DDD762C7FCFAFA0D177809067B3970
```

从 GPG 密钥导出公钥和私钥对，并为 ArgoCD 创建一个 kubernetes 秘密来读取它们

```
gpg --export-secret-keys --armor "${GPG_ID} |
kubectl create secret generic sops-gpg **\** --namespace=argocd **\** --from-file=sops.asc=/dev/stdin
```

从本地机器上删除本地创建的私钥，因为密钥现在在集群上。

```
gpg --delete-secret-keys "${GPG_ID}"
```

如果一个团队正在进行回购，并且他们需要使用 sop 在本地加密机密，那么导出公钥并将其存储在回购中是一个好主意，这样他们就可以下载并使用该密钥来加密机密

```
gpg --export --armor "${GPG_ID}" > .sops.pub.asc
```

这个公钥可以被导入并用于在本地加密机密，然后推送到 git repos

```
gpg --import .sops.pub.asc
```

让我们创造一个虚假的秘密

```
kubectl -n myapp create secret generic app-secret \
--from-literal=token=4BD1CE23-E3C0-4BBF-A75E-ABA103EE95C9 \
--dry-run=client \
-o yaml > secret.yaml
```

导出的 secret yaml 将如下所示

```
apiVersion: v1
data:
  token: NEJEMUNFMjMtRTNDMC00QkJGLUE3NUUtQUJBMTAzRUU5NUM5
kind: Secret
metadata:
  name: app-secret
  namespace: myapp
```

用 sop 加密 secret.yaml

```
sops --encrypt --in-place secret.yaml
```

如果有多个 GPG 密钥，那么使用导出的 GPG ID 环境变量或 GPG 密钥 ID 用特定的密钥加密

```
sops --encrypt --in-place \
-p 7AD1E449E7DDD762C7FCFAFA0D177809067B3970 basic-auth.yaml
```

现在 secret.yaml 的内容将如下所示:

```
❯ cat secret.yaml
apiVersion: ENC[AES256_GCM,data:ges=,iv:bkleuf27QnbKQTqLdCnawJ4oJOnj/EA3sVVSLDNdRHo=,tag:BGZaPAV8maLId+KH/YFV0g==,type:str]
data:
    token: ENC[AES256_GCM,data:GdQcwyBNvMPxzrwQUNI6IKdvt3djCRR37mckZ+N6oDqfoCCa0FNILRAWJeT0BIQ5,iv:5ZCMiZpAGIkR5qaj1DdHvTY4K5mki5n2Q0QK8T5WtoA=,tag:sl+NG6u86QEMwth9eWhzBQ==,type:str]
kind: ENC[AES256_GCM,data:gD6BgdIS,iv:7bF7ViYyBxYYQE6WDgxEN/Vi+3qLSh3TzDIzZcFCjeM=,tag:60Ytp9Ec5ERVEXTH2CS/6w==,type:str]
metadata:
    creationTimestamp: null
    name: ENC[AES256_GCM,data:J224lqhNHkLLjg==,iv:gfENh1kucL0H9Tm/5cIiN8RRyiHTWPsX9j7/tbf9ffc=,tag:H4t+z664L05ExGBo9ZuyGA==,type:str]
    namespace: ENC[AES256_GCM,data:Jz6qCfc+3w==,iv:XoqLmWZibTxOkEwbsY86ygd0+oAfVzyMbNMCw9KsiOY=,tag:OIJabS57TlbjXAkce48BtA==,type:str]
sops:
    kms: []
    gcp_kms: []
    azure_kv: []
    hc_vault: []
    age: []
    lastmodified: "2021-12-23T10:09:26Z"
    mac: ENC[AES256_GCM,data:9pL458Y1TZ0g4LXqc1puif9eGz3uK5jkIRYQKECoAUBAkBlv9nbVN/DpZYpdGYfRSHuLNTJ9ASXSInSJqT4EXp9CegRDYnEEpcPq+PBV0a3wPuoPYeGMVXBax1JFSdmzakzVsBL0HcsyPWrU4YLoAc5Rl5kxO1NEBsMNQBl+mXg=,iv:UTETLuqmUG1SebwHjJP8lOKKBJQMSBPlIZjoiREPtj4=,tag:1YM6KpjzPX4vmkA6LSN+0w==,type:str]
    pgp:
        - created_at: "2021-12-23T10:09:25Z"
          enc: |
            -----BEGIN PGP MESSAGE-----hQIMA55I+RFb4Yu5AQ/9F/1eBWZccqsvbI53cIMT4gkrQ1p+p1n0PHIWW2VJKu6f
            6hC4OkbrJIcum87PgCCg8xxsUyDaUgd891/G7+KMGC8pl6fcEDMOqyEUR3tz12S0
            zIBWYx3mIniNmZrp6xnBFbokKahO6iALBlQJ9s4DsSMWqdonuDJtO6ILZI8U7v0k
            4H0abk1F3Eb7808IQ3w4BZhsnV1x/yxAghvsjQNOAHyBxIny8F74GmnRnkGLQqMc
            txRPKCs17eUhUMoMJ5rkXYzgTdbFqA/fMDRubgQ2eD9E8GfRE2rskEM4ednKg1A3
            VaTCeIXCazqxVpmxBYCqOGks5hfTrgY6WqW/aK8GIysusStXs/N5O8J4jgAy4pYt
            SZFy8wILBd6jozYLeJZO7jVsdZuDC0btxNwxK5fTnUnZeRySkUJDEEeyOeJOIwbt
            UaRe3B/Lu3wXY4PKD7F3sWsRKJXEP61MMDVc0hCH07g/U2bbq3+ucGnOMfNXIilb
            YfSPBR91TAyaB4B7MvlYjPdL9BIiJPAoddE7pEFh7iQSS29GnphYDC+uis84i8ET
            YWVvMm0NEymDlpROToczpN/yvQ29EvkSJX1qPmEol3vE+gB+Z6C9Gi6/KGy+sW1F
            ziot0887LWcYSPIZZNSk2Rc9sDoTAx6Q6JAN2Fthdb89yhL6oPWa8ZhicRgQ8cTU
            aAEJAhDZjpuTM/sNdNMstIX+OMeHVT0eiECAqNNtSNoXwXvRqhjubY6soXcXqtRR
            vSZ1TlPH+985ukG57fhV3zgfCpaxMmEfpQx5p2LtE98dQtkLUVwfx59113rVTDJu
            6VeWUKAYWPjf
            =moTo
            -----END PGP MESSAGE-----
          fp: 7AD1E449E7DDD762C7FCFAFA0D177809067B3970
    unencrypted_suffix: _unencrypted
    version: 3.7.1
```

这个秘密文件可以推送到回购。

现在对于 ArgoCD 部分，如前所述，我将使用`init-containers`作为 ArgoCD 回购服务器。

在修改 repo server 的部署之前，我们需要通过 configMap 进行 kustomize 以启用 ksops。这需要通过替换当前的`argocd-cm`配置图来完成。配置图如下所示:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
data:
  # For KSOPs versions < v2.5.0, use the old kustomize flag style
  # kustomize.buildOptions: "--enable_alpha_plugins"
  kustomize.buildOptions: "--enable-alpha-plugins"
```

repo-server-deployment 的补丁如下所示

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: argocd-repo-server
  namespace: argocd
spec:
  template:
    spec:
      initContainers:
        - name: install-ksops
          image: viaductoss/ksops:v3.0.1
          command: ["/bin/sh", "-c"]
          args:
            - echo "Installing KSOPS...";
              export PKG_NAME=ksops;
              mv ${PKG_NAME}.so /custom-tools/;
              mv $GOPATH/bin/kustomize /custom-tools/;
              echo "Done.";
          volumeMounts:
            - mountPath: /custom-tools
              name: custom-tools
        - name: import-gpg-key
          image: argoproj/argocd:v2.1.7
          command: ["gpg", "--import","/sops-gpg/sops.asc"]
          env:
            - name: GNUPGHOME
              value: /gnupg-home/.gnupg
          volumeMounts:
            - mountPath: /sops-gpg
              name: sops-gpg
            - mountPath: /gnupg-home
              name: gnupg-home
      containers:
      - name: argocd-repo-server
        env:
          - name: XDG_CONFIG_HOME
            value: /.config
          - name: GNUPGHOME
            value: /home/argocd/.gnupg
        volumeMounts:
        - mountPath: /home/argocd/.gnupg
          name: gnupg-home
          subPath: .gnupg
        - mountPath: /usr/local/bin/kustomize
          name: custom-tools
          subPath: kustomize
        - mountPath: /.config/kustomize/plugin/viaduct.ai/v1/ksops/ksops
          name: custom-tools
          subPath: ksops
      volumes:
      - name: custom-tools
        emptyDir: {}
      - name: gnupg-home
        emptyDir: {}
      - name: sops-gpg
        secret:
          secretName: sops-gpg
```

并为 repo server 应用配置映射和补丁

```
kubectl apply -f cm.yaml
kubectl patch deployment -n argocd argocd-repo-server --patch "$(cat repo-deploy-patch.yaml)
```

现在 ArgoCD 可以使用定制的`kustomize`插件来解密秘密，这个插件是用 sop 加密的，使用的是集群中作为秘密安装的 GPG 密钥的私钥。

享受使用 ArgoCD 在 GitOps 中加密的乐趣！！