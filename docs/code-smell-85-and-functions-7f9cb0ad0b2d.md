# 代码气味 85 —和功能

> 原文：<https://blog.devgenius.io/code-smell-85-and-functions-7f9cb0ad0b2d?source=collection_archive---------4----------------------->

## 不要执行超过要求的内容。

![](img/6529818f75d5ee04e24f13b4ea5334a7.png)

[保罗](https://unsplash.com/@causeimluap)在 [Unsplash](https://unsplash.com/s/photos/train) 上的照片

> TL；DR:除非你需要原子性，否则不要执行一个以上的任务。

# 问题

*   连接
*   违反单一责任原则
*   可读性
*   低内聚力
*   易测性

# 解决方法

1.  中断功能

# 示例代码

## 错误的

## 对吧

# 侦查

包括“和”的函数是候选函数。然而，我们需要仔细检查它们，因为可能会有假阳性。

# 标签

*   可读性
*   命名

# 结论

我们应该避免做多余的事情，我们的功能应该是最小的和原子的。

# 更多信息

[](/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

blog.devgenius.io](/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) 

# 信用

这种气味的灵感来自于

> 如果用一句以上的话来解释你正在做的事情，这几乎总是表明你正在做的事情太复杂了。

*山姆奥特曼*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)