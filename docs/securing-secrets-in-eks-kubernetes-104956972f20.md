# 保护 EKS 的秘密(Kubernetes)

> 原文：<https://blog.devgenius.io/securing-secrets-in-eks-kubernetes-104956972f20?source=collection_archive---------2----------------------->

![](img/ee850eb9674702be75637cff00ac9a43.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

每个人都有秘密。我们必须正确管理机密，因为它们通常包含证书和密钥等敏感信息。那么在 Kubernetes (k8s)中应该如何处理秘密呢？

默认情况下，k8s 提供 [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) 进行秘密管理。K8s secrets 非常灵活，允许 pods 从控制平面请求机密，然后这些机密可以通过环境变量在容器中公开，或者作为卷装入容器的文件系统 [*](https://kubernetes.io/docs/concepts/configuration/secret/#use-cases) 。当秘密被装载到容器的文件系统中时，当秘密被更新时，它也会自动更新 [*](https://kubernetes.io/docs/concepts/configuration/secret/#mounted-secrets-are-updated-automatically) 。K8s secrets 对象与 pod 分离，并且是独立创建的，因此您可以跨多个 pod 重用机密。

K8s 机密是处理 k8s 内部机密的强大方法，但是默认行为并不总是安全的，了解风险以及如何管理它们非常重要。

## 静态加密

K8s secrets 的工作原理是将秘密存储在控制平面的 [etcd](https://github.com/etcd-io/etcd) 中。默认情况下，机密只是 etcd 中的 base 64 编码，这与纯文本基本相同。由于 etcd 位于控制平面，在 EKS 你无法直接访问它，这并不一定是一件坏事，但是 EKS 确实提供了一种进一步锁定它的方法。在集群创建过程中，您需要在 EKS 集群中打开[信封加密](https://aws.amazon.com/about-aws/whats-new/2020/03/amazon-eks-adds-envelope-encryption-for-secrets-with-aws-kms/)，以进一步保护您的静态秘密。请注意，这仅适用于集群创建(此时不能在集群创建后进行)。

## 命名空间

机密被隔离在一个名称空间内，这意味着它们只能由在同一名称空间内运行的所有 pod 访问。为了保护您的秘密，您需要将您的应用程序隔离到单独的名称空间中，这样您的每个应用程序只能访问它需要的秘密，仅此而已。在应用程序被破坏的情况下，您还将减少爆炸半径，因为该应用程序只能访问其名称空间中的机密。

## 节点安全性

在每个节点上运行的 kubelet 负责从控制平面获取机密，并将机密卷安装到 pod 中。它还会定期检查装载的机密的新鲜度。这意味着 kubelet 非常强大，因为它可以访问所有的秘密，并且知道秘密何时更新。如果攻击者能够获得作为 EKS 集群一部分的 ec2 机器的超级用户访问权限，他们就可以冒充 kubelet。我们可以做的一件事是锁定对机器的 ssh 访问，这样攻击者就很难通过从 ec2 的安全组中删除任何端口 22 入口来访问机器。为了将访问权还给用户，我们可以用 [AWS 会话管理器](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)来代替它，这样只有经过身份验证的 IAM 角色或用户才能访问 ec2 机器的控制台。

我们还需要防止恶意机器加入您的集群。与 EKS 位于同一 VPC 的工作者节点可以通过运行 [bootstrap.sh](https://github.com/awslabs/amazon-eks-ami/blob/master/files/bootstrap.sh) 来加入 EKS 集群，只要工作者节点的实例角色被映射到 [aws-auth](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html) 配置图中的正确组和用户名。因此，这个配置图只能由集群管理员更新，这一点非常重要。

## 秘密的处理

根据您创建或生成秘密的方式，您需要一种方法来存储它们并将它们导入到您的集群中。有很多解决这个问题的开源项目。

第一个我想提到的是来自 Bitnami 的[密封的秘密](https://github.com/bitnami-labs/sealed-secrets)。这个项目试图通过让您将秘密安全地存储在 git 中来解决这个问题。您需要将 Sealed Secrets 控制器部署到您的集群上，然后当您创建您的秘密时，您调用您的集群上的控制器，您将需要访问您的 k8s api，以便使用公钥对它进行加密。

示例:

```
apiVersion: bitnami.com/v1alpha1
kind: SealedSecret
metadata:
  name: mysecret
  namespace: mynamespace
spec:
  encryptedData:
    foo: AgBy3i4OJSWK+PiTySYZZA9rO43cGDEq.....
```

然后，当您将 SealedSecret 部署到您的集群中时，Sealed secret 将解密资源，然后在您的集群中创建一个普通的 k8s secret。然后你的应用程序可以引用 k8s 秘密资源。因为 SealedSecret 是加密的，所以可以将它们提交给 git 存储库。有了密封的秘密，您可以确保您的整个秘密供应链受到保护。

另一种方法是利用 GoDaddy 的 E [外部秘密](https://github.com/external-secrets/kubernetes-external-secrets)和 [AWS 秘密管理器](https://aws.amazon.com/secrets-manager/)。External Secret 使用控制器将数据从外部源引入 k8s。通过这个设置，您将把您的秘密上传到 AWS Secret Manager，并使用 External Secret 将其从 Secret Manager 导入 k8s。类似于密封的秘密，您将需要创建一个自定义资源来指定秘密的来源。

示例:

```
apiVersion: 'kubernetes-client.io/v1'
kind: ExternalSecret
metadata:
  name: hello-service
spec:
  backendType: secretsManager
  # optional: specify role to assume when retrieving the data
  roleArn: arn:aws:iam::123456789012:role/test-role
  data:
    - key: hello-service/password
      name: password
  # optional: specify a template with any additional markup you would like added to the downstream Secret resource.
  # This template will be deep merged without mutating any existing fields. For example: you cannot override metadata.name.
  template:
    metadata:
      annotations:
        cat: cheese
      labels:
        dog: farfel
```

这个资源将告诉控制器去从秘密管理器中获取秘密，然后它将在 k8s 中创建一个秘密资源。类似于密封的秘密，这允许我们安全地处理秘密直到集群。

这些只是我在 EKS 保护我们自己的 k8s 秘密时发现的一些技巧，希望它们能对你的 EKS 之旅有所帮助。