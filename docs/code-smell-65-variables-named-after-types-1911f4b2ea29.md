# 代码味道 65 —以类型命名的变量

> 原文：<https://blog.devgenius.io/code-smell-65-variables-named-after-types-1911f4b2ea29?source=collection_archive---------4----------------------->

## 名字应该总是表明角色。

![](img/091a9a43126df3ef29cb3f1b46b1916c.png)

照片由 [Unsplash](https://unsplash.com/s/photos/name) 上的 sanga Rima Roman Selia 拍摄

# 问题

*   宣言的
*   为改变而设计
*   耦合到意外实现

# 解决方法

1.根据角色重命名变量。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一个语义规则。我们可以指示我们的 linters 警告我们不要使用与现有类、保留字类型相关的名称，因为它们太容易实现了。

# 标签

*   宣言的

# 结论

我们能碰到的第一个名字与一个偶然的观点有关。用我们的[绘图仪](https://maximilianocontieri.com/the-one-and-only-software-design-principle)在我们正在建造的模型上建立一个理论需要时间。一旦我们到达那里，我们必须重新命名我们的变量-

# 关系

[](https://medium.com/dev-genius/code-smell-38-abstract-names-c2306d0846ca) [## 代码气味 38 —抽象名称

### 避免太抽象的名字。名字应该有真实的意义

medium.com](https://medium.com/dev-genius/code-smell-38-abstract-names-c2306d0846ca) 

# 更多信息

[](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [](http://xunitpatterns.com/Intent%20Revealing%20Name.html) [## 意图暴露名称

### 这本书现在已经出版，这一章的内容可能已经发生了实质性的变化。你如何编写代码…

xunitpatterns.com](http://xunitpatterns.com/Intent%20Revealing%20Name.html) 

# 信用

这个想法来自这条推特

> 类型本质上是关于程序的断言。我认为让事情尽可能简单是有价值的，包括甚至不要说类型是什么。

丹·英格尔斯

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)