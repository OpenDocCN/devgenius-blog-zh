# 2021 年值得关注的技术和工具

> 原文：<https://blog.devgenius.io/technologies-tools-to-watch-in-2021-a216dfc30f25?source=collection_archive---------0----------------------->

为开发运维工程师和 sre 评估的技术意见列表

![](img/0ded5b83400cf5b16277946e2f0feacc.png)

制造者在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [NESA 的照片](https://unsplash.com/@nesabymakers?utm_source=medium&utm_medium=referral)

# 通过 Kubernetes CRDs 管理云服务

所有三个主要的云提供商(AWS/Azure/GCP)现在都支持通过自定义资源定义(CRD)来供应和管理 Kubernetes 的云服务。AWS 在开发者预览版中有针对 Kubernetes (ACK) 的 [AWS 控制器；Azure 最近推出了](https://github.com/aws/aws-controllers-k8s) [Azure 服务运营商](https://github.com/Azure/azure-service-operator)(不赞成为 Azure 开放服务代理)；GCP 有[配置连接器](https://cloud.google.com/config-connector/docs/overview)作为 GKE 的附件。虽然 Terraform、Ansible 和 Puppet 等基础设施即代码(IaC)工具仍被广泛用于管理云基础设施，但对 Kubernetes 管理的云服务的支持表明，组织正朝着将 Kubernetes 作为云基础设施焦点的方向发生巨大转变。这里的好处是，开发人员现在可以使用相同的工具来管理 Kubernetes 应用程序和其他使用 Kubernetes APIs 的云服务，这可能会简化工作流程。然而，根据您当前的基础架构工作流或 Kubernetes 的专业知识，Kubernetes 和您的云工作负载的这种紧密耦合可能并不理想。

# 普鲁米

说到 IaC 工具， [Pulumi](https://www.pulumi.com/) 最近宣布了其[3750 万美元的 B 轮融资](https://www.pulumi.com/blog/series-b/)以挑战 Terraform 在该领域的主导地位。与传统的 IaC 产品不同，Pulumi 选择让开发人员用他们喜欢的语言(如 Go、Python、Javascript)编写基础设施代码，而不是推出另一种基于 JSON/YAML 的特定领域语言。这种选择使 Pulumi 比 Terraform 更加灵活，并使开发人员能够利用现有的测试框架来验证他们的基础设施。然而，考虑到它的污秽，普鲁米的社区与地形相比是相当小的。

# Terragrunt & TFSEC

与 Pulumi 不同，Terraform 通过其开源社区解决了一些不足。Terragrunt 是围绕 Terraform 的一个薄薄的包装器，通过将配置组织成版本化的模块来帮助团队管理大型 Terraform 项目。Terragrunt 实现了由 [Gruntwork](https://gruntwork.io/) 联合创始人 Yevgeniy Brikman 提出的一些最佳实践。虽然 Terragrunt 是完全开源的，但 Gruntwork 最近宣布[为寻求更多生产就绪服务的企业提供商业支持](https://terragrunt.gruntwork.io/commercial-support/)。TFSEC 是另一个补充 Terraform 项目的开源工具。它使用静态分析来标记基础设施代码的潜在安全威胁。随着安全性越来越多地融入到 DevSecOps 运动中，像 tfsec 这样的工具在未来将变得更加重要。

# 泰克顿

CI/CD 市场充斥着 Jenkins 和 Spinnaker 等成熟工具以及 ArgoCD 等新兴的云原生工具。 [Tekton](https://tekton.dev/) 是这一领域的新玩家，专注于 Kubernetes 工作负载。Tekton 最初是 Knative 项目的一部分，后来捐赠给了持续交付基金会(CDF)。Tekton 的与众不同之处在于它通过 Kubernetes CRDs 定义了管道。这使得管道可以继承原生 Kubernetes 特性(例如回滚)，并且还可以与现有工具(例如 Jenkins X 或 ArgoCD)集成，以支持复杂的端到端 CI/CD 管道。

# 琐碎

对容器的弱点扫描正在成为任何 CI/CD 管道的重要部分。像 CI/CD 市场一样，有大量开源和商业工具，包括用于安全的[Docker Bench](https://github.com/docker/docker-bench-security)、 [Clair](https://github.com/quay/clair) 、 [Cilium](https://github.com/cilium/cilium) 、 [Anchore Engine](https://github.com/anchore/anchore-engine) 和 [Falco](https://github.com/falcosecurity/falco) 。Trivy 是 Aqua Security 的一个工具，它不仅扫描容器，还扫描代码中的底层包。结合 Aqua Security 的 [kube-bench](https://github.com/aquasecurity/kube-bench) ，组织可以更轻松地将安全性融入应用程序开发工作流。

# 外壳检查

尽管基础设施工具领域有了巨大的改进，但是 shell 脚本仍然存在于各种工作流中，以完成简单的任务。 [ShellCheck](https://github.com/koalaman/shellcheck) 是一个静态分析工具，用于 lint shell 脚本的语法和常见错误。ShellCheck 可以从 web、终端/CI 以及您喜欢的文本编辑器(例如 Vim、Sublime、Atom、VS Code)中运行。

# Pitest/Stryker

[Pitest](http://pitest.org/) (Java)和 [Stryker](https://stryker-mutator.io/) (Javascript，C#，Scala)都在各自的语言中实现了变异测试。突变测试通过向测试中注入错误，并检查即使发生突变，测试是否仍然通过，来衡量测试的质量。当测试用例中出现突变时，一个好的单元测试应该失败。突变测试补充了测试覆盖率，以检测未测试和未充分测试的代码。

# 石蕊

早在 2011 年，网飞用混沌猴推广了混沌工程，混沌猴是猿猴军队工具套件的一部分。在 Kubernetes 的世界里，有大量的混沌工程工具如 [chaoskube](https://github.com/linki/chaoskube) 、 [kube-monkey](https://github.com/asobti/kube-monkey) 、 [PowerfulSeal](https://github.com/powerfulseal/powerfulseal) 以及商业平台如 [Gremlin](https://www.gremlin.com/kubernetes/) 。我想强调一下 [Litmus](https://github.com/litmuschaos/litmus) 是一个成熟的混沌工程解决方案，可扩展且易于使用。Litmus 是一个轻量级 Kubernetes 操作符，由 ChaosEngine、ChaosExperiment 和 ChaosResult 组成。Litmus 支持细粒度的实验，这些实验不仅仅是简单地杀死名称空间中的随机 pod，而是通过混沌结果 CRD 显示结果，而不是将可观察性留给用户。

还有其他我关注的技术和趋势(例如零信任架构、微前端、服务网格工具)，但由于缺乏经验而被忽略了。如果有其他工具或趋势我错过了，欢迎在下面评论。