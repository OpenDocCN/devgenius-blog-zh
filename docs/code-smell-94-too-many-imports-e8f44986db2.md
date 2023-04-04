# 代码气味 94 —太多导入

> 原文：<https://blog.devgenius.io/code-smell-94-too-many-imports-e8f44986db2?source=collection_archive---------1----------------------->

## 如果你的类依赖别人太多，就会耦合，脆弱。长长的进口清单是一个很好的指标。

![](img/7889ab9842c15cb1b43cafaed0412045.png)

在 [Unsplash](https://unsplash.com/s/photos/pile) 上由[zdenk macha ek](https://unsplash.com/@zmachacek)拍摄的照片

> TL；博士:不要进口太多。

# 问题

*   连接
*   违反单一责任原则
*   低内聚力

# 解决方法

1.下课

2.隐藏中间意外实现

# 示例代码

## 错误的

## 对吧

# 侦查

我们可以在棉绒上设置一个警告阈值。

# 标签

*   连接
*   涟漪效应

# 结论

在构建解决方案时，我们需要考虑依赖性，以最小化连锁反应。

# 关系

[](/code-smell-61-coupling-to-classes-8c52b8e33c95) [## 代码气味 61——耦合到类

### 上课很方便。我们可以随时调用它们。这样好吗？

blog.devgenius.io](/code-smell-61-coupling-to-classes-8c52b8e33c95) 

# 更多信息

[](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [## 耦合:唯一的软件设计问题

### 对我们软件的所有故障进行根本原因分析，会发现一个有多种伪装的单一罪魁祸首。

codeburst.io](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) 

[维基百科上的命名空间](https://en.wikipedia.org/wiki/Namespace)

# 信用

> 傻瓜忽视复杂性。实用主义者深受其害。有些可以避免。天才移除它。

艾伦·珀利斯

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)