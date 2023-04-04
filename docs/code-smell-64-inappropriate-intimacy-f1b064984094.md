# 代码气味 64——不适当的亲密关系

> 原文：<https://blog.devgenius.io/code-smell-64-inappropriate-intimacy-f1b064984094?source=collection_archive---------3----------------------->

## 两个纠结于爱情的阶层。

![](img/2d40d92a54f517480b1a9c3316e0eff8.png)

由 [Unsplash](https://unsplash.com/s/photos/intimate) 上的 [Becca Tapert](https://unsplash.com/@beccatapert) 拍摄的照片

# 问题

*   连接
*   错误的责任分配
*   凝聚力差
*   类接口太公开
*   可维护性
*   展开性

# 解决方法

1.  重构
2.  合并
3.  用委派替换层次结构。

[](https://refactoring.com/catalog/replaceSuperclassWithDelegate.html) [## 用委托替换超类

### 编辑描述

refactoring.com](https://refactoring.com/catalog/replaceSuperclassWithDelegate.html) 

# 示例代码

## 错误的

## 对吧

# 侦查

一些 linters 图类关系和协议依赖。通过分析协作图，我们可以推断出规则和提示。

# 标签

*   连接

# 结论

如果两个类太过相关，并且彼此不怎么交流，我们可能需要拆分、合并或重构它们，那么两个类应该尽可能少地互相了解。

# 关系

[](https://medium.com/dev-genius/code-smell-63-feature-envy-ac1f93cf8dce) [## 代码气味 63 —特征羡慕

### 如果你的方法是嫉妒和不信任授权，你应该开始这样做。

medium.com](https://medium.com/dev-genius/code-smell-63-feature-envy-ac1f93cf8dce) 

# 更多信息

【https://wiki.c2.com/?InappropriateIntimacy 

[](https://refactoring.guru/es/smells/inappropriate-intimacy) [## 不适当的亲密

### 一个类使用另一个类的内部字段和方法。

重构大师](https://refactoring.guru/es/smells/inappropriate-intimacy) [](https://www.thecodebuzz.com/awesome-code-inappropriate-intimacy-code-smell-resolution/) [## 可怕的代码-不适当的亲密代码气味决议

### 应用程序中使用的类可能会成为亲密的伙伴，在一起度过更多的时间。这可能会被发现是…

www.thecodebuzz.com](https://www.thecodebuzz.com/awesome-code-inappropriate-intimacy-code-smell-resolution/) 

> 无论你写干净的代码有多慢，如果你写得一团糟，你总是会更慢。

罗伯特·马丁

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)