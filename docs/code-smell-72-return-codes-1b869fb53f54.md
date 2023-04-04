# 代码气味 72 —返回代码

> 原文：<https://blog.devgenius.io/code-smell-72-return-codes-1b869fb53f54?source=collection_archive---------4----------------------->

## API，返回代码，C 编程语言，我们都经历过。

![](img/e8c9ca0500a491e6570665206f1e8bc0.png)

亚历克斯·海伊在 [Unsplash](https://unsplash.com/s/photos/numbers) 上的照片

**TL；DR:不要给自己返回代码。引发异常。**

# 问题

*   [IFs](https://mcsee.medium.com/how-to-get-rid-of-annoying-ifs-forever-317033474484)
*   代码污染
*   过时的文档
*   耦合到意外代码。
*   功能逻辑被污染。

# 解决方法

1.更改 id 并返回一般异常。

2.区分快乐路径和例外路径。

# 示例代码

## 错误的

## 对吧

# 侦查

我们可以通过 ifs 和返回检查来教会我们的 linters 寻找整数和字符串返回的模式。

# 标签

*   例外

# 结论

id 和代码是外部标识符。

当您需要与外部系统(例如 API Rest)交互时，它们非常有用。

我们不应该在我们自己的系统和我们自己的内部 API 上使用它们。

创建并引发一般异常。

只有当您准备好处理它们，并且它们有专门的行为时，才创建特定的异常。

不要创建[贫血的](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3)异常。

避免[不成熟和不成熟的优化语言](https://golangdocs.com/errors-exception-handling-in-golang)偏向返回代码。

# 关系

[](/code-smell-26-exceptions-polluting-9246aca40234) [## 气味代码 26 —污染例外

### 有许多不同的例外是非常好的。您的代码是声明性的和健壮的。还是没有？

blog.devgenius.io](/code-smell-26-exceptions-polluting-9246aca40234) 

# 更多信息

[](/how-to-get-rid-of-annoying-ifs-forever-317033474484) [## 如何永远摆脱烦人的 if

### 为什么我们学习编程的第一条指令应该是最后使用的。

blog.devgenius.io](/how-to-get-rid-of-annoying-ifs-forever-317033474484)  [## 干净代码:第 7 章-错误处理

### 错误处理很重要，但是如果它混淆了逻辑，那么它就是错误的，这可以从引用的“错误处理”中得到解释

nicolecarpenter.github.io](http://nicolecarpenter.github.io/2016/03/15/clean-code-chapter-7-error-handling.html) 

> 错误处理很重要，但是如果它模糊了逻辑，那就是错误的。

罗伯特·马丁

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)