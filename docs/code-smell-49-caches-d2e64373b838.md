# 代码气味 49 —缓存

> 原文：<https://blog.devgenius.io/code-smell-49-caches-d2e64373b838?source=collection_archive---------3----------------------->

## 缓存很性感。他们是一夜情。我们需要在长期关系中避开它们。

![](img/26defa1d640d713e1c97d78a308533a6.png)

[艾梅·沃格桑](https://unsplash.com/@vogelina)在 [Unsplash](https://unsplash.com/) 上的照片

# 问题

*   连接
*   易测性
*   缓存失效。
*   可维护性
*   过早优化
*   反常行为
*   缺乏透明度
*   非确定性行为

# 解决方法

1.  如果你有一个结论性的基准，并愿意支付一些耦合:把一个对象放在中间。
2.  对所有失效场景进行单元测试。经验表明，我们以渐进的方式面对这些挑战。
3.  寻找一个真实世界的缓存隐喻，并对其建模。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一种设计气味。

政策很难强制执行。

# 标签

*   过早优化

# 结论

缓存应该具有功能性和智能性。

这样我们就可以管理失效。

通用缓存仅适用于低级对象，如操作系统、文件和流。

我们不应该缓存域对象。

该页面托管在一个缓存网站上。

# 关系

[](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

medium.com](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) 

# 更多信息

[](https://dev.vamsirao.com/what-is-cache-and-common-ways-of-using-it) [## 什么是缓存，为什么新开发人员会纠结于它？

### 让我用这段话来说明一下背景:在计算机科学中只有两个难题:缓存失效和…

dev.vamsirao.com](https://dev.vamsirao.com/what-is-cache-and-common-ways-of-using-it) [](https://frankel.hashnode.dev/a-hitchhikers-guide-to-caching-patterns) [## 缓存模式的搭便车指南

### 当您的应用程序开始变慢时，原因可能是执行链中的某个瓶颈…

frankel.hashnode.dev](https://frankel.hashnode.dev/a-hitchhikers-guide-to-caching-patterns) 

计算机科学只有两个硬东西:缓存失效和事物命名。

菲尔·卡尔顿

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)