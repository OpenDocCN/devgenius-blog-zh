# 代码味道 18 —静态函数

> 原文：<https://blog.devgenius.io/code-smell-18-static-functions-68d35c15e76?source=collection_archive---------1----------------------->

## 又一个全球接入加上懒惰。

![](img/5776a28e6b495e257ea95b808b3dfb96.png)

由 [Alex Azabache](https://unsplash.com/@alexazabache) 在 [Unsplash](https://unsplash.com/s/photos/bridge) 上拍摄的照片

# 问题

*   连接
*   易测性
*   协议过载
*   内聚力

# 解决方法

1.  类[单一责任原则](https://en.wikipedia.org/wiki/Single-responsibility_principle)是创建实例。尊重它。
2.  将方法委托给实例。
3.  创建无状态对象。不要叫他们**帮手**。

# 例子

*   静态类方法
*   静态属性

# 示例代码

## 错误的

## 对吧

# 侦查

我们可以实施一个策略来避免静态方法(除了构造函数之外的所有类方法)。

# 标签

*   全球的
*   图书馆

# 结论

阶级是全球性的伪装。用“库方法”污染他们的协议会破坏内聚力并产生耦合。我们应该用重构来提取静态。

在大多数语言中，我们不能操纵类和多态地使用它们，所以我们不能模仿它们或在测试中插入它们。

因此，我们有一个全球参考太难太脱钩。

# 关系

*   一个

[](https://mcsee.medium.com/code-smell-22-helpers-1d5057137ddb) [## 代码气味 22 —助手

### 你需要帮助吗？你要给谁打电话？

mcsee.medium.com](https://mcsee.medium.com/code-smell-22-helpers-1d5057137ddb) 

# 更多信息

*   [单一责任原则](https://en.wikipedia.org/wiki/Single-responsibility_principle)

> 没有任何编程问题不能通过多一层间接方式来解决。

*约翰·麦卡锡*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)