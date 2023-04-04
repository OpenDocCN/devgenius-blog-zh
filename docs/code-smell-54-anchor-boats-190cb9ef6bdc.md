# 代码气味 54 —锚船

> 原文：<https://blog.devgenius.io/code-smell-54-anchor-boats-190cb9ef6bdc?source=collection_archive---------2----------------------->

## *代码就在那里。以防万一。我们可能很快就需要它。*

![](img/c37e599953fe9115f922092fa681e00a.png)

照片由[克里斯·米凯尔·克里斯特](https://unsplash.com/@kmkr)在 [Unsplash](https://unsplash.com/s/photos/anchor) 上拍摄

# 问题

*   复杂性
*   连接

# 解决方法

1.  删除死代码。
2.  留下覆盖的和真正测试过的代码。

# 示例代码

## 错误的

## 对吧

# 侦查

使用一些[突变测试](https://en.wikipedia.org/wiki/Mutation_testing)变体，我们可以移除死代码并看到它测试失败。

我们需要有良好的覆盖面来依赖这个解决方案。

# 标签

*   YAGNI

# 结论

死代码总是一个问题。

我们可以使用像 TDD 这样的现代开发技术来确保所有代码都是活的。

[](https://medium.com/dev-genius/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) [## 如何在遗留系统上挤压测试驱动开发

### 我们都喜欢 T.D.D .我们知道它的好处，我们已经阅读了一千篇关于如何使用它来构建系统的教程…

medium.com](https://medium.com/dev-genius/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) 

# 关系

[](https://medium.com/dev-genius/code-smell-09-dead-code-9fb2488d9f5f) [## 代码气味 09 —死代码

### 不再使用或需要的代码。

medium.com](https://medium.com/dev-genius/code-smell-09-dead-code-9fb2488d9f5f) 

# 更多信息

[](https://exceptionnotfound.net/boat-anchor-the-daily-software-anti-pattern) [## 船锚-日常软件反模式

### 船锚是一种编程反模式，当一个系统的一部分被保留在那个系统中，尽管它没有…

exceptionnotfound.net](https://exceptionnotfound.net/boat-anchor-the-daily-software-anti-pattern)  [## 你不需要它

### 从维基百科，免费的百科全书跳转到导航跳转到搜索“你不会需要它”(YAGNI)是一个…

en.wikipedia.org](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) 

> 很难预测，尤其是未来。

*尼尔斯·玻尔*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)