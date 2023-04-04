# 代码气味 30——嘲弄商业

> 原文：<https://blog.devgenius.io/code-smell-30-mocking-business-885299c32d6f?source=collection_archive---------3----------------------->

## 在测试行为时，嘲笑是一个很好的帮助。像许多其他工具一样，我们正在滥用它们。

![](img/84625f62b335930eac1e53afa8cdd852.png)

由[赛义德·艾哈迈德](https://unsplash.com/@syedabsarahmad)在 [Unsplash](https://unsplash.com/s/photos/monkey) 上拍摄的照片

# 问题

*   复杂性
*   虚假的安全感。
*   平行/复制对象(真实和模拟)
*   可维护性

# 解决方法

1.[嘲笑](https://en.wikipedia.org/wiki/Mock_object)只是非商业实体。

2.如果 mock 的接口有太多的行为，就移除它。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一种建筑模式。创建自动检测规则并不容易。

# 例外

*   模拟意外问题(序列化、数据库、API)是避免耦合的一个非常好的实践。

# 标签

*   虐待者

# 结论

像许多其他的测试替身一样，模拟也是很好的工具。明智地选择何时使用它们是一门艺术。

想象一下，在一部戏中，每个演员不是与其他演员一起排练，而是必须与 25 名编剧互动。演员们永远不会一起排练。这出戏的结果会怎样？

# 也被称为

*   骗子

# 更多信息

[](https://medium.com/javascript-scene/mocking-is-a-code-smell-944a70c90a6a) [## 嘲讽是一种代码气味

### 注:这是“作曲软件”系列的一部分(现在是一本书！)关于学习函数式编程和…

medium.com](https://medium.com/javascript-scene/mocking-is-a-code-smell-944a70c90a6a) [](https://martinfowler.com/articles/mocksArentStubs.html) [## 模仿不是树桩

### 我将用一个简单的例子来说明这两种风格。(这个例子是用 Java 写的，但是原理是有意义的……

martinfowler.com](https://martinfowler.com/articles/mocksArentStubs.html) 

> 农药悖论。你用来防止或发现错误的每一种方法都会留下一些更微妙的错误，这些方法对它们是无效的。

鲍里斯·贝泽

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)