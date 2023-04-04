# Kubernetes 不赞成 Docker 的支持&这就是为什么你不应该恐慌

> 原文：<https://blog.devgenius.io/kubernetes-is-deprecating-docker-support-heres-why-you-should-not-panic-40259b108aff?source=collection_archive---------2----------------------->

## 为什么大家都在喊“Docker 死了！RIP，Docker！”？

![](img/09e628d32c8507f52da6e27d42009bc1.png)

照片由[泰国人莫莱斯](https://unsplash.com/@tata_morais?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/container?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

2020 年 12 月 2 日，Kubernetes 发表了一篇文章，声称 Kubernetes 是[贬低 Docker](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.20.md#deprecation) 作为一个容器运行时…我认为这不是一件大事，因为我通常从 [Kubernetes 博客](https://kubernetes.io/blog/)上阅读文章，虽然这只是一个发布更新博客。但是后来我看到 Kubernetes SIG 安全联席主席 Ian Coldwater 发了一条微博，这条微博引起了 DevOps 领域的恐慌。然后我意识到，这篇博文是为了澄清这条推文而写的。

*伊恩·科尔沃特在推特上写道*

> Docker 支持在 Kubernetes 中已被否决。你需要注意这一点，并为此做好计划。这将打破你的集群。

这在 DevOps 社区中引起了巨大的轰动，以至于她不得不删除之前的推文并发布这条推文。

伊恩·科尔德沃特在推特上道歉，但她的意图只是通知社区

# 那么，到底是什么呢？Docker 到底死没死？

不，码头工人没有死。你仍然可以继续和 Docker 一起工作。这没什么大不了的，它变成了。虽然有那么一瞬间，我也很困惑。

实际上，Kubernetes 的维护者解释说，他们在 v1.20 之后不赞成 Docker 作为容器运行时，*这是什么意思？*嗯，Docker 已经成为容器运行时的代名词。如果有人谈论容器映像、容器注册、甚至容器运行时，他们只会想到 Docker。Kubernetes 想要改变这种常态。

[](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/) [## 不要惊慌:Kubernetes 和 Docker

### 作者:豪尔赫卡斯特罗，达菲库利，凯特科斯格罗维，贾斯汀加里森，诺亚坎特罗威茨，鲍勃基伦，雷伊莱亚诺，丹“流行”…

kubernetes.io](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/) 

Docker 实际上是在 Kubernetes 之前引入的，目的是从整体上普及 Linux 容器模式。**这意味着 Docker 并不是周围唯一的容器运行时，还有其他类似** [**rkt**](https://coreos.com/rkt/) **，**[**container d**](https://containerd.io/)**，以及**[**lxd**](https://linuxcontainers.org/lxd/)**。**不知道的话，*容器运行时是在一个节点上执行容器和管理容器映像的软件。* Docker 非常流行，是生产 Kubernetes 环境中最常用的容器运行时。

总结一下，我看到了谷歌云平台开发人员倡导者 Kelsey Hightower 的一条令人难以置信的推文。

“码头工人！凯尔西·海托华的推特

# 但是…这重要吗？需要什么？

Kubernetes v1.5 引入了一个名为[容器运行时接口(CRI)](https://kubernetes.io/blog/2016/12/container-runtime-interface-cri-in-kubernetes) 的内部插件 API，以提供对不同容器运行时的轻松访问。 *CRI 使 Kubernetes 能够使用任何容器运行时，而无需重新编译。这意味着 Kubernetes 可以使用任何实现 CRI 的容器运行时来管理 pod、容器和容器映像。*

*Docker 目前不支持 Kubernetes 的 CRI* ，因此 Kubernetes 的维护人员编写了一个额外的层来支持这一点，这就是所谓的 **Dockershim。但是维护 dockershim 已经成为 Kubernetes 维护者的沉重负担。**

*库伯内特陈述，*

> …您的 Kubernetes 集群必须使用另一个名为 Dockershim 的工具来获得它真正需要的东西，即 containerd。这并不好，因为它给了我们另一个必须维护并且可能会损坏的东西。

**他们在另一篇关于** [**贬低 Dockershim**](https://kubernetes.io/blog/2020/12/02/dockershim-faq/) **的博文中发表了这篇文章。**因此，Kubernetes 的维护者鼓励开发人员迁移到兼容 CRI 的容器运行时。

好消息是，如果你正在使用 GKE、EKS 或 AKS ( [*，默认为 containerd*](https://containerd.io/) )这样的托管 Kubernetes 服务，你将需要确保你的工作节点正在使用受支持的容器运行时*，才能在未来版本的 Kubernetes 中移除 Docker 支持，目前计划在 2021 年末推出 1.22 版。*

我真的很喜欢写这篇文章，仅仅是因为我在 Ian 的第一条推文之后读了这么多推文。我添加了一首我最喜欢的歌曲供你欣赏。

如果你喜欢读这篇文章，你可能也会发现下面的文章值得你花时间去读。

[](https://medium.com/swlh/kubernetes-architecture-explained-in-brief-6a07f59193e) [## Kubernetes 建筑简介

### Kubernetes 已经成为最受欢迎的编排平台，因为它具有压倒性的特性，包括…

medium.com](https://medium.com/swlh/kubernetes-architecture-explained-in-brief-6a07f59193e) [](https://medium.com/the-innovation/kubernetes-deployments-in-5-minutes-7ac43eed80d6) [## kubernetes:5 分钟内部署完毕

### 简单地说，Kubernetes 部署负责创建、更新和删除您的…

medium.com](https://medium.com/the-innovation/kubernetes-deployments-in-5-minutes-7ac43eed80d6) [](https://medium.com/dev-genius/everything-a-developer-must-know-about-microservices-dae854782ab) [## 开发人员必须了解的关于微服务的一切

### 微服务是首选的应用平台，每个开发人员都必须了解它。

medium.com](https://medium.com/dev-genius/everything-a-developer-must-know-about-microservices-dae854782ab)