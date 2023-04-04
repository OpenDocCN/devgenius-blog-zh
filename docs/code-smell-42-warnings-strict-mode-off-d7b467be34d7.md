# 代码气味 42 —警告/严格模式关闭

> 原文：<https://blog.devgenius.io/code-smell-42-warnings-strict-mode-off-d7b467be34d7?source=collection_archive---------5----------------------->

## 编译器和警告灯在那里提供帮助。不要忽视他们。

![](img/49580d346299d9dd61c07bd3433ad41c.png)

由[诺亚·多米尼克·西尔维奥](https://unsplash.com/@electronicsocks)在 [Unsplash](https://unsplash.com/s/photos/traffic-light) 上拍摄的照片

# 问题

*   遗漏的错误
*   涟漪效应
*   快速失败

# 解决方法

1.  启用所有警告
2.  在生产中启用前提条件和断言。
3.  [快速失败](https://codeburst.io/fail-fast-3f3f036032b0)
4.  [合同设计](https://en.wikipedia.org/wiki/Design_by_contract)

# 示例代码

## 错误的

## 对吧

# 侦查

大多数语言都有警告级别。我们应该把大部分的*打开*。

我们应该运行 linters 来静态分析我们的代码，寻找潜在的问题。

# 标签

*   快速失败

# 结论

如果我们忽略警告，代码迟早会失败。

如果软件在*之后*出现故障，我们将很难找到根本原因。

缺陷可能会靠近第一个警告，而远离崩溃。

如果我们遵循*破窗理论*，我们就不应该容忍任何警告，所以一个新问题不会在*容忍的*警告的海洋中被忽视。

# 关系

[](https://medium.com/dev-genius/code-smell-19-optional-arguments-c0714855dbbb) [## 代码味道 19 —可选参数

### 伪装成友好的快捷方式是另一种耦合气味。

medium.com](https://medium.com/dev-genius/code-smell-19-optional-arguments-c0714855dbbb) [](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c) [## 代码气味 12 —空

### 程序员使用 Null 作为不同的标志。它可以提示缺席、未定义的值、错误等。

medium.com](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c) 

# 更多信息

[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) [](https://blog.rahulism.co/use-strict-in-javascript) [## JavaScript 中的“使用严格”

### 严格模式使您的程序或函数遵循严格的操作上下文。现在这是我最近的一篇关于…

blog.rahulism.co](https://blog.rahulism.co/use-strict-in-javascript) 

> 一个人的蹩脚软件是另一个人的全职工作。

杰西卡·加斯顿

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)