# 如何准备你的 Kubernetes 面试

> 原文：<https://blog.devgenius.io/how-to-prepare-for-your-kubernetes-interview-e8947f99fe3d?source=collection_archive---------9----------------------->

由[谢恩撰写显示](https://www.linkedin.com/in/shaneshown/)， [Nxt 级别的首席执行官](https://www.nxtlevel.io/)

![](img/7f6161585d9a545a1c0c172145c7755d.png)

你是一个 DevOps 工程师还是一个站点可靠性工程师，需要为了面试而复习你的 Kubernetes 知识？日常操作中有实践知识，然后是面试。“面试”是一项独特的技能。所以，让我们来谈谈在你即将到来的 Kubernetes 聚焦面试中可能会出现的一些面试问题。

# Kubernetes 在高水平上的核心特征是什么？

据 [VMware](https://www.vmware.com/topics/glossary/content/kubernetes.html) 透露，未来包括以下内容:

Kubernetes 有许多特性，可以帮助跨多个主机编排容器，自动管理 K8s 集群，并通过更好地利用基础设施来最大限度地利用资源。据[报道，VMware](https://www.vmware.com/topics/glossary/content/kubernetes.html) 的重要功能包括:

1.  **自动缩放**。根据使用情况自动扩展容器化的应用程序及其资源
2.  **生命周期管理**。自动化部署和更新，能够:A)回滚以前的版本。b)暂停并继续展开
3.  **声明式模型**。声明所需的状态，K8s 在后台维护该状态并从任何故障中恢复
4.  **韧性和自愈能力**。自动放置、自动重启、自动复制和自动扩展提供了应用程序自我修复功能
5.  **持久存储**。能够动态装载和添加存储
6.  **负载均衡**。Kubernetes 支持各种内部和外部负载平衡选项，以满足不同的需求
7.  发展合作支持。DevSecOps 是一种高级的安全性方法，它简化并自动化了跨云的容器操作，集成了整个容器生命周期的安全性，并使团队能够更快地交付安全、高质量的软件。结合 DevSecOps 实践和 Kubernetes 提高了开发人员的生产力。

# 什么是 Kubernetes 架构组件？

Kubernetes 集群的主要组件包括:

**节点:**节点是托管容器化应用程序的虚拟机或物理服务器。群集中的每个节点可以运行一个或多个应用程序实例。可以只有一个节点，但是一个典型的 Kubernetes 集群会有几个节点(有数百个或更多节点的部署并不少见)。

**图像注册表**:容器图像保存在注册表中，并由控制平面传送到节点，以便在容器盒中执行。

**pod**:pod 是运行容器化应用程序的地方。它们可以包含一个或多个容器，是 Kubernetes 集群中应用程序的最小部署单元。

# 附加 Kubernetes 术语

了解作为控制平面的一部分或者在 Kubernetes 节点上执行的主要 K8s 组件的名称和功能非常重要。

控制平面有四个主要组件，用于控制通信、管理节点和跟踪 Kubernetes 集群的状态。

*   **Kube-apiserver** 。顾名思义，kube-apiserver 公开了 Kubernetes API。
*   **etcd** 。存储与 Kubernetes 集群相关的所有数据的键值存储。
*   **Kube-调度器**。监视没有分配节点的新 Kubernetes Pods，并根据资源、策略和“关联性”规范将它们分配给一个节点执行。
*   **Kube-控制者-管理者。**控制平面的所有控制器功能都编译成一个二进制文件:kube-controller-manager。

K8s 节点有三个主要组件:

*   **库伯莱**。确保 Kubernetes Pod 中运行必要容器的代理。
*   **Kube-proxy** 。在群集中的每个节点上运行的网络代理，用于维护网络规则并允许通信。
*   **容器运行时**。负责运行容器的软件。Kubernetes 支持任何符合 Kubernetes CRI(容器运行时接口)的运行时。

需要了解的其他术语包括:

*   Kubernetes 服务公司。Kubernetes 服务是一组执行相同功能的 Kubernetes Pods 的逻辑抽象。Kubernetes 服务被分配了唯一的地址，即使 pod 实例来来去去，这些地址也保持不变。
*   **控制器。**控制器确保 Kubernetes 集群的实际运行状态尽可能接近期望状态。
*   **操作员**。Kubernetes 操作符允许您为应用程序封装特定领域的知识，类似于一本操作手册。通过自动化特定于应用程序的任务，运营商允许您更轻松地在 K8s 上部署和管理应用程序。

# 什么是容器编排？

[容器编排](https://www.vmware.com/topics/glossary/content/container-orchestration.html)自动化运行容器化工作负载和服务所需的大部分任务，包括对 Kubernetes 容器生命周期至关重要的操作:供应、部署、扩展、联网和负载平衡。

# Kubernetes 对 Docker

经常被误解为非此即彼的选择，Kubernetes 和 Docker 是运行容器化应用程序的不同但互补的技术。

Docker 可以让你把运行应用程序所需的所有东西放进一个盒子里，这个盒子可以在需要的时候随时随地打开。一旦你开始打包你的应用程序，你需要一种方法来管理它们；这就是 Kubernetes 所做的。

Kubernetes 是一个希腊词，在英语中是“船长”的意思。就像船长负责船只在海上的安全航行一样，Kubernetes 负责将这些箱子安全地运送到可以使用的地方。

1.  Kubernetes 可以使用或不使用 Docker
2.  Docker 不是 Kubernetes 的替代品，所以这不是一个“Kubernetes 与 Docker”的问题。它是关于使用 Kubernetes 和 Docker 来封装你的应用程序并大规模运行它们
3.  Docker 和 Kubernetes 之间的区别在于它们在应用程序的容器化和运行中所扮演的角色
4.  Docker 是一个用于在容器中打包和分发应用程序的开放式行业标准
5.  Kubernetes 使用 Docker 来部署、管理和扩展容器化的应用程序

# 潜在的面试问题—尤里卡的博客

尤里卡博客展示了大量的常见问题。使用此链接查看他们所有的 [50 个常见问题](https://www.edureka.co/blog/interview-questions/kubernetes-interview-questions/)。

# Kubernetes 和 Docker Swarm 有什么不同？

![](img/a064da633e9e229e2fe38c73b77ba4ce.png)

# 什么是 Kubernetes？

![](img/1e189296af3bad30355fdf2194f9ef3e.png)

图 1:什么是 Kubernetes——Kubernetes 面试问题

Kubernetes 是一个开源的容器管理工具，负责容器部署、容器的伸缩和负载平衡。作为谷歌的创意，它提供了一个优秀的社区，并与所有的云提供商合作愉快。因此，我们可以说 Kubernetes 不是一个容器化平台，但它是一个多容器管理解决方案。

# 在主机和容器上部署应用程序有什么区别？

![](img/d764ffc4a78187185c884435c514bce3.png)

图 2:在主机和容器上部署应用程序——Kubernetes 访谈问题

参考上图。左侧架构表示在主机上部署应用程序。因此，这种架构将有一个操作系统，然后操作系统将有一个内核，该内核将在操作系统上安装应用程序所需的各种库。因此，在这种框架中，您可以有许多应用程序，并且所有应用程序都将共享该操作系统中的库，而在容器中部署应用程序时，架构略有不同。

这种架构将有一个内核，这是所有应用程序之间唯一的共同点。因此，如果有一个特定的应用程序需要 Java，那么这个特定的应用程序将可以访问 Java，如果有另一个应用程序需要 Python，那么只有这个特定的应用程序可以访问 Python。

您可以在图的右侧看到的各个块基本上都是容器化的，因此与其他应用程序隔离开来。因此，应用程序拥有与系统其余部分隔离的必要库和二进制文件，不能被任何其他应用程序侵占。

# 容器编排的需求是什么？

假设您有 5–6 个微服务用于执行各种任务的单个应用程序，所有这些微服务都放在容器中。现在，为了确保这些容器相互通信，我们需要容器编排。

![](img/4a52879de3a5d194100de2d506c3ca11.png)

图 3:没有容器编排的挑战——Kubernetes 访谈问题

正如您在上图中看到的，在没有使用容器编排的情况下，也出现了许多挑战。因此，为了克服这些挑战，容器编排应运而生。

# Kubernetes 有什么特点？

Kubernetes 的特点如下:

![](img/7a55ce14f6e9efe96b2b0c493cd934d4.png)

图 4:Kubernetes 的特点——Kubernetes 面试问题

# Kubernetes 建筑有哪些不同的组成部分？

Kubernetes 架构主要有两个组件——主节点和工作者节点。正如您在下图中看到的，主节点和工作节点中有许多内置组件。主节点有 kube 控制器管理器、kube API server、kube 调度器等。而 worker 节点在每个节点上运行 kubelet 和 kube-proxy。

![](img/810f044c04e85edbbdcc7c50faeec175.png)

图 7:Kubernetes 的架构——Kubernetes 面试问题

# 这里是其他内容的资源中心:

D2IQ —小抄—【https://d2iq.com/resources/category/cheat-sheet 

Kube by:特色学习路径—【https://kubebyexample.com/ 

Kubernetes 开源文档:[https://kubernetes.io/docs/concepts/overview/](https://kubernetes.io/docs/concepts/overview/)

谷歌的“是什么”解释:【https://cloud.google.com/learn/what-is-kubernetes 

微软的“是什么”解释:[https://azure . Microsoft . com/en-us/topic/what-is-kubernetes/#概述](https://azure.microsoft.com/en-us/topic/what-is-kubernetes/#overview)

VMware 的“是什么”解释:[https://www . VMware . com/topics/glossary/content/kubernetes . html](https://www.vmware.com/topics/glossary/content/kubernetes.html)

Ubuntu 的“是什么”解释:[https://ubuntu.com/kubernetes/what-is-kubernetes](https://ubuntu.com/kubernetes/what-is-kubernetes)