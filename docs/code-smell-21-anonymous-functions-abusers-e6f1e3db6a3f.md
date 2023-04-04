# 代码气味 21 —匿名函数滥用者

> 原文：<https://blog.devgenius.io/code-smell-21-anonymous-functions-abusers-e6f1e3db6a3f?source=collection_archive---------2----------------------->

函数、λ、闭包。所以高阶，非声明，和热。

![](img/7c29efeebcd6e25d3c25cc10aa435d2f.png)

由[罗马法师](https://unsplash.com/@roman_lazygeek)在 [Unsplash](https://unsplash.com/s/photos/math) 上拍摄的照片

# 问题

*   可维护性
*   易测性
*   代码重用
*   实现隐藏
*   排除故障

# 解决方法

1.  包装函数/闭包
2.  在方法对象/策略中具体化算法

# 示例代码

## 错误的

## 对吧

# 侦查

*   闭包和匿名函数对于建模*代码块*、*承诺*等非常有用。所以很难把它们分开。

# 标签

*   原始的
*   虐待者

# 结论

人类阅读代码。使用匿名函数时，软件工作正常，但是当调用多个闭包时，可维护性会受到损害。

# 关系

[](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

medium.com](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) 

> 面向对象编程通过管理这种复杂性增加了这些指标的价值。处理复杂性最有效的工具是抽象。可以使用许多类型的抽象，但是封装是面向对象编程中管理复杂性的主要抽象形式。

*丽贝卡·沃斯-布洛克*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)