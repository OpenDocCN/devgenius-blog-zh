# 气味代码 51 —双重否定

> 原文：<https://blog.devgenius.io/code-smell-51-double-negatives-993b6160f3e1?source=collection_archive---------2----------------------->

## *不是操作员是我们的朋友。不是不是操作员不是我们的朋友*

![](img/faea3df122c2fd94e490e2bdbdcf1dc1.png)

由[丹尼尔·赫伦](https://unsplash.com/@herrond)在 [Unsplash](https://unsplash.com/s/photos/no) 上拍摄

# 问题

*   可读性

# 解决方法

1.  用积极的名字命名你的变量、方法和类。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一种语义气味。我们需要在代码审查中发现它。

我们可以告诉 linters 检查像*这样的正则表达式！不是*也不是*！不是*等作为警告。

# 标签

*   可读性

# 结论

双重否定是我们作为初级开发人员学到的一个非常基本的规则。

很多生产系统都充满了这种味道。

我们需要信任我们的测试覆盖率，并进行安全的重命名和其他重构。

# 关系

[](https://medium.com/dev-genius/code-smell-24-boolean-coercions-1594cc1be008) [## 代码气味 24 —布尔强制

### 布尔应该只有真和假

medium.com](https://medium.com/dev-genius/code-smell-24-boolean-coercions-1594cc1be008) [](https://medium.com/dev-genius/code-smell-07-boolean-variables-260bef4d1146) [## 代码气味 07 —布尔变量

### 使用布尔变量作为标志，暴露了意外的实现，并用 Ifs 污染了代码。

medium.com](https://medium.com/dev-genius/code-smell-07-boolean-variables-260bef4d1146) [](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

medium.com](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) 

# 更多信息

[](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1)  [## 去除双重否定

### 你有双重否定条件。使它成为一个单一的肯定的有条件的双重否定经常被…

refactoring.com](https://refactoring.com/catalog/removeDoubleNegative.html) [](https://levelup.gitconnected.com/knot-of-nots-avoiding-negative-names-for-boolean-methods-641896a94a42) [## 打结:避免布尔方法的负面名称

### 命名返回布尔值的方法的更好做法

levelup.gitconnected.com](https://levelup.gitconnected.com/knot-of-nots-avoiding-negative-names-for-boolean-methods-641896a94a42) 

> 读代码比写代码更难。

乔尔·斯波尔斯基

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)