# 代码味道 57 —版本化函数

> 原文：<https://blog.devgenius.io/code-smell-57-versioned-functions-d548b7f1b481?source=collection_archive---------7----------------------->

## sort，sortOld，sort20210117，workingSort，把它们都有了太好了。以防万一

![](img/9002df19e24cd395ece2ee68990bfe9a.png)

[K8](https://unsplash.com/@k8_iv) 在 [Unsplash](https://unsplash.com/s/photos/onion) 上拍照

# 问题

*   可读性
*   可维护性

# 解决方法

1.  只保留工件的一个工作版本(类、方法、属性)。
2.  将时间控制留给您的版本控制系统。

# 示例代码

## 错误的

## 对吧

# 侦查

我们可以添加自动规则来寻找带有模式的版本化方法。

像许多其他模式一样，我们可以创建一个内部策略并进行交流。

# 标签

*   版本控制

# 结论

时间和代码演化管理总是出现在软件开发中。幸运的是，现在我们有成熟的工具来解决这个问题。

# 关系

[](https://medium.com/dev-genius/code-smell-05-comment-abusers-feec3aeb926) [## 代码气味 05 —评论滥用者

### 代码有很多注释。注释与实现联系在一起，很难维护。

medium.com](https://medium.com/dev-genius/code-smell-05-comment-abusers-feec3aeb926) 

# 本意

> 这也是我写作的原因，因为生活除了回顾，从来就没有工作过。你不能控制生活，至少你可以控制你的版本。

*恰克·帕拉尼克*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)