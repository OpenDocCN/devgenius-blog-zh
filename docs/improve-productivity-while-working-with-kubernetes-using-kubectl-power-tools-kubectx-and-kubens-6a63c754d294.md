# 在使用 kubectl 电动工具 Kubectx 和 kubens 与 Kubernetes 一起工作时提高生产力

> 原文：<https://blog.devgenius.io/improve-productivity-while-working-with-kubernetes-using-kubectl-power-tools-kubectx-and-kubens-6a63c754d294?source=collection_archive---------3----------------------->

![](img/c3f2e02fad8bbac04c6110bb7cd5c45b.png)

# 背景

如果您是 DevOps 或站点可靠性工程师(SRE)工程师，您很可能会使用多个集群。您还可能在这些集群中使用多个名称空间。使用 **kubectl** 命令行工具时，在上下文和名称空间之间切换可能会非常耗时。

# kubectl 电动工具

Kubernetes 现在非常流行，人们使用 **kubectl** 作为命令行界面来使用 Kubernetes 集群是很常见的。大多数人更喜欢使用 kubectl，而不是基于 GUI 的工具。

对于 SRE 和 DevOps 工程师以及使用多个集群的开发人员来说，例如开发、分级/ UAT 和生产环境，他们需要切换上下文。在许多企业中，Kubernetes 集群由不同的团队共享。为了隔离工作负载，使用了 kubernetes **名称空间**。每次执行任何命令时，都需要提供名称空间。

在这种情况下，在同一个集群上的名称空间之间切换以及切换整个上下文以指向不同的集群可能是一项繁琐的任务。这就是我在使用 kubectl 时遇到的两个电动工具， **kubectx** 和 **kubens** 。

在视频中，我们可以看到如何安装 kubectx 和 kubens 以及模糊搜索(FZF)工具的互动模式。

[](https://www.youtube.com/watch?v=nGznJeOH4ws?WT.mc_id=AZ-MVP-5003170) [## kubectl 电动工具 kubectx 和 kubens

### 这个视频是关于两个有用的实用工具 kubectx 和 kubens，它们有助于大大提高生产力，同时…

www.youtube.com](https://www.youtube.com/watch?v=nGznJeOH4ws?WT.mc_id=AZ-MVP-5003170) 

# 结论

使用 kubectx 和 kubens 有助于在使用 kubectl 时提高生产率。我们不需要记住冗长的上下文名称以及集群中每个名称空间的名称。使用这些电动工具可以提高工作效率。

直到下一次，激情编码，精益求精。

*原载于 2021 年 8 月 24 日 https://www.handsonarchitect.com*[](https://www.handsonarchitect.com/2021/08/improve-productivity-while-working-with.html)**。**