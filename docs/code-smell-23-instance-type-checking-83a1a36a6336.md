# 代码味道 23 —实例类型检查

> 原文：<https://blog.devgenius.io/code-smell-23-instance-type-checking-83a1a36a6336?source=collection_archive---------3----------------------->

你检查过你在和谁说话吗？

![](img/85acb21d35b2e9499e2e702bbf6b65e3.png)

由[雷米·吉林](https://unsplash.com/@gieling)在 [Unsplash](https://unsplash.com/s/photos/assembly-line) 上拍摄的照片

# 问题

*   [联轴器](https://mcsee.hashnode.dev/coupling-the-one-and-only-software-design-problem)
*   元模型干扰
*   [if](https://mcsee.hashnode.dev/how-to-get-rid-of-annoying-ifs-forever)

# 解决方法

1.  避免*类*，*类*，*实例*， *getClass()* ，*类*等..
2.  不要对域对象使用反射和[元编程](https://mcsee.hashnode.dev/laziness-i-meta-programming)。
3.  用多态替换 [*IFs*](https://mcsee.hashnode.dev/how-to-get-rid-of-annoying-ifs-forever) 。
4.  避免检查*‘未定义’*。使用[完成对象](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3)，避免[空值](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c)和设置器，支持[不变性](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e)，你将永远不会有未定义的和如果。

# 示例代码

## 错误的

## 对吧

# 侦查

因为类型检查方法是众所周知的，所以很容易建立一个代码策略来检查使用。

# 标签

*   元编程

# 结论

对一个类类型[的测试将](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04)对象与[偶然决策](https://medium.com/dev-genius/no-silver-bullet-65cba1775f9b)结合起来，并且违反了[双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)，因为在现实世界中不存在这样的控制。这是一种气味，我们的模型不够好。

# 关系

[](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c) [## 代码气味 12 —空

### 程序员使用 Null 作为不同的标志。它可以提示缺席、未定义的值、错误等。

medium.com](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c) 

# 更多信息

[](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484) [## 如何永远摆脱烦人的 if

### 为什么我们学习编程的第一条指令应该是最后使用的。

medium.com](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484) [](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) [## 懒惰 I:元编程

### 元编程是神奇的。这是我们不应该使用它的主要原因。有许多可怕的后果…

codeburst.io](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)