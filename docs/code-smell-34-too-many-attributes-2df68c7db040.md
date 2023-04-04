# 代码味道 34 —属性太多

> 原文：<https://blog.devgenius.io/code-smell-34-too-many-attributes-2df68c7db040?source=collection_archive---------1----------------------->

## 一个类定义了具有许多属性的对象。

![](img/1ecb307a7baab13c59f5c215d22b0727.png)

[安迪李](https://unsplash.com/@andasta)在 [Unsplash](https://unsplash.com/s/photos/container) 上拍照

# 问题

*   低内聚力
*   连接
*   可维护性
*   可读性

# 解决方法

1.  查找与属性相关的方法。
2.  聚集这些方法。
3.  断开与这些集群相关的对象。
4.  查找与此新对象相关的真实对象，并替换现有引用。

# 例子

*   DTOs
*   非规范化的表行

# 示例代码

## 错误的

## 对吧

# 侦查

当你声明太多属性时，大多数 linters 会发出警告。设置一个好的警告阈值应该很容易。

# 标签

*   原始的

# 结论

臃肿的对象知道的太多，并且由于内聚性而非常难以改变。

开发人员经常更改这些对象，因此它们会带来合并冲突，并且是常见的问题来源。

# 关系

[](https://medium.com/dev-genius/code-smell-10-too-many-arguments-b44064610789) [## 代码味道 10 —参数太多

### 对象或函数需要太多的参数才能工作

medium.com](https://medium.com/dev-genius/code-smell-10-too-many-arguments-b44064610789) 

> 软件中如此多的复杂性来自于试图让一件事做两件事。

*瑞安歌手*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)