# 代码气味 52 —易碎测试

> 原文：<https://blog.devgenius.io/code-smell-52-fragile-tests-b917de33fd53?source=collection_archive---------8----------------------->

## 测试是我们的安全网。如果我们不信任他们的正直，我们将处于极大的危险之中。

![](img/18b6c816902d8ee0ee1aeffa6cfc1e33.png)

由[吉尔博特·易卜拉希米](https://unsplash.com/@jilburr)在 [Unsplash](https://unsplash.com/s/photos/glass-broken) 上拍摄的照片

# 问题

*   决定论
*   信心丧失
*   浪费的时间

# 解决方法

1.  测试应该完全受控。不应该有古怪行为和自由度的空间。
2.  拆除所有测试联轴器。

[](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [## 耦合:唯一的软件设计问题

### 对我们软件的所有故障进行根本原因分析，会发现一个有多种伪装的单一罪魁祸首。

codeburst.io](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) 

# 例子

脆弱、间歇、零星或不稳定的测试在许多组织中很常见。

然而，他们挖掘了开发者的信任。

我们必须避开他们。

# 示例代码

## 错误的

## 对吧

# 侦查

检测可以通过测试运行统计来完成。

因为我们正在拆除一个安全网，所以很难在维护中进行一些测试。

# 更多信息

[](https://softwareengineering.stackexchange.com/questions/109703/how-to-avoid-fragile-unit-tests) [## 如何避免脆弱的单元测试？

### 我也有这个问题。我改进的方法如下:不要写单元测试，除非它们是唯一的…

softwareengineering.stackexchange.com](https://softwareengineering.stackexchange.com/questions/109703/how-to-avoid-fragile-unit-tests) 

# 标签

*   连接
*   决定论

# 结论

脆弱测试显示系统耦合，而不是确定性或不稳定的行为。

开发人员花费大量的时间和精力来对抗这种误报。

> 业余软件工程师总是在寻找神奇的东西。

格雷迪·布奇

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)