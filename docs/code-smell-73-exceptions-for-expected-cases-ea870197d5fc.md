# 代码气味 73 —预期情况的例外

> 原文：<https://blog.devgenius.io/code-smell-73-exceptions-for-expected-cases-ea870197d5fc?source=collection_archive---------5----------------------->

## 例外情况是方便的 Gotos 和 flags。让我们虐待他们。

![](img/d3b7ef59d7fb2cda049629c90b0967c2.png)

由[格雷格·罗森克](https://unsplash.com/@greg_rosenke)在 [Unsplash](https://unsplash.com/s/photos/bounds) 上拍摄的照片

> TL；DR:不要对流量控制使用异常。

# 问题

*   可读性
*   最小破坏原则。

# 解决方法

1.只在意外情况下使用异常。

2.异常处理[合同违约](https://en.wikipedia.org/wiki/Design_by_contract)。看合同。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一种语义气味。除非我们使用机器学习棉绒，否则很难找到错误。

# 标签

*   可读性

# 结论

异常很方便，我们应该明确地使用它们，而不是返回代码。

正确使用和错误使用之间界限就像许多设计原则一样模糊不清。

# 关系

[](/code-smell-72-return-codes-1b869fb53f54) [## 代码气味 72 —返回代码

blog.devgenius.io](/code-smell-72-return-codes-1b869fb53f54) [](/code-smell-26-exceptions-polluting-9246aca40234) [## 气味代码 26 —污染例外

### 有许多不同的例外是非常好的。您的代码是声明性的和健壮的。还是没有？

blog.devgenius.io](/code-smell-26-exceptions-polluting-9246aca40234) 

# 更多信息

[https://wiki.c2.com/?DontUseExceptionsForFlowControl](https://wiki.c2.com/?DontUseExceptionsForFlowControl)

[](https://dzone.com/articles/exceptions-as-controlflow-in-java) [## 为什么应该避免使用异常作为 Java - DZone Java 中的控制流

### 了解更多关于在 Java 中使用异常作为控制流的信息。Java 是一种通用编程语言，有许多…

dzone.com](https://dzone.com/articles/exceptions-as-controlflow-in-java) [](https://softwareengineering.stackexchange.com/questions/189222/are-exceptions-as-control-flow-considered-a-serious-antipattern-if-so-why) [## 异常作为控制流被认为是严重的反模式吗？如果有，为什么？

### 我认为回答这个问题最简单的方法是理解 OOP 这些年来取得的进步。所有事情都发生在…

softwareengineering.stackexchange.com](https://softwareengineering.stackexchange.com/questions/189222/are-exceptions-as-control-flow-considered-a-serious-antipattern-if-so-why) [](https://stackoverflow.com/questions/729379/why-not-use-exceptions-as-regular-flow-of-control) [## 为什么不使用异常作为常规的控制流？

### 为了避免所有我可以搜索到的标准答案，我将提供一个你们可以随意攻击的例子。C#和…

stackoverflow.com](https://stackoverflow.com/questions/729379/why-not-use-exceptions-as-regular-flow-of-control) 

> 调试时，新手插入纠错代码；专家删除有缺陷的代码。

*理查德·帕蒂斯*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)