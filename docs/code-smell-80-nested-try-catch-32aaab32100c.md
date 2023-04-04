# 代码气味 80 —嵌套的尝试/捕捉

> 原文：<https://blog.devgenius.io/code-smell-80-nested-try-catch-32aaab32100c?source=collection_archive---------3----------------------->

## 例外是区分快乐之路和困难之路的好方法。但是我们倾向于使我们的解决方案过于复杂。

![](img/e44fe40fbf48d4736c356cba18d920ff.png)

戴维·克洛德在 [Unsplash](https://unsplash.com/s/photos/fishing-net) 上的照片

> TL；DR:不要嵌套异常。没人关心你在内部街区做什么。

# 问题

*   可读性

# 解决方法

1.重构

# 示例代码

## 错误的

## 对吧

# 侦查

我们可以使用解析树来检测这种气味。

# 标签

*   例外

# 结论

不要滥用异常，不要创建没有人会捕获的异常类，也不要为每种情况都做好准备(除非你有一个很好的覆盖测试的真实场景)。

快乐路径应该永远比例外情况更重要。

# 关系

[](/code-smell-73-exceptions-for-expected-cases-ea870197d5fc) [## 代码气味 73 —预期情况的例外

blog.devgenius.io](/code-smell-73-exceptions-for-expected-cases-ea870197d5fc) [](/code-smell-26-exceptions-polluting-9246aca40234) [## 气味代码 26 —污染例外

### 有许多不同的例外是非常好的。您的代码是声明性的和健壮的。还是没有？

blog.devgenius.io](/code-smell-26-exceptions-polluting-9246aca40234) 

# 更多信息

 [## Java 中嵌套的 try catch 块——异常处理

### 当一个 try catch 块出现在另一个 try 块中时，它被称为嵌套 try catch 块。每一次尝试…

beginnersbook.com](https://beginnersbook.com/2013/04/nested-try-catch/) 

# 信用

感谢@[罗德里哥]([@罗德里哥](http://twitter.com/rodrigomd))的启发

> 编写软件时，好像我们是唯一需要理解它的人，这是最大的错误和错误的假设之一。

卡罗琳娜·什苏尔

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)