# 代码味道 10 —参数太多

> 原文：<https://blog.devgenius.io/code-smell-10-too-many-arguments-b44064610789?source=collection_archive---------1----------------------->

## *对象或函数需要太多参数才能工作*

![](img/781a7b30beb87702409e2a9bc029bc24.png)

由[托比亚斯·图利乌斯](https://unsplash.com/@tobiastu)在 [Unsplash](https://unsplash.com/s/photos/loaded) 上拍摄的照片

# 问题

*   低可维护性
*   低重用
*   连接

# 解决方法

*   找出论点之间的衔接关系
*   创造一个“环境”。
*   考虑使用一个[方法对象](https://wiki.c2.com/?MethodObject)模式。
*   避免“基本”类型:字符串、数组、整数等。思考物体。

# 例外

*   现实世界中的行动不需要紧密的合作者。

# 示例代码

## 错误的

## 对吧

# 侦查

当参数列表太大时，大多数 linters 会发出警告。

# 标签

*   原始的

# 结论

将论点联系起来并分组。总是喜欢真实世界的映射。在现实世界中找到如何将内聚对象中的参数分组。

如果一个函数有太多的参数，其中一些可能与类的构造有关。这也是一种设计的味道。

# 关系

[](https://mcsee.medium.com/code-smell-34-too-many-attributes-2df68c7db040) [## 代码味道 34 —属性太多

### 一个类定义了具有许多属性的对象。

mcsee.medium.com](https://mcsee.medium.com/code-smell-34-too-many-attributes-2df68c7db040) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)