# 5 种最有效的集装箱安全技术

> 原文：<https://blog.devgenius.io/5-most-effective-techniques-for-container-security-187fb6bdbb8a?source=collection_archive---------13----------------------->

ontainers 有着巨大的影响力，并在整个计算机行业被迅速采用。然而，它们真的像我们想象的那样安全吗？

这里有 5 种你需要用来保护你的集装箱和保证你的基础设施安全的技术。

> “观众越多，责任越大。开发人员和架构师必须小心保护和监控他们在分布式系统上的工作负载，几乎都是在公共云环境中。”
> 
> — Renato Losio，首席云架构师兼 InFOQ 发言人。

![](img/024113dad37b0c8e7d85e8f93554b6e7.png)

[伊恩·泰勒](https://unsplash.com/@carrier_lost?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/containers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

# 1.扫描您的图像

最明显的第一步是确保您使用的图像不包含任何固有的漏洞。由于 Docker 是在分层的基础上工作的，所以准确地知道您的平台上有哪些软件和版本是一个挑战。扫描图像可以揭示这些漏洞。如果您使用 Docker，我们可以简单地使用 [scan](https://docs.docker.com/engine/scan/) 命令来完成。

```
**$ docker scan hello-world**Testing hello-world...Organisation:  docker-desktop-test
Package manager: linux
Project name: docker-image|hello-world
Docker image: hello-world
Licences: enabledTested 0 dependencies for known issues, no vulnerable paths found.
```

虽然在 Docker Hub 上找到与您的用例相匹配的公开图像可能很容易，但重要的是要注意它们来自哪里。Docker 提供了许多高质量的图片，由经过验证的出版商维护。我建议从值得信赖的供应商*那里选择带有*最小额外软件包*的基本映像。*

# 2.使用安全工具

Kubernetes(和其他容器编排程序)包含内置的安全工具，可以确保您的平台是安全的。

然而，我们可能想看看一些第三方工具，以获得一些额外的保护。它们可以在开发和生产环境中扫描我们的容器，并应用运行时应用程序自我保护(RASP)。RASP 是一种安全技术，用于保护我们的应用程序免受攻击，快速识别威胁并加以防范。

这些工具通常还附带有用的功能，用于网络和漏洞扫描和检测、事件响应和测试。以下是我最喜欢的几个:

*   [Anchore](https://anchore.com/) —针对容器环境或云的安全性和合规性解决方案。
*   [Aqua Security](https://www.aquasec.com/) —云原生安全工具，非常适合 Kubernetes 和 Docker 专用环境。
*   [帕洛阿尔托](https://www.paloaltonetworks.com/prisma/cloud/container-security) —最适合需要高度网络可见性和安全性的大规模云应用。

# 3.安全部署环境

仅仅因为我们运行容器，并不意味着我们不应该抛弃我们正常的安全实践。

我们可以通过多种方式做到这一点，例如设置防火墙规则、限制帐户访问以及强化容器下的主机。在构建平台之前，请确保删除主机上运行的任何额外的不必要的服务。

# 4.运行时的安全性

基于云的平台或任何使用容器的解决方案都应该确保在运行时将通信限制在最低限度。应该只暴露需要开放连接的端口和应用程序。并且应尽可能适用最低特权原则。

> 要在运行时保护容器，您应该跨工作负载监控和检测网络流量、文件活动、进程行为和系统调用中的异常，以便全面了解运行时威胁
> —蒂皮涅尼

# 5.监控，监控，监控…

把最好的留到最后— **监控你的应用是目前保护它的最好方法**。使用 ELK 和 Prometheus 等工具，您可以快速查看集装箱性能的关键指标。我们还可以使用外部编排工具来设置复杂的规则，在高工作负载下检查漏洞、错误和容器。

感谢阅读。如果你觉得这篇文章有用，别忘了鼓掌。

我定期在 DevOps 上发布专门在 Medium 上发布的文章——如果你想阅读更多，我建议看看下面的故事。

[](https://medium.com/@joelbelton/optimising-docker-performance-the-key-4-techniques-you-need-6440cfebb650) [## 优化 Docker 性能—您需要的 4 项关键技术

### Docker 集装箱和集装箱化技术已经永远改变了云计算产业。成千上万的新…

medium.com](https://medium.com/@joelbelton/optimising-docker-performance-the-key-4-techniques-you-need-6440cfebb650) [](https://medium.com/@joelbelton/learning-containers-from-the-ground-up-a-roadmap-to-success-dd16151b2388) [## 从头开始学习容器——通往成功的路线图

### 容器化已经成为软件开发中的一个重要趋势，并且是一种快速增长的替代方式。

medium.com](https://medium.com/@joelbelton/learning-containers-from-the-ground-up-a-roadmap-to-success-dd16151b2388) 

如果你喜欢这篇文章，而你还不是会员，可以考虑通过 [**注册成为会员**](https://medium.com/@joelbelton/membership) 来支持我和其他成千上万的作家，并无限制地访问 Medium 的优秀作家的内容。你的会员资格直接用你的一部分费用支持我，不会花你更多的钱。

[](https://medium.com/@joelbelton/membership) [## 通过我的推荐链接加入 Medium-Joel Belton

### 阅读乔尔·贝尔顿(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接支持…

medium.com](https://medium.com/@joelbelton/membership)