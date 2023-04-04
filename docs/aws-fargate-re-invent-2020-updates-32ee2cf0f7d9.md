# AWS Fargate-re:发明 2020 更新

> 原文：<https://blog.devgenius.io/aws-fargate-re-invent-2020-updates-32ee2cf0f7d9?source=collection_archive---------3----------------------->

在 AWS Fargate 上运行容器的新更新&与其他云产品的比较。

![](img/8a42f1d862fc9b8c8dffde4c81ad555c.png)

图片来源: [ECS 工作坊](https://ecsworkshop.com/introduction/whyfargate/)

到目前为止，re:Invent 2020 在容器轨道方面的亮点是宣布 [ECS/EKS anywhere](https://aws.amazon.com/eks/eks-anywhere/) 将使用 AWS 在内部部署容器。这一声明强调了 AWS 拥抱多重/混合云的主题，反映了我在“[为什么 BigQuery Omni 很重要](https://medium.com/dataseries/why-bigquery-omni-is-a-big-deal-e7e696b4cd60)”中提到的 GCP 的战略。

与此同时，AWS 还发布了 AWS Fargate 的一些新更新，这是一个在 EKS/ECS 上运行容器的无服务器产品。在本文中，我们将介绍 AWS 容器产品，重点介绍 Fargate 及其新特性，并将它与其他云供应商的同类产品进行比较。

# AWS Fargate

AWS 根据管理(ECS 和 EKS)和托管(EC2 和 Fargate)来细分其容器产品:

![](img/4937e2b7327d793a17ab542f7f729292.png)

与解决容器编排的 ECS 和 EKS 不同，AWS Fargate 提供了一个无服务器的计算引擎来部署容器，并让 AWS 管理底层基础设施，类似于 AWS Lambda 对函数的处理。与任何无服务器产品一样，使用 Fargate 的主要优势是底层基础设施(在本例中是运行容器所需的计算)由 AWS 管理。简而言之，AWS 将匹配正确的实例类，并根据需要相应地扩展集群容量。

![](img/893e0ca237af30549c7dce284e9132ae.png)

## 新的 Fargate 特性

迄今为止，Fargate 的主要目标是为现有的 ECE/EKS 用户运行无状态应用程序。许多关于 Fargate 的客户评价涵盖了 Fargate 简化了许多 DevOps 需求的用例，即使团队在 ECE 或 EKS 上使用 Spot 实例优化了结构。

为此，支持 AWS 批处理作业和 Fluent Bit 的声明建议进一步支持具有大数据或分析需求的团队简化他们的工作流并获得更多见解。

**新更新**:

*   亚马逊 EFS 支持持久存储
*   暂时存储增加到 20GB
*   默认情况下加密的临时存储
*   通过容器洞察和任务元数据展现的网络性能指标
*   支持 CAP_SYS_PTRACE
*   Containerd 作为容器运行时(替换了 Docker)
*   针对服务配额的使用指标
*   增加了默认并发任务/pod 服务配额
*   [Fargate(ECS)的 AWS 批处理作业](https://aws.amazon.com/blogs/aws/new-fully-serverless-batch-computing-with-aws-batch-support-for-aws-fargate/)
*   [流畅的钻头支持](https://aws.amazon.com/blogs/containers/fluent-bit-for-amazon-eks-on-aws-fargate-is-here/)

**仍不支持:**

*   特许集装箱
*   添加或删除除 CAP_SYS_PTRACE 之外的 Linux 功能
*   Amazon EBS 对持久存储的支持
*   Windows 操作系统
*   ARM 架构
*   绘图处理器
*   EKS 的守护天使

考虑到这些限制，如果您需要管理状态并且仍然希望利用 Fargate，请确保容器与其他 AWS 服务(例如 DynamoDB、RDS、ElastiCache)集成。为了绕过 DaemonSets 对 EKS 的限制，为每个 pod 模仿 sidecar 模式的行为，因为 Fargate 支持多容器 pod。

# 与 GCP 和 Azure 产品的比较

Fargate 不是唯一的无服务器容器服务:GCP 有[云运行](https://cloud.google.com/run)和 Azure 提供[容器实例](https://azure.microsoft.com/en-us/services/container-instances/#features)。虽然他们三个都在引擎盖下使用 Kubernetes，但他们的方法略有不同。

*注:如果您想要全面比较托管 Kubernetes 产品，请查看“* [*托管 Kubernetes 2020*](https://medium.com/swlh/state-of-managed-kubernetes-2020-4be006643360#:~:text=According%20to%20Cloud%20Native%20Computing,and%20GCP%20leading%20the%20pack.)*”*

## 谷歌云运行

谷歌云用户既可以将 Cloud Run 作为类似于云功能的独立 PaaS 产品使用，也可以将其与现有的 GKE 集群集成，类似于 EKS 的 Fargate。GKE 混合云用户也可以在 Anthos 上使用 Cloud Run。Cloud Run 建立在 [Knative 开源项目](https://knative.dev/)的基础上，这意味着 AWS 用户可以使用像本指南这样的[示例设置来模拟行为。](https://github.com/mreferre/knative-on-fargate)

与 Fargate 相比，Cloud Run 的主要优势是对自动扩展的现成支持，包括将 pod 扩展到零。这类似于 GKE 在默认情况下安装集群和水平自动缩放，而不是 AWS 让其 EKS 用户配置自动缩放行为。对于 Anthos 用户来说，Cloud Run 也有 GPU 支持，非常适合运行无状态的大数据作业。Cloud Run 也是唯一支持部署版本控制和通过容器图像标签路由流量的产品。

另一方面，云运行不允许多容器 pod。如果您使用 sidecar 模式或多容器，您需要单独部署每个服务。好的一面是，Cloud Run 可以在几秒钟内部署这些容器，并与其他谷歌云产品无缝集成。

## Azure 容器实例

Azure 在 Fargate 和 Cloud Run 之前于 2018 年发布了容器实例。容器实例利用轻量级虚拟机实现隔离，并使用可选的公共 IP 地址部署容器。VM 级隔离类似于 Fargate，但是可选的公共 IP 地址是一个很好的特性，如果没有 ALBs，Fargate 是不支持的。

除了 Windows 容器支持(这是 Azure 所期望的)，容器实例的不同之处在于不支持本地 Kubernetes 清单。相反，它符合类似于 Kubernetes pod 规范的按容器组分组的 Azure 实现。然而，通过开源 Kubelet 实现 [Virtual Kubelet](https://virtual-kubelet.io/) ，Azure 容器实例用户可以将容器实例虚拟机伪装成附属于 AKS 或其他 Kubernetes 集群的 Kubernetes 节点。Azure 容器实例的另一个很好的特性是通过 [Azure 文件共享](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-overview#persistent-storage)的本地持久存储支持。 [GPU 支持](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-gpu)(NVIDIA Tesla GPU)目前正在预览中。

# 外卖食品

AWS 向 Fargate 发布新功能，表明 AWS 承诺改善其容器产品线。然而，与 Cloud Run 和 Azure Container 实例相比，它缺乏关键功能(如 GPU 支持、原生自动缩放)以及流畅的开发人员入职体验。除非您已有 ECS 或 EKS 集群，否则 Fargate 不会像 Cloud Run 一样作为独立的 PaaS 产品运行，也不会在不创建 ALB 来路由流量的情况下分配公共 IP。尽管存在缺点，但如果您正在寻找一个托管的无服务器容器计算，或者希望为您的无状态应用程序卸载一些 DevOps 负担，Fargate 值得在 AWS 生态系统上看看。