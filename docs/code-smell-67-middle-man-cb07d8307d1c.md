# 气味代码 67——中间人

> 原文：<https://blog.devgenius.io/code-smell-67-middle-man-cb07d8307d1c?source=collection_archive---------3----------------------->

## 让我们打破德米特里定律。

![](img/b42d99031a6eb09b7762b0bc4a34f7c0.png)

照片由 Unsplash 上的 Dan counsel 拍摄

# 问题

*   不必要的间接
*   空类
*   可读性

# 解决方法

1.移除中间人。

# 示例代码

## 错误的

## 对吧

# 侦查

同它的[相反的气味](https://medium.com/dev-genius/code-smell-08-long-chains-of-collaborations-68aa0207ddd0)，我们可以利用解析树来检测这个小。

# 标签

*   连接
*   宣言的
*   可读性

# 结论

这与[的消息链完全相反。](https://medium.com/dev-genius/code-smell-08-long-chains-of-collaborations-68aa0207ddd0)我们明确了信息链。

# 关系

[](https://medium.com/dev-genius/code-smell-08-long-chains-of-collaborations-68aa0207ddd0) [## 代码气味 08——长长的合作链

### 使长链产生耦合和涟漪效应。任何连锁变化都会破坏代码。

medium.com](https://medium.com/dev-genius/code-smell-08-long-chains-of-collaborations-68aa0207ddd0) 

# 更多信息

[](https://refactoring.guru/smells/middle-man) [## 中间人

### 如果一个类只执行一个动作，将工作委托给另一个类，那么它到底为什么存在？

重构大师](https://refactoring.guru/smells/middle-man) [](https://refactoring.com/catalog/removeMiddleMan.html) [## 移除中间人

### 编辑描述

refactoring.com](https://refactoring.com/catalog/removeMiddleMan.html) 

[https://wiki.c2.com/?MiddleMan](https://wiki.c2.com/?MiddleMan)

[](https://www.jetbrains.com/help/idea/remove-middleman.html#remove_middleman_example) [## 删除中间人| IntelliJ IDEA

### “移除中间人”重构允许您用等价的调用替换对类中委托方法的所有调用…

www.jetbrains.com](https://www.jetbrains.com/help/idea/remove-middleman.html#remove_middleman_example)  [## 德米特里定律-维基百科

### 德米特法则(LoD)或最小知识原则是开发软件的设计指南，特别是…

en.wikipedia.org](https://en.wikipedia.org/wiki/Law_of_Demeter) 

> 每当我不得不思考理解代码在做什么时，我会问自己是否可以重构代码以使理解更加直接。

*马丁福勒*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)