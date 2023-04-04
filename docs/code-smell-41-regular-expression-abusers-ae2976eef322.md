# 代码气味 41 —正则表达式滥用者

> 原文：<https://blog.devgenius.io/code-smell-41-regular-expression-abusers-ae2976eef322?source=collection_archive---------3----------------------->

## 正则表达式是一个很好的工具，我们应该小心使用它们，不要显得太聪明。

![](img/e3272b4ff4ff4ffa990354db8ee22c1b.png)

照片由[约翰·詹宁斯](https://unsplash.com/@john_jennings)在 [Unsplash](https://unsplash.com/s/photos/letters) 上拍摄

# 问题

*   可读性
*   可维护性
*   易测性
*   意图揭示

# 解决方法

1.  将正则表达式仅用于字符串验证。
2.  如果你需要操作对象，不要把它们做成字符串。

# 示例代码

## 错误的

## 对吧

# 侦查

正则表达式是一种有效的工具。没有多少自动化的方法来检查可能的滥用者。白名单可能会有所帮助。

# 标签

*   原始痴迷
*   虐待者

# 结论

正则表达式是一个很好的字符串验证工具。我们必须以声明的方式使用它们，而且只能用于字符串。

名称对于理解模式含义非常重要。

如果我们需要操作对象或层次结构，我们应该用一种*对象方式*来完成。

除非我们有令人印象深刻的性能改进的决定性基准。

# 关系

[](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

medium.com](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

medium.com](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) 

# 更多信息

[](https://medium.com/dev-genius/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) [## 名字到底是什么？—第一部分:探索

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) 

> 如果一个 Perl 程序在你的老板解雇你之前完成了工作，那么它就是正确的。

*拉里·沃尔*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)