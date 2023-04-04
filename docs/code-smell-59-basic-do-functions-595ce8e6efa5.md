# 代码气味 59 —基本/执行功能

> 原文：<https://blog.devgenius.io/code-smell-59-basic-do-functions-595ce8e6efa5?source=collection_archive---------7----------------------->

## *sort，doSort，basicSort，doBasicSort，primitiveSort，superBasicPrimitiveSort，谁做真正的工作？*

![](img/b70a30aba1b30990fa075f11a787a351.png)

由[罗杰·布拉德肖](https://unsplash.com/@roger3010)在 [Unsplash](https://unsplash.com/s/photos/recursive) 上拍摄的照片

# 问题

*   可读性
*   错误的命名
*   低内聚力
*   单一责任原则

# 解决方法

1.  使用好的对象包装器
2.  使用动态装饰器

# 示例代码

## 错误的

## 对吧

# 侦查

我们可以指导我们的静态棉绒找到包装方法，如果它们遵循像*doxx()*， *basicXX()* 等约定的话。

# 标签

*   声明性

# 结论

在我们的开发生涯中，我们曾经遇到过这种方法，我们感觉它们有些不对劲。现在是改变它们的时候了！

# 更多信息

[](https://maximilianocontieri.com/what-exactly-is-a-name-part-ii-rehab) [## 名字到底是什么？-第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。我们都用名字编程，它…

maximilianocontieri.com](https://maximilianocontieri.com/what-exactly-is-a-name-part-ii-rehab)  [## 包装函数

### 包装函数是软件库或计算机程序中的子程序(函数的另一种说法),它…

en.wikipedia.org](https://en.wikipedia.org/wiki/Wrapper_function) [](https://en.wikipedia.org/wiki/Decorator_pattern) [## 装饰图案

### 在面向对象编程中，装饰模式是一种设计模式，它允许将行为添加到

en.wikipedia.org](https://en.wikipedia.org/wiki/Decorator_pattern) 

> Wrap 方法的主要缺点是它会导致糟糕的名称。在前面的例子中，我们重命名了支付方法 dispatchPay()，因为我们需要在原始方法中为代码取一个不同的名称。

*迈克尔羽毛*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)