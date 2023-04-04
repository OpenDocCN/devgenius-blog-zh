# 代码气味 63 —特征羡慕

> 原文：<https://blog.devgenius.io/code-smell-63-feature-envy-ac1f93cf8dce?source=collection_archive---------4----------------------->

## *如果你的方法是嫉妒，不信任委托，你就应该开始做了。*

![](img/4869e5c51a8ae93c1feb0d7206d828ae.png)

照片由 [Hisu lee](https://unsplash.com/@lee_hisu) 在 [Unsplash](https://dev.to/s/photos/brothers?) 上拍摄

# 问题

*   连接
*   低重用
*   低可测性
*   错误的职责分配
*   双射断层

[](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

# 解决方法

1.  将方法移到适当的类中。

# 示例代码

## 错误的

## 对吧

# 侦查

一些 linters 可以检测到与另一个对象协作的序列模式。

# 标签

*   连接

# 结论

*   我们应该根据真实对象[映射器](https://maximilianocontieri.com/what-is-wrong-with-software)分配职责，避免滥用其他对象协议。

# 关系

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) 

# 更多信息

[](https://refactoring.guru/es/smells/feature-envy) [## 特征羡慕

### 一个方法访问另一个对象的数据多于访问它自己的数据。

重构大师](https://refactoring.guru/es/smells/feature-envy)  [## 德米特里定律-维基百科

### 德米特法则(LoD)或最小知识原则是开发软件的设计指南，特别是…

en.wikipedia.org](https://en.wikipedia.org/wiki/Law_of_Demeter) 

我们认为采用数据驱动方法的设计实践未能最大化封装，因为它们太快地关注对象的实现。我们提出了另一种面向对象的设计方法，它采用了责任驱动的方法。

丽贝卡·沃斯-布洛克

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)