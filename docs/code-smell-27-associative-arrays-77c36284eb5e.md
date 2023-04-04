# 代码味道 27 —关联数组

> 原文：<https://blog.devgenius.io/code-smell-27-associative-arrays-77c36284eb5e?source=collection_archive---------2----------------------->

## *【键，值】，魔法，快速，可塑性和错误修剪。*

![](img/1baaf8a2d9e2b4c2312283e9d895b9a1.png)

照片由[梅丽莎·艾斯丘](https://unsplash.com/@melissaaskew?utm_source=unsplash)在 [Unsplash](https://unsplash.com/s/photos/group) 上拍摄

# 问题

*   连接
*   信息隐蔽
*   代码复制
*   快速失败
*   完整

# 解决方法

1.  具体化对象
2.  创造有凝聚力的小物件
3.  不要让他们贫血，找到他们的衔接关系。

# 示例代码

## 错误的

## 贫血的

## 验证

## 对吧

## 学位应该具体化

*很多人都患有* ***原始执念*** *并相信这是过度设计。设计软件就是要做决策，比较取舍。由于现代虚拟机可以有效地处理小的短命对象，所以性能的论点现在已经站不住脚了。*

# 侦查

我们不能禁止关联数组，因为它们作为第一种方法非常好。

它们将适用于导出数据、序列化、持久性和其他意外的实现问题。

我们应该避免它们出现在我们的系统中。

# 标签

*   原始的

# 结论

当创建对象时，我们不能把它们当成数据。这是一个常见的误解。

我们应该忠于我们的[双射](https://maximilianocontieri.com/the-one-and-only-software-design-principle)并发现真实世界的物体。

大多数关联数组都具有内聚性，并且表示真实世界的实体，我们必须将它们视为一级类对象。

# 关系

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

> 没有什么比暂时的入侵更长久的了。

凯尔·辛普森

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)