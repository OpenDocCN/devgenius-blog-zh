# Java 之后如何写仙丹

> 原文：<https://blog.devgenius.io/how-to-write-elixir-after-java-7f3b42663afd?source=collection_archive---------1----------------------->

## 如何忘记课，遇到模块

![](img/1ab30733b16a2a5b54760c1be110b7f3.png)

照片由[卡斯帕·卡米尔·鲁宾](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 模块>>类

在 Spring 中，你必须重启服务器，但是在这里一切都在控制之中。模块拥有可以测试、修改和重新编译的函数。类是分离责任的，模块是分离关注的。

BEAM 是用于执行这些功能的操作系统进程。编译后的模块文件有一个 BEAM 扩展名，它位于 build 文件夹中。模块的伟大之处在于`recompile`函数，所以你可以在你的`iex`中重新编译模块。如果你使用 Java 代码，你会发现这个特性非常有用。

你在项目结构上有所有的自由。模块名用点分隔，只是为了帮助我们理解结构。在 Java 包中没有严格的命名。

没有遗产！什么都不受保护！你有公共函数和私有函数，你还需要什么:)

## 函数>>方法

你只需要`def`你的功能，你就可以开始了。酏剂是特定于它的流水线功能。您可以轻松地在函数之间传递结果。

功能简单，干净，优雅。您可以将代码分成几个函数。模式匹配来处理多种情况，从而避免 if-else 代码。

![](img/9325eccd66ccdc2728cbdc4cb2d64f4b.png)

照片由 [Shahadat Shemul](https://unsplash.com/@shemul?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

您必须使用 getters 来获取对象的属性。模式匹配使得获取属性成为一行程序。

## 控制

![](img/37d44f9ec062e2ad436a6dda3eeea963.png)

Byron Sterk 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在 Elixir 中创建流程太棒了！长生不老药使它们重量轻，可观察，容错。`:observer`可以为您提供实时流程所需的所有信息。

在主管的帮助下，你可以微调你的流程。只要你愿意，进程就会一直存在。

Spring container 可以在您需要的地方提供 beans，您只需调用函数。这两个原则对我们隐藏了实现，使我们的生活更容易。

![](img/5226f2c2284d62add0493075054c595c.png)

&give_some_claps/0