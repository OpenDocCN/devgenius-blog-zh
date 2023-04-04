# 使用服务帐户的简单 GCP 身份验证

> 原文：<https://blog.devgenius.io/simple-gcp-authentication-with-service-accounts-6b877c2e2649?source=collection_archive---------1----------------------->

使用 GCP 服务帐户轻松安全地认证和使用 Google Cloud APIs 的实用指南。

![](img/e55e87dc282041d4fcb5ad52a3cc8a87.png)

照片由[晨酿](https://unsplash.com/@morningbrew?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了使用 Google Cloud 产品，应用程序必须首先向 Google 认证并检查 IAM 权限。虽然 [Google 的 IAM 文档](https://cloud.google.com/iam/docs)页面详细介绍了安全最佳实践，但作为一名非安全工程师，很难解析所有文本并为每个用例应用 Google 的建议。我知道我应该对我的服务帐户执行最小特权，但是在本地开发和在 GKE 上提供访问权限时，用 Google SDKs 进行身份验证的最佳方式是什么？在这篇文章中，我将总结与谷歌云交互时常见用例的建议。

# 使用 CLI (gcloud、terraform)

如果您主要通过 CLI 与 GCP 交互(或者调用`gsutil`、`gcloud`，或者通过`terraform`创建 GCP 组件)，创建一个具有各自角色的服务帐户，**使用服务帐户模拟特性**。直到最近，GCP 控制台还为用户提供了在创建服务帐户时创建和下载密钥的选项。现在，该选项隐藏在默认创建工作流中，用户必须单击服务帐户选项来手动创建密钥。

所有 Google Cloud APIs 都使用 OAuth2 进行认证。当您生成服务帐户密钥时，它会创建一个公钥/私钥对，用于将 JWT 令牌签名为真正的 GCP 凭据。虽然这很方便，但下载密钥被认为是危险的，因为它可能会被意外地签入版本控制，并且不会过期(默认的过期日期是 9999 年 12 月 31 日)。GCP 支持密钥轮换，但是集中执行这一点非常困难，除非你有像 [Vault 这样的工具来自动执行这个过程](https://medium.com/techking/key-rotation-in-google-cloud-3ee8ff0a7828)。

好消息是，您可以模拟服务帐户进行身份验证，而无需下载密钥。首先，您需要`serviceAccountTokenCreator`角色并用常规的`gcloud`命令运行`--impersonate-service-accouunt=<sa-name>@project.iam.gservicaccount.com`。您还可以设置您的配置，以避免每次都传入命令:

```
gcloud config set auth/impersonate_service_account \
  <sa-name>@project.iam.gserviceaccount.com
```

对于 Terraform，设置`GOOGLE_OAUTH_ACCESS_TOKEN`变量来传递 OAuth2 令牌:

```
export GOOGLE_OAUTH_ACCESS_TOKEN=$(gcloud auth print-access-token)
```

现在，您可以使用一个短期令牌运行`terraform`命令，而不是下载您必须安全管理的密钥。来自 Google 的 Ryan Canty 写了一篇关于这个主题的好文章，其中有在多个服务帐户之间切换的 bash 脚本示例:

[](https://medium.com/@jryancanty/stop-downloading-google-cloud-service-account-keys-1811d44a97d9) [## 停止下载谷歌云服务账户密钥！

### TL；DR:下载服务帐户密钥会给你的组织带来严重的安全风险，因为它们的寿命很长…

medium.com](https://medium.com/@jryancanty/stop-downloading-google-cloud-service-account-keys-1811d44a97d9) 

# 当地/非 GCP 发展

所有 Google Cloud 客户端库都使用一个名为应用默认凭证(ADC)的底层认证库来自动查找和设置服务帐户凭证。它按以下顺序查找凭据:

1.  它检查环境变量`GOOGLE_APPLICATION_CREDENTIALS`是否被设置。如果是，它使用 env 指向的服务帐户文件。
2.  如果未设置该变量，则它使用默认的服务帐户。
3.  如果 ADC 不能使用上述任何一项，它就会出错。

这为开发人员提供了两种身份验证选择:

1.  创建一个服务帐户，将密钥下载到一个安全的位置，并设置`GOOGLE_APPLICATION_CREDENTIALS`
2.  使用`gcloud auth application-default login`将您的用户凭证设置为`~/.config/gcloud/application_default_credentials.json`

虽然第二种方法对于初始测试更方便，但不推荐使用这种方法，因为它使用的是您的用户凭据，而不是服务帐户的权限范围，后者很可能受到更多限制。此外，一些 SDK 特性，如 [Firebase 定制令牌](https://firebase.google.com/docs/auth/admin/create-custom-tokens)和 [GCS 签名 URL](https://cloud.google.com/storage/docs/access-control/signed-urls)需要 client_email 字段，这不是应用程序默认登录凭证的一部分。

**Google 团队的一般建议是创建并下载 JSON 凭证文件，并将路径设置为** `**GOOGLE_APPLICATION_CREDENTIALS**`。考虑到上面列出的关于这些持久密钥的安全问题，请确保将其存储在安全的地方，并在开发 GCP 项目上进行测试。欲了解更多信息，请查看 Theodore Siu 关于 GCP 地方发展的文章:

[](https://medium.com/google-cloud/local-remote-authentication-with-google-cloud-platform-afe3aa017b95) [## 使用 Google 云平台进行本地/远程身份验证

### 在本地或远程机器上设置谷歌云平台认证可能是一项令人困惑的任务。

medium.com](https://medium.com/google-cloud/local-remote-authentication-with-google-cloud-platform-afe3aa017b95) 

*注意:这一部分也适用于在混合云架构上调用 GCP API。例如，您可能使用来自 AWS 的 Google ML API 或使用 Firebase 进行移动开发，同时将其他计算工作负载放在不同的云上。*

# 谷歌云上的应用

在 Google Cloud 上，ADC 在 Compute Engine、App Engine、Kubernetes Engine、Cloud Run 和 Cloud Functions 上运行时，会自动搜索默认服务帐户。默认服务帐户具有编辑者角色，并为各种 GCP 产品设置默认授权范围。例如，使用默认服务帐户创建的 GKE 群集授予对存储的只读访问权限，以及对堆栈驱动程序日志记录和监视的写访问权限。如果需要更细粒度的访问，可以创建一个新的服务帐户，并附加它以使用其 IAM 角色，而不是默认的服务帐户。

**至于让容器访问 Google 云资源，推荐的做法是使用** [**工作负载标识**](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity) 。工作负载身份类似于亚马逊 EKS IAM 服务帐户角色(IRSA ),因为它使 Kubernetes 服务帐户(KSA)能够在访问 Google Cloud APIs 时充当 Google 服务帐户(GSA)。一旦在 GKE 上启用了工作负载标识，就会创建一个名为`<project-id>.svc.id.goog`的标识池。

要在 KSA 和 GSA 之间建立连接，首先创建两个服务帐户:

```
$ kubectl create sa <ksa_name> -n <k8s_namespace>
$ gcloud iam service-accounts create <gsa-name>
```

将 IAM 策略与`roles/iam.workloadIdentityUser`角色绑定:

```
$ gcloud iam service-accounts add-iam-policy-binding \
  --role roles/iam.workloadIdentityUser \
  --member "serviceAccount:<project_id>.svc.id.goog[k8s_namespace/ksa_name]" \
  <gsa_name>@<project_id>.iam.gserviceaccount.com
```

最后，给 KSA 加一个注解:

```
$ kubectl annotate serviceaccount \
  --namespace <k8s_namespace> \
   <ksa_name> \
   iam.gke.io/gcp-service-account=<gsa_name>@<project_id>.iam.gserviceaccount.com
```

工作负载身份确实有一些限制，比如不支持 GKE 本地和 Windows 节点。在这种情况下，您可以从 secret 安装服务帐户密钥并设置`GOOGLE_APPLICATION_CREDENTIALS`，或者使用类似[保险库秘密注入器](https://www.hashicorp.com/blog/injecting-vault-secrets-into-kubernetes-pods-via-a-sidecar/)的工具。

# 最后的话

我绝不是安全专家，但你可以遵循这些简单的服务帐户建议来安全地访问 Google Cloud APIs，即使没有复杂的秘密管理设置。在接下来的步骤中，您还可以查看 [GCP CIS 基准 Inspec Profile 工具](https://github.com/GoogleCloudPlatform/inspec-gcp-cis-benchmark)，以确保您的项目涵盖了适当的 IAM 部分。