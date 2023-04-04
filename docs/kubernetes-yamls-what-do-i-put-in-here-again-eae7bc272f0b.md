# kubernetes YAMLs——我在这里又放了什么？

> 原文：<https://blog.devgenius.io/kubernetes-yamls-what-do-i-put-in-here-again-eae7bc272f0b?source=collection_archive---------11----------------------->

![](img/0a6f5fbddbc4187b1d1bd55a1795e033.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [James Wainscoat](https://unsplash.com/@tumbao1949?utm_source=medium&utm_medium=referral) 拍摄

随着某些软件架构成为标准化的“最佳实践”，使用 Kubernetes 来管理和协调多容器或分布式应用程序变得越来越普遍和必要这使得学习如何使用 Kubernetes(或 K8s)成为所有 web 开发人员的一项重要技能，以便至少获得用于故障排除的基本工作知识。

无论您是刚刚完成第一个教程，还是经验丰富的高级 K8s 管理员，在处理 k8 设置时，有一项工作永远不会消失——填写配置 YAML 文件。我想，理论上，您可以强制地从命令行手动创建每个对象，以避免编写 yaml 的恐惧，但随着您的应用程序规模的扩大，这变得不可能，您需要依赖控制平面自动化来让应用程序甚至可以在许多用户的情况下运行。

无论您写了多少次基本的样板部署配置，都不可能在下一次制作时准确地记住需要什么。它们也不是这些疯狂的难以理解的文档，它们在阅读时非常有意义——它们只是字面上描述了部署/服务/您需要的任何东西的理想工作状态，k8s 会为您完成其余的工作。然而，这些 yaml 文件中包含了某种心理魔法，使得细节像筛子一样从你的脑海中流出。

到目前为止，我用来写出所有配置的主要技术是 Googling 我试图创建的特定对象，并复制默认的配置文档，为我的用例添加必要的信息。[官方部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) [和服务文档](https://kubernetes.io/docs/concepts/services-networking/service/)有大量使用，其他[各种控制器](https://kubernetes.io/docs/concepts/workloads/controllers/) [和服务 I](https://kubernetes.io/docs/concepts/services-networking/) 可能需要使用。在基于 Google 写出第一个部署、服务和入口或检查我的一些旧应用程序后，我通常只是根据需要在我的 infra 目录中复制它们，并更改名称和细节。

# 新的解决方案

Octopus Deploy 的人已经创建了一个非常方便的参考页面，可以在设置 yaml 配置时节省你很多时间:[Kubernetes Yaml 生成器！](https://k8syaml.com/)通过方便易读的下拉菜单，您可以浏览 k8s 配置的所有选项。每个选项都有一个方便的描述，以及相关文档的链接以获取更多信息。当您更改设置时，它会生成完整的 yaml，使它成为您创建所有配置文件的实时、最新、交互式方式。

也许这个工具可以从我们在 k8s 努力中陷入的无休止的 yaml 谷歌搜索中提供急需的缓解！

 [## 库伯内特 YAML 发电机

### Octopus Deploy 是一个用于 AWS、Azure、.NET，Java，Kubernetes，Windows 和 Linux，还有一个…

k8syaml.com](https://k8syaml.com/) [](https://kubernetes.io/docs/concepts/) [## 概念

### 概念部分帮助您了解 Kubernetes 系统的各个部分以及 Kubernetes 用来…

kubernetes.io](https://kubernetes.io/docs/concepts/)