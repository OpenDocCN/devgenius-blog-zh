# 代码气味 03 —函数太长

> 原文：<https://blog.devgenius.io/code-smell-03-functions-are-too-long-accea7eb4ae9?source=collection_archive---------4----------------------->

## 人类过了 10 线就烦了。

![](img/9e3d8b57d01ebbe9933594c9193bbb3c.png)

照片由 [Hari Panicker](https://unsplash.com/@invisibleecho) 在 [Unsplash](https://unsplash.com/s/photos/long-road) 上拍摄

# 问题

*   低内聚力
*   高耦合
*   难以阅读

# 解决方法

1)重构

2)创建处理一些任务的小对象。对他们进行单元测试。

3)组合方法

# 例子

*   图书馆

# 示例代码

## 错误的

## 对吧

# 侦查

当方法大于预定义的阈值时，所有的 linters 都可以测量并发出警告。

# 也被称为

*   长方法

# 更多信息

*   [重构大师](https://refactoring.guru/es/smells/long-method)

# 标签

*   复杂性

> 程序是要被人类阅读的，只是顺便让计算机执行。

*唐纳德·克努特*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

mcsee.medium.com](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)