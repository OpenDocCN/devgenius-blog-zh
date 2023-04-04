# 代码气味 46 —重复代码

> 原文：<https://blog.devgenius.io/code-smell-46-repeated-code-433d3feb516d?source=collection_archive---------5----------------------->

## 干是我们的口头禅。老师告诉我们要消除重复。我们需要超越。

![](img/60f125c66754eb796b22b1f3b00ea46a.png)

锡德·巴拉钱德朗在 [Unsplash](https://unsplash.com/s/photos/parrot) 上拍摄的照片

# 问题

*   代码复制
*   可维护性
*   [不要重复自己](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

# 解决方法

1.  寻找重复的模式(不是重复的代码)。
2.  创建一个抽象概念。
3.  参数化抽象调用。
4.  用作文，千万不要继承。
5.  单元测试新的抽象。

# 示例代码

## 错误的

## 对吧

# 侦查

Linters 可以找到重复的代码。

没有很好的发现相似的模式。

也许很快机器学习就会帮助我们自动找到这样的抽象。

现在，这取决于我们人类。

# 例子

*   [自动代码生成器](https://codeburst.io/lazyness-ii-code-wizards-18cc5672b642)

# 标签

*   复制

# 结论

重复的代码总是一股臭味。

复制粘贴代码永远是一种耻辱。

使用我们的重构工具，我们需要接受消除重复的挑战，相信我们的测试是一个安全网。

# 关系

[](https://medium.com/dev-genius/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) [## 代码味道 11 —代码重用的子类化

### 代码重用是好的。但是子类化会产生静态耦合。

medium.com](https://medium.com/dev-genius/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) 

# 更多信息

[](https://deepdive.hashnode.dev/dry-dont-repeat-yourself) [## 干——不要重复自己

### DRY 原则是干净代码不可或缺的一部分，但是它实际上意味着什么，它真正有什么好处…

deepdive.hashnode.dev](https://deepdive.hashnode.dev/dry-dont-repeat-yourself) [](https://codeburst.io/lazyness-ii-code-wizards-18cc5672b642) [## 懒惰 II:代码向导

### 代码生成器完成了我们的艰苦工作。但是我们不再需要它们了。

codeburst.io](https://codeburst.io/lazyness-ii-code-wizards-18cc5672b642) 

> 复制粘贴是一个设计错误。

*大卫·帕纳斯*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)