# 代码味道 19 —可选参数

> 原文：<https://blog.devgenius.io/code-smell-19-optional-arguments-c0714855dbbb?source=collection_archive---------1----------------------->

伪装成友好的快捷方式是另一种耦合气味。

![](img/793a796acb4d0c716c770c2ee403944d.png)

# 问题

*   连接
*   意外的结果
*   副作用
*   涟漪效应
*   在有可选参数但仅限于基本类型的语言中，我们需要设置一个标志，并添加一个偶然的 IF(另一种味道)。

# 解决方法

1.  使论点明确。
2.  所有函数调用必须有相同的 [arity](https://en.wikipedia.org/wiki/Arity) 。
3.  如果你的语言支持，使用*命名参数*。

# 示例代码

## 错误的

## 对吧

# 侦查

如果语言支持可选参数，检测就很容易。

# 标签

*   可选择的
*   怠惰

# 结论

明确一点。比起更短(耦合度更高)的函数调用，更注重可读性。

# 更多信息

*   [函数 Arity](https://en.wikipedia.org/wiki/Arity)
*   [解耦遗留系统](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642)

[](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) [## 如何分离遗留系统

### 改进遗留代码的练习

medium.com](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) 

> 程序员的问题在于，你永远无法知道一个程序员在做什么，直到为时已晚。

*西摩·克雷*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)