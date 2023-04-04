# 气味代码 66——猎枪手术

> 原文：<https://blog.devgenius.io/code-smell-66-shotgun-surgery-d97eb688622?source=collection_archive---------7----------------------->

## 只说一次

![](img/b9ce903f02855efbcccfd5d638730dcf.png)

照片由[威廉·伊西德](https://unsplash.com/@williamisted)在 [Unsplash](https://unsplash.com/s/photos/shotgun) 上拍摄

# 问题

*   错误的责任分配
*   代码复制
*   可维护性
*   [单责](https://en.wikipedia.org/wiki/Single-responsibility_principle)违规。
*   复制粘贴的代码。

# 解决方法

1.  重构

# 示例代码

## 错误的

## 对吧

# 侦查

一些现代的 linters 可以检测重复的模式(不仅仅是重复的代码),并且在执行我们的代码审查时，我们可以很容易地检测到这个问题并要求重构。

# 标签

*   代码复制

# 结论

如果我们的模型以 1:1 的比例映射到现实世界，并且我们的职责在正确的位置，那么添加新功能应该是简单的。我们应该警惕跨越几个类的小变化。

# 更多信息

[](https://en.wikipedia.org/wiki/Shotgun_surgery) [## 猎枪手术-维基百科

### 猎枪手术是软件开发中的一种反模式，发生在开发人员向应用程序添加特性的地方…

en.wikipedia.org](https://en.wikipedia.org/wiki/Shotgun_surgery) [](https://refactoring.guru/es/smells/shotgun-surgery) [## 猎枪手术

### 做任何修改都需要对许多不同的类做许多小的改变。

重构大师](https://refactoring.guru/es/smells/shotgun-surgery) [](https://blog.ndepend.com/shotgun-surgery) [## 霰弹枪手术:它是什么以及如何停止它

### 我真的很喜欢“猎枪手术”这个名字，因为它描述了一种代码气味。这是一种侵略性和…

blog.ndepend.com](https://blog.ndepend.com/shotgun-surgery) [](https://dzone.com/articles/code-smell-shot-surgery) [## 霰弹枪手术代码气味- DZone Java

### 代码气味在概念上类似于开发级反模式。有时在我们的代码中，我们引入了代码气味…

dzone.com](https://dzone.com/articles/code-smell-shot-surgery) 

> 重复是设计良好的系统的主要敌人。

罗伯特·马丁

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)