# Docker:我为什么要用？

> 原文：<https://blog.devgenius.io/docker-why-i-had-to-use-it-2cac1264a869?source=collection_archive---------31----------------------->

## Docker 在技术领域一直很受欢迎。它的使用量激增。没有容器化的新应用程序感觉缺少了什么。这篇文章详细介绍了在你的开发环境中使用 Docker 的原因。

![](img/fc22c53bc2efa13ab2817d9736b31f54.png)

Docker 对于开发环境非常有用，主要有三个原因:

## 1.它跑哪都行！！

如果你已经正确地对你的应用进行了对接，并且它在你的机器上运行没有问题，那么 99%的时间里，它将在任何地方顺利运行:在你朋友的设备上，在暂存环境上，也在生产环境中。

## 2.从第一天开始就富有成效

几年前，在 Docker 之前，当一个新的团队成员加入时，他花了一天多的时间为机器安装合适的操作系统，设置语言，并在其上添加数据库。仅仅是正确设置环境就浪费了两三天时间。

但是现在，安装 docker，然后运行 3 或 5 个命令，喝点咖啡，然后变魔术:你的应用程序正在运行。新加入者可以在第一天就对工作代码做出贡献。

想想一家公司通过这种方法可以节省的所有成本。🤔

## 3.测试新版本兼容性的简单方法

想象一下，你使用的语言的新版本刚刚发布。好像你用的是 Java 8，14 出来了。您不知道要使您的应用程序兼容新的语言版本需要做多少工作。

这里用 docker 您需要运行两个不同的 docker 容器，一个运行当前版本，另一个运行新版本。你甚至可以并排测试应用程序来衡量性能。

Docker 对软件工程师和 DevOps 工程师来说都是福音。这使得发布软件变得更加容易，因为整个堆栈都已发布，而不仅仅是代码。尽管如此，要将您的产品转移到 Docker，还需要考虑许多事情。比如容器构建器，容器编排。

✉ [***保持联系吧！报名我的每周简讯***](https://softwaretipsandtricks.substack.com/)

**❤如果你喜欢这篇文章，你可能也会喜欢:**

[](https://medium.com/quick-code/spring-boot-how-to-design-efficient-search-rest-api-c3a678b693a0) [## Spring Boot:如何设计高效的搜索 REST API？

### 不要努力工作，更聪明地工作

medium.com](https://medium.com/quick-code/spring-boot-how-to-design-efficient-search-rest-api-c3a678b693a0) [](https://medium.com/quick-code/useful-git-alias-926f27a27f92) [## 有用的 git 别名

### 众所周知，开发人员希望优化和简化重复性任务。我们喜欢走捷径，少打字…

medium.com](https://medium.com/quick-code/useful-git-alias-926f27a27f92)