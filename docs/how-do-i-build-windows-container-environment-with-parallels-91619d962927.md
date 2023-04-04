# 如何使用 Parallels 构建 Windows 容器环境

> 原文：<https://blog.devgenius.io/how-do-i-build-windows-container-environment-with-parallels-91619d962927?source=collection_archive---------0----------------------->

![](img/86d36cb2f5c889d80a8466a4bcdf81bf.png)

卢多维克·沙雷特在 [Unsplash](https://unsplash.com/s/photos/limit?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

我是一名 DevOps 工程师，我的工作是编写和管理运行在 container 和 Kubernetes 上的软件。唯一的另一件事是，我经常使用 Windows 服务器而不是 Linux。是的。过去，我是一名 Windows 桌面应用程序开发人员，一名 ASP.NET 开发人员，现在我是一名 DevOps 工程师。

如果您使用的是 MacBook 或 iMac，如果您正在开发任何类型的 Windows 应用程序，您可能会使用 Parallels 或类似的虚拟化软件。如你所知，相干模式是有益的。我非常喜欢并行模式中的相干模式。(VMware Fusion 也提供了一个类似的功能，称为 Unity，但它看起来很丑，速度很慢。😭)

Windows 在容器技术方面发生了巨大的变化。这是其历史上的一次巨大飞跃，非常棒。但是，尽管有了这种巨大的飞跃，Windows 并不是 Linux，它的局限性是显而易见的。

目前，Windows 不支持直接从主机操作系统运行不同版本的容器映像。这种限制使得虚拟机管理程序用户的处境更加艰难，因为这种情况涉及虚拟化本身。因此，这意味着您应该使用支持嵌套虚拟化的虚拟机管理程序。

# 痛苦的嵌套虚拟化

嵌套虚拟化通常不是正确的解决方案，因为在许多情况下，它会延长运行时间或产生非常不稳定的结果。即使您购买了 Parallels 或 VMware Fusion 的 Pro 版本，也可能会失败。

新兵训练营怎么样？Boot camp 将你的 Mac 变成了一台出色的 Windows PC，但它迫使你必须启动 Windows 操作系统。另外，Mac 上的 Windows 运行起来很笨拙。我不喜欢 Windows 上内置触摸板的工作方式。😇

此外，微软和苹果都不保证 Windows 体验的质量。Windows 以后的版本在 Bootcamp 环境下越来越差。

最后，如果你想知道为什么我只在本地机器上解决这个问题，我会告诉你。我想把我的重点放在我的 MacBook 上，而不是云上。我不想为此付钱。

那么我该如何打破这种困扰呢？这很简单。我制作了一个专用于 Windows Docker 机器的辅助虚拟机。然后我用 Docker 上下文连接了 VM。

# 限制

尽管如此，这不是一个完美的解决方案，但它可以是 b 计划。例如，您可能会对 docker 卷和挂载本地文件夹感到困惑。有时我忘记了我在远程 Docker 主机上工作，浪费了我的时间。😇

此外，如果因为 Windows Kubernetes 而使用多个版本的 Windows，可能会很麻烦。每次你都应该检查启动的虚拟机并改变上下文。

但是，如果没有嵌套虚拟化，这可能是最佳答案。此外，Hyper-V 隔离容器，即使没有嵌套虚拟化，基本上也很慢。(尤其是在 docker build 命令中。)

# 结论

> 使用或不使用嵌套虚拟化。这是个问题。