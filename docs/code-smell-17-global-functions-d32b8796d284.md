# 代码味道 17 —全局函数

> 原文：<https://blog.devgenius.io/code-smell-17-global-functions-d32b8796d284?source=collection_archive---------1----------------------->

许多混合语言不支持面向对象编程，而是支持它。开发者滥用它们。

![](img/8cc5fa5b2d50b95e177a470816f9fe62.png)

照片由[梅姆](https://unsplash.com/@picoftasty)在 [Unsplash](https://unsplash.com/s/photos/spaghetti) 上拍摄

# 问题

*   连接
*   可读性
*   可维护性
*   易测性

# 解决方法

*   将函数包装在上下文对象中。

# 例子

*   外部资源访问、数据库访问、时间和操作系统资源。

# 示例代码

## 错误的

## 对吧

# 侦查

许多现代语言都避免使用它们。对于许可的规则，可以应用范围规则并自动检查。

# 标签

*   全球的

# 结论

结构化编程认为全局函数是有害的。然而，我们可以观察到一些跨范式边界的不良实践。

# 关系

*   单体和类是全局访问点。

[](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 辛格尔顿:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) 

# 更多信息

*   [维基百科](https://en.wikipedia.org/wiki/Global_variable)

> 通往编程地狱的道路布满了全局变量。

史蒂夫·麦康奈尔

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)