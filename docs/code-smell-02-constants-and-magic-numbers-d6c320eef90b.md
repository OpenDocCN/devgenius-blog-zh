# 代码气味 02——常数和幻数

> 原文：<https://blog.devgenius.io/code-smell-02-constants-and-magic-numbers-d6c320eef90b?source=collection_archive---------1----------------------->

## 一种方法用大量的数字进行计算，而不描述它们的语义。

![](img/0524157a3f1fd1ebb7dd89b3fc595677.png)

由 [Unsplash](https://unsplash.com/s/photos/magic) 上的[克里斯托佛罗拉](https://unsplash.com/@krisroller)拍摄的照片

# 问题

*   [联轴器](https://mcsee.hashnode.dev/coupling-the-one-and-only-software-design-problem)
*   低可测性
*   可读性低
*   重复代码

# 解决方法

1)用语义和名称(有意义和揭示意图)重命名常数。

2)用参数替换常量，这样就可以从外部模拟它们。

3)常量定义通常是与常量(ab)用户不同的对象。

# 例子

*   算法超参数

# 示例代码

## 错误的

## 对吧

# 侦查

许多 linters 可以检测属性和方法中的数字文字。

# 关系

# 标签

*   硬编码
*   常数
*   重复代码

# 更多信息

*   [维基百科](https://en.wikipedia.org/wiki/Magic_number_(programming)
*   [重构大师](https://refactoring.guru/es/replace-magic-number-with-symbolic-constant)
*   [如何分离遗留系统](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642)

本文是代码气味系列的一部分。

[](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

mcsee.medium.com](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)