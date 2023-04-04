# 代码气味 14 —上帝物品

> 原文：<https://blog.devgenius.io/code-smell-14-god-objects-b84b75b702?source=collection_archive---------2----------------------->

## 知道得太多或做得太多的对象。

![](img/7820907e113576e613e72d7965d94120.png)

由 [Francisco Ghisletti](https://unsplash.com/@tank_ghisletti) 在 [Unsplash](https://unsplash.com/s/photos/greek-god-statue) 上拍摄的照片

# 问题

*   内聚力
*   连接

# 解决方法

*   分担责任。
*   遵循单一责任原则。
*   遵循童子军规则。

# 例子

*   图书馆

# 例外

*   [立面](https://en.wikipedia.org/wiki/Facade_pattern)

# 示例代码

## 错误的

## 对吧

# 侦查

Linters 可以计算方法，并针对阈值发出警告。

# 标签

*   紧密结合的

# 结论

图书馆在 60 年代还不错。在面向对象编程中，我们将在许多对象中分配职责。

# 也被称为

*   大班

# 更多信息

*   [维基百科](https://en.wikipedia.org/wiki/God_object)
*   [重构大师](https://refactoring.guru/es/smells/large-class)
*   [联轴器](https://mcsee.hashnode.dev/coupling-the-one-and-only-software-design-problem)

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)