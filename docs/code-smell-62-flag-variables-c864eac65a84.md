# 代码气味 62 —标志变量

> 原文：<https://blog.devgenius.io/code-smell-62-flag-variables-c864eac65a84?source=collection_archive---------6----------------------->

## 旗帜表明发生了什么。除非他们的名字太普通了。

![](img/ed61fff4a1800c2de8c0fd6bbe39f6fe.png)

# 问题

*   可读性
*   可维护性
*   连接

# 解决方法

1.  使用有意义的名称
2.  尽量避免旗帜。它们产生耦合。

# 示例代码

## 错误的

## 对吧

# 侦查

我们可以在所有代码中搜索错误的命名标志。

# 标签

*   可读性

# 结论

标志在生产代码中广泛存在。我们应该限制他们的使用，并使用明确和意图揭示的名称。

# 关系

[](https://medium.com/dev-genius/code-smell-51-double-negatives-993b6160f3e1) [## 气味代码 51 —双重否定

### 不是操作员是我们的朋友。不是不是操作员不是我们的朋友

medium.com](https://medium.com/dev-genius/code-smell-51-double-negatives-993b6160f3e1) [](https://medium.com/dev-genius/code-smell-07-boolean-variables-260bef4d1146) [## 代码气味 07 —布尔变量

### 使用布尔变量作为标志，暴露了意外的实现，并用 Ifs 污染了代码。

medium.com](https://medium.com/dev-genius/code-smell-07-boolean-variables-260bef4d1146) 

# 更多信息

[](https://en.wikipedia.org/wiki/Boolean_flag) [## 布尔标志

### 计算机科学中的布尔标志、真值位或真值标志是一个布尔值，用一个位表示，一个字节可以…

en.wikipedia.org](https://en.wikipedia.org/wiki/Boolean_flag) [](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) 

> 如果你对编译器撒谎，它会报复的。

*亨利·斯潘塞*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)