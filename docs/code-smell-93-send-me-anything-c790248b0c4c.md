# 气味代码 93——发送任何信息给我

> 原文：<https://blog.devgenius.io/code-smell-93-send-me-anything-c790248b0c4c?source=collection_archive---------3----------------------->

## 可以接收许多不同的(而不是多态的)参数的神奇函数

![](img/8ed4faaa79f54e473ea5caa6382d8547.png)

亨妮·斯坦德在 [Unsplash](https://unsplash.com/s/photos/juggler) 上的照片

> TL；DR:创建一个清晰的合同。预计只有一个协议。

# 问题

*   失败快速原则违反
*   容易出错
*   可读性
*   [如果造成污染](https://mcsee.medium.com/how-to-get-rid-of-annoying-ifs-forever-317033474484)
*   [空值](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0)
*   凝聚力差

# 解决方法

1.只接受一种输入

2.参数应该遵循单一协议。

# 示例代码

## 错误的

## 对吧

# 侦查

当它们做不同的事情时，我们可以检测到这种方法，要求参数*种类。*

# 标签

*   如果污染者

# 结论

魔法铸件和灵活性是有代价的。他们把垃圾放在地毯下，违反了快速原则。

# 关系

[](/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) [## 代码味道 69 —大爆炸(JavaScript 荒谬的铸件)

blog.devgenius.io](/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) [](/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

blog.devgenius.io](/code-smell-06-too-clever-programmer-bffec35daf0b) [](/code-smell-12-null-64fbd7792a7c) [## 代码气味 12 —空

### 程序员使用 Null 作为不同的标志。它可以提示缺席、未定义的值、错误等。

blog.devgenius.io](/code-smell-12-null-64fbd7792a7c) 

> 引用透明性是一个非常可取的属性:它意味着函数在给定相同输入的情况下始终产生相同的结果，而不管它们在何时何地被调用。

爱德华·加森

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)