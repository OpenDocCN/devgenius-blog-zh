# 代码味道 75 —方法内部的注释

> 原文：<https://blog.devgenius.io/code-smell-75-comments-inside-a-method-c911f8729ee1?source=collection_archive---------3----------------------->

## *评论往往是一股代码味。将它们插入到方法中需要紧急重构。*

![](img/6da1351315777a3cf64ba7bd0bef1901.png)

> TL；不要在你的方法中添加注释。提取它们，只为不明显的设计决策留下声明性的注释。

# 问题

*   可读性
*   吻
*   低重用
*   错误的文档

# 解决方法

1.提取方法

2.重构

3.移除非声明性注释。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一种政策味道。每个 linter 都可以检测出不在第一行的评论并警告我们。

# 标签

*   可读性
*   长方法
*   评论

# 结论

评论是一种代码味道。如果您需要记录一个设计决策，您应该在实际的方法代码之前做。

# 关系

[](/code-smell-03-functions-are-too-long-accea7eb4ae9) [## 代码气味 03 —函数太长

### 人类过了 10 线就烦了。

blog.devgenius.io](/code-smell-03-functions-are-too-long-accea7eb4ae9) [](/code-smell-74-empty-lines-66d8d41d83e4) [## 代码气味 74 —空行

blog.devgenius.io](/code-smell-74-empty-lines-66d8d41d83e4) [](/code-smell-05-comment-abusers-feec3aeb926) [## 代码气味 05 —评论滥用者

### 代码有很多注释。注释与实现联系在一起，很难维护。

blog.devgenius.io](/code-smell-05-comment-abusers-feec3aeb926) 

> 不要被注释所迷惑，它们可能会产生严重的误导:只调试代码。

*戴夫·斯托勒*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)