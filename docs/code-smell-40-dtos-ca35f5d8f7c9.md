# 代码气味 40—dto

> 原文：<https://blog.devgenius.io/code-smell-40-dtos-ca35f5d8f7c9?source=collection_archive---------1----------------------->

## dto 被广泛使用，它们“解决”了实际问题，不是吗？

![](img/ccabc22c82e4debe9ebfebc6962cdabe.png)

# 问题

*   贫血的对象
*   不一致的数据
*   重复逻辑
*   重复结构
*   等级污染
*   信息隐藏违规
*   代码在[变异器](https://en.wikipedia.org/wiki/Mutator_method)、存取器、[串行化器](https://en.wikipedia.org/wiki/Serialization)、[解析器](https://en.wikipedia.org/wiki/Parsing)中重复
*   涟漪效应
*   数据完整性

# 解决方法

1.  在数组上传输不足的数据。
2.  使用真实的业务对象。
3.  如果我们要转移部分对象:使用代理或空对象来中断引用图。

# 示例代码

## 错误的

## 对吧

# 侦查

我们可以用同样的贫血物体探测器。

我们可以检查没有业务对象行为的*贫血的*类(移除序列化、构造函数、赋值函数等)。

# 标签

*   贫血的

# 结论

在某些语言中，dto 是一种工具和惯例。我们应该小心谨慎地使用它们。

如果我们需要分解我们的物体以便把它们送出我们的领域，我们需要非常小心。因为被肢解的对象没有完整性考虑。

他的作者警告我们关于它的实际滥用。

# 关系

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [](https://medium.com/dev-genius/code-smell-13-empty-constructors-8fa705fb3f33) [## 代码味道 13 —空构造函数

### 不完整的对象会导致很多问题。

medium.com](https://medium.com/dev-genius/code-smell-13-empty-constructors-8fa705fb3f33) [](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

medium.com](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) [## 气味代码 28 —设定器

### 初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

medium.com](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) 

# 更多信息

[](https://martinfowler.com/bliki/LocalDTO.html) [## bliki:localdo

### 如果你一直在关注我的思想博客们，你会知道我的一个鸟人似乎已经吹了…

martinfowler.com](https://martinfowler.com/bliki/LocalDTO.html) [](https://refactoring.guru/es/smells/data-class) [## 数据类

### 数据类是指只包含字段和访问它们的简单方法(getters 和 setters)的类…

重构大师](https://refactoring.guru/es/smells/data-class) [](https://softwareengineering.stackexchange.com/questions/171457/what-is-the-point-of-using-dto-data-transfer-objects) [## 使用 DTO(数据传输对象)有什么意义？

### 尽管 DTO 不是一个过时的模式，但它经常被不必要地应用，这可能会使它显得过时。来自 Java…

softwareengineering.stackexchange.com](https://softwareengineering.stackexchange.com/questions/171457/what-is-the-point-of-using-dto-data-transfer-objects) 

> 最好的气味是容易被发现的，大多数时候会引导你找到真正有趣的问题。数据类(只有数据没有行为的类)就是很好的例子。你看着他们，问自己在这个班级应该有什么样的行为。

马丁·福勒

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)