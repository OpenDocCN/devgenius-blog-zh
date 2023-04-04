# 代码气味 102 —箭头代码

> 原文：<https://blog.devgenius.io/code-smell-102-arrow-code-3e9301e83269?source=collection_archive---------1----------------------->

## 嵌套的 if 和 Elses 很难阅读和测试

![](img/5af71e9745912c76e010e1f428bb5bb6.png)

> TL；DR:避免嵌套的 if。更好的是:避免所有如果

# 问题

*   可读性

# 解决方法

1.提取方法

2.组合布尔条件

3.移除意外 if

# 语境

在过程代码中，很容易看到复杂的嵌套 if。这与脚本的关系比面向对象编程更大。

# 示例代码

错误的

## 对吧

# 侦查

[X]自动

由于许多 linters 可以解析树，我们可以在编译时检查嵌套层次。

# 标签

*   可读性
*   复杂性

# 结论

遵循鲍勃叔叔的建议，我们应该让代码比我们发现时更干净。

重构这个问题很容易。

# 关系

[](/code-smell-78-callback-hell-aeb4a3010270) [## 代码气味 78 —回调地狱

### 将算法作为一系列嵌套回调来处理并不明智。

blog.devgenius.io](/code-smell-78-callback-hell-aeb4a3010270) [](/code-smell-03-functions-are-too-long-accea7eb4ae9) [## 代码气味 03 —函数太长

### 人类过了 10 线就烦了。

blog.devgenius.io](/code-smell-03-functions-are-too-long-accea7eb4ae9) [](/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b) [## 代码气味 36 — Switch/case/elseif/else/if 语句

### 第一节编程课:控制结构。高级开发人员的教训:避免它们。

blog.devgenius.io](/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b) 

# 更多信息

*   [C2 维基](http://wiki.c2.com/?ArrowAntiPattern)
*   [展平箭头代码](https://blog.codinghorror.com/flattening-arrow-code/)

> 软件工程的目的是控制复杂性，而不是创造复杂性。

帕米拉·扎夫

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)