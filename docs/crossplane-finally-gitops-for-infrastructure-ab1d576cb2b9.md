# Crossplane 终于停止了基础设施建设！

> 原文：<https://blog.devgenius.io/crossplane-finally-gitops-for-infrastructure-ab1d576cb2b9?source=collection_archive---------3----------------------->

![](img/b594b3e04d2b129743ae0c2f53337cbc.png)

【https://crossplane.io/】图片:*版权所有。*

您可能正在阅读标题，并认为我已经有了一个针对云基础架构的 GitOps 模型。

我使用 Terraform，代码存储在 Git 存储库中。

我使用基于拉式请求的自动化，例如 [Atlantis](https://www.runatlantis.io/) 。

但是状态存储在 Git 中吗？

GitOps 模型的关键基础事实的来源应该存储在 Git 中。

使用基础设施即代码(IAC)工具，如 [Terraform](https://www.terraform.io/) ，状态通常存储在外部源中，如 AWS 中的 S3 存储桶。

Terraform 仅在`terraform plan`或`terraform apply`被启动时获取/刷新状态。这可能会在部署到云的实际资源和 Terraform 在代码中声明的内容之间产生差异。

如果像 s3 buckets、ec2 实例或 RDS 数据库这样的云基础设施可以通过易于使用的 YAML 文件来声明，并将真实的来源存储在 Git 中，岂不是更好？

这类似于大多数用户在 Kubernetes 的应用程序级别上使用的部署模型。定义 Kubernetes 资源或舵图，以使用流行的 GitOps 控制器部署其应用程序，如 [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) 和 [Flux](https://fluxcd.io/) 。

好吧，让我给你介绍一个叫做[交叉平面](https://crossplane.io/)的工具。

Crossplane 是建立在流行的 Kubernetes 生态系统之上的控制平面。它作为[运营商](https://developers.redhat.com/articles/2021/06/22/kubernetes-operators-101-part-2-how-operators-work#)(广泛使用的 Kubernetes 框架)部署在一个引导集群上，并使用第三方提供商与流行的公共云提供商的 API(AWS、Azure、GCP)进行交互，以供应云资源。

工作流程可能包括。

*   创建一个有效的 [YAML](https://www.redhat.com/en/topics/automation/what-is-yaml) 清单来声明一个或多个云资源，定义它们的期望状态。
*   将清单签入所需的 Git 存储库，并创建一个 pull 请求以合并到主分支。
*   使用 GitOps 控制器，如 [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) 或 [Flux](https://fluxcd.io/) ，从存储库的主分支自动应用到引导集群。然后，这些文件作为自定义资源定义文件(CRD)存储在集群上，可以使用`kubectl`命令行查看。
*   然后，交叉平面操作员开始协调循环，资源被直接部署到您的云帐户。
*   对这些资源所做的任何更改，例如手动配置。Crossplane 将在下一个循环中修复这些更改，以达到清单的期望状态。
*   如果不再需要某个资源，则创建一个 pull 请求，从存储库中删除清单，CRD 将与云基础架构一起被销毁。

我提到最初需要一个引导集群，这是一个先有鸡还是先有蛋的场景。这可以在交叉平面工作流之外按照您喜欢的方式进行配置。

这可能是管理云基础架构部署的一种非常强大的方式。在这里，我们现在可以利用 GitOps 模型在存储库中定义我们的云环境的理想状态，作为清晰可读的代码。

所以现在问题来了。

如果基础设施现在很容易使用 GitOps 方法部署，开发人员能不能不写清单？
这会不会让所有平台和 DevOps 工程师失业？哎呀！

不完全是，Crossplane 被设计成允许平台团队为资源生命周期管理创建抽象 API。因此，开发人员可以用他们选择的参数请求或创建资源，而不需要知道支撑资源的复杂配置。

您可以通过创建以下资源来创建这种抽象和声明。

*   [复合资源定义](https://crossplane.io/docs/v1.9/concepts/terminology.html#composite-resource-definition) (XRD)用于定义新类型的复合资源和声明。您可以在这里创建抽象来隐藏实现细节，只暴露您希望最终用户配置的变量。
*   [复合资源声明](https://crossplane.io/docs/v1.9/concepts/terminology.html#composite-resource-claim) (XRC)一个声明对应一种复合资源。这是最终用户用来为资源定义所选变量的内容。

我发现这可能有点难以理解，但是[文档](https://crossplane.io/docs/v1.9/concepts/composition.html)写得很好，也很详细。

开发人员申请数据库的复合资源声明 (XRC)通常如下所示。

```
apiVersion: database.example.org/v1alpha1
kind: PostgreSQLInstance
metadata:
  name: my-db
  namespace: default
spec:
  parameters:
    storageGB: 20
  compositionSelector:
    matchLabels:
      provider: aws
      vpc: default
  writeConnectionSecretToRef:
    name: db-conn
```

开发人员现在可以轻松地部署/请求他们希望应用程序使用的云基础架构。

平台团队现在仍然可以控制部署的内容，并定义可以使用的资源和可用的有效配置选项。

赞成的意见

*   面向云基础设施的 GitOps 部署。
*   基础设施代码和实际部署到云中的内容之间不再有差异。真实状态的来源是在 Git 中存储和声明的内容。
*   跨团队协作对于开发人员来说很容易采用和请求基础设施。平台团队仍然可以控制所需的配置选项。
*   易于阅读的声明性 YAML 文件。
*   大型开源社区，是 [CNCF](https://landscape.cncf.io/) 的孵化项目。

骗局

*   引导集群需要在交叉面板工作流之外进行配置和维护。
*   并非每个提供商都支持所有的云资源。如果有你需要的东西，你应该为社区做贡献。
*   严重依赖 Kuberenets 作为未来几年的实际部署方法。

总之，如果您的团队选择采用交叉平面，您将使用尖端技术，这将带来一些挑战。作为回报，您将拥有一个使用日益流行的 GitOps 方法部署云资源的可靠方法。

这可能是基础设施即代码(IAC)的未来吗？
时间会证明，在这个不断变化的 DevOps 环境中，任何工具都取决于适应率和社区参与。