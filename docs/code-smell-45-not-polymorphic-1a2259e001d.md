# 代码气味 45 —非多态

> 原文：<https://blog.devgenius.io/code-smell-45-not-polymorphic-1a2259e001d?source=collection_archive---------7----------------------->

## 如果方法做的一样，它们应该是可互换的。

![](img/0f5715e0114864a04057a69768265ae5.png)

# 问题

*   缺失多态性
*   连接
*   IFs /类型检查污染
*   耦合到类型的名称。
*   偏爱多态性。

# 解决方法

1.  根据方法的作用来重命名方法。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一个语义错误。我们可以为多态类上的*相似的*方法名添加一个警告。

# 标签

*   多态的

# 结论

命名很重要。我们需要以概念命名，而不是以偶然的类型命名，

# 关系

[](https://medium.com/dev-genius/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b) [## 代码气味 36 — Switch/case/elseif/else/if 语句

### 第一节编程课:控制结构。高级开发人员的教训:避免它们。

medium.com](https://medium.com/dev-genius/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b) 

# 更多信息

[](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) 

> 如果你有三只宠物狗，给它们起名字。如果你有一万头牛，那就别麻烦了。现在，给你电脑上的每个文件起一个名字的想法是荒谬的。

*大卫·杰伦特*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)