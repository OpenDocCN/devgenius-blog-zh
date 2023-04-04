# 代码味道 11 —代码重用的子类化

> 原文：<https://blog.devgenius.io/code-smell-11-subclassification-for-code-reuse-2e2f5b564204?source=collection_archive---------1----------------------->

## 代码重用是好的。但是子类化会产生静态耦合。

![](img/8f2a7d40e3bb52cfdf34f0f911ea36cf.png)

由[布兰登·格林](https://unsplash.com/@brandgreen)在 [Unsplash](https://unsplash.com/s/photos/tree) 拍摄的照片

# 问题

*   连接
*   可维护性

# 解决方法

*   喜欢作文。

# 例外

*   如果层级遵循的原则是*的行为类似于*，那么它就是安全的。

# 示例代码

## 错误的

## 对吧

# 侦查

*   当子类化具体方法时，重写会发出警告。
*   深层次(超过 3 层)也是坏子类化的一个线索。

# 标签

*   作文

# 结论

在遗留系统中，让*深层次*和*方法覆盖*是很常见的，我们需要重构它们，并根据*本质*原因而不是实现原因来子类化它们。

# 更多信息

*   [利斯科夫换人](https://en.wikipedia.org/wiki/Liskov_substitution_principle)

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)