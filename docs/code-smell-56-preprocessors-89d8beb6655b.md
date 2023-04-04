# 代码气味 56 —预处理器

> 原文：<https://blog.devgenius.io/code-smell-56-preprocessors-89d8beb6655b?source=collection_archive---------3----------------------->

我们希望我们的代码在不同的环境和操作系统下表现不同，所以在编译时做出决定是最好的决定，不是吗？。

![](img/4107969b3c99894201f7d57cd414316d.png)

[疾控中心](https://unsplash.com/@cdc)在 [Unsplash](https://unsplash.com/s/photos/customs) 上拍摄的照片

# 问题

*   可读性
*   过早优化
*   不必要的复杂性
*   排除故障

# 解决方法

1.  移除所有编译器指令。
2.  如果你想要不同的行为，用对象建模
3.  如果你认为会有性能损失，那就做一个严肃的基准测试，而不是过早的优化。

# 示例代码

# 错误的

# 对吧

# 侦查

这是几种语言提倡的语法指令，因此很容易检测并替换为实际行为。

# 标签

*   编译程序
*   元编程

# 结论

增加一层额外的复杂性会使调试变得非常困难。这种技术在内存和 CPU 不足的时候使用。如今，我们需要干净的代码，我们必须将过早的优化埋在过去。

比雅尼·斯特劳斯特鲁普在他的书*c++*中，对他几年前创建的预处理器指令表示遗憾。

# 关系

[](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

medium.com](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) 

# 更多信息

[](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) [## 懒惰 I:元编程

### 元编程是神奇的。这是我们不应该使用它的主要原因。有许多可怕的后果…

codeburst.io](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6)  [## 你是说预处理程序是邪恶的吗？，C++常见问题

### 来自马歇尔·克莱恩:比雅尼·斯特劳斯特鲁普、赫伯·萨特、安德烈·亚历山德雷斯库、皮尔逊/爱迪生-韦斯利出版社和我…

www.parashift.com](http://www.parashift.com/c++-faq-lite/preprocessor-is-evil.html) [](https://en.wikipedia.org/wiki/C_preprocessor) [## c 预处理程序

### C 预处理器或 cpp 是 C、Objective-C 和 C++计算机编程语言的宏预处理器。的…

en.wikipedia.org](https://en.wikipedia.org/wiki/C_preprocessor) 

> C++旨在让你表达想法，但如果你没有想法或不知道如何表达想法，C++不会提供太多帮助。

*比雅尼·斯特劳斯特鲁普*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)