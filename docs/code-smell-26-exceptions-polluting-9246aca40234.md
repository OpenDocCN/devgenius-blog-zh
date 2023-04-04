# 气味代码 26 —污染例外

> 原文：<https://blog.devgenius.io/code-smell-26-exceptions-polluting-9246aca40234?source=collection_archive---------5----------------------->

有许多不同的例外是非常好的。您的代码是声明性的和健壮的。还是没有？

![](img/6337ae657ba11d232fd2c6f440cdeba1.png)

由[尼克·范·登伯格](https://unsplash.com/@nngvandenberg)在 [Unsplash](https://unsplash.com/s/photos/smog) 上拍摄

# 问题

*   过度设计
*   命名空间污染

# 解决方法

1.  避免将[贫血的](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3)异常创建为全局异常。
2.  仅当异常行为不同时才创建异常。
3.  用对象建模异常。对于懒惰的程序员来说，类很方便。

# 示例代码

## 错误的

## 对吧

# 侦查

新的异常应该重写行为方法。

编号*代码*、*描述*、*可恢复*等都不是行为。

# 标签

*   虐待者
*   命名

# 结论

您不会为每个 Person 实例创建不同的类，所以它们返回不同的名称。为什么你会有例外？

您多久捕获一次特定的异常？。

出去检查你的代码。

有必要成为一个阶层吗？

您已经与该类耦合。请结合描述。

异常实例不应是[单例](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243)。

# 关系

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) 

> 你会走向毁灭，因为你相信规则的例外会产生新的规则。

皮尔斯·布朗

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)