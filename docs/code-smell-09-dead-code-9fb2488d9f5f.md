# 代码气味 09 —死代码

> 原文：<https://blog.devgenius.io/code-smell-09-dead-code-9fb2488d9f5f?source=collection_archive---------3----------------------->

不再使用或需要的代码。

![](img/dbce58cf912cdd3b803a81e644005c3e.png)

由[雷·什鲁斯伯里](https://pixabay.com/es/users/ray_shrewsberry-7673058/)在 [Pixabay](https://pixabay.com/) 上拍摄的照片

# 问题

*   可维护性

# 解决方法

*   删除代码
*   [亲吻](https://en.wikipedia.org/wiki/KISS_principle)

# 例子

*   镀金码或 [Yagni](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) 码。

# 例外

*   避免元编程。使用时，很难找到代码的引用。

[](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) [## 懒惰 I:元编程

### 元编程是神奇的。这是我们不应该使用它的主要原因。有许多可怕的后果…

codeburst.io](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) 

# 示例代码

## 错误的

## 对吧

# 侦查

如果你有一套很棒的测试，覆盖工具可以找到死代码(未覆盖的)。

# 标签

*   不必要的

# 结论

为了简单起见，删除死代码。如果您不确定代码，您可以使用[功能开关](https://en.wikipedia.org/wiki/Feature_toggle)临时禁用它。删除代码总是比添加代码更有价值。

# 更多信息

*   [懒惰 I:元编程](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6)

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)