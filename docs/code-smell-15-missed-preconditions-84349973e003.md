# 代码气味 15 —错过的前提条件

> 原文：<https://blog.devgenius.io/code-smell-15-missed-preconditions-84349973e003?source=collection_archive---------1----------------------->

断言、前置条件、后置条件和不变量是我们避免无效对象的盟友。回避它们会导致错误。

![](img/02e111c4cd8d160dc99a774a40c93ecc.png)

照片由 [Jonathan Chng](https://unsplash.com/@jon_chng) 在 [Unsplash](https://unsplash.com/s/photos/running-track) 上拍摄

# 问题

*   一致性
*   违约
*   难以调试
*   凝聚力差

# 解决方法

*   创造强有力的先决条件
*   引发异常
*   快速失败
*   防错性程序设计

# 例子

*   构造函数是极好的第一道防线
*   [贫血的物体](https://mcsee.hashnode.dev/code-smell-01-anemic-models)缺乏这些规则。

# 示例代码

## 错误的

## 对吧

# 侦查

*   只要有断言和不变量，就很难找到缺失的前提条件。

# 标签

*   一致性

# 结论

始终明确对象完整性。

打开生产断言。

即使这会带来性能损失。

数据和对象损坏更难发现。

快速失败是一种福气。

[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

# 更多信息

*   [面向对象的软件构造(Bertrand Meyer)](https://en.wikipedia.org/wiki/Object-Oriented_Software_Construction)

# 关系

*   [代码气味 01 —贫血型号](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3)

> 编写一个没有契约的类就像生产一个没有规范的工程组件(电路、VLSI(超大规模集成)芯片、桥、引擎……)。没有专业工程师会考虑这个想法。

*伯特兰·迈耶*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)