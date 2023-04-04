# 代码气味 05 —评论滥用者

> 原文：<https://blog.devgenius.io/code-smell-05-comment-abusers-feec3aeb926?source=collection_archive---------7----------------------->

## 代码有很多注释。注释与实现联系在一起，很难维护。

![](img/39e74394969123cd610ce7e1d8f73f51.png)

沃洛季米尔·赫里先科在 [Unsplash](https://unsplash.com/s/photos/chat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 问题

*   可维护性
*   过时的文件

# 解决方法

1)重构方法。

2)将方法重命名为更具声明性的方法。

3)打破方法。

4)如果一个注释描述了一个方法做什么，用这个描述命名这个方法。

5)只评论重要的设计决策。

# 示例:

*   图书馆
*   班级评论
*   方法注释

# 示例代码:

## 错误的

## 对吧

# 侦查

Linters 可以检测注释，并根据预定义的阈值检查注释/代码行的比率。

# 更多信息:

*   [重构。咕噜](https://refactoring.guru/es/smells/comments)
*   [什么是名字:第一部分](https://medium.com/dev-genius/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf)

# 标签

*   评论
*   宣言的

> 如果你不得不花费精力去查看一段代码并弄清楚它在做什么，那么你应该把它提取到一个函数中，并以 what 命名这个函数。

*马丁福勒*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

mcsee.medium.com](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)