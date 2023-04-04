# 代码味道 76 —通用断言

> 原文：<https://blog.devgenius.io/code-smell-76-generic-assertions-b345491b745e?source=collection_archive---------4----------------------->

## 不要做弱测试来制造虚假的覆盖感。

![](img/30204ff4f0048265ccc053dd292212eb.png)

> TL；DR:测试断言应该精确。不太模糊也不太具体。没有灵丹妙药。

# 问题

*   假阴性
*   缺乏信任

# 解决方法

1.检查正确的案例

2.为功能案例断言。

3.不要测试实现。

# 示例代码

## 错误的

## 对吧

# 侦查

使用[突变测试](https://en.wikipedia.org/wiki/Mutation_testing)技术，我们可以在测试中发现这些错误。

# 标签

*   测试

# 结论

我们应该使用像 [TDD](https://mcsee.medium.com/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) 这样的开发技术，要求具体的业务案例，并基于我们的领域做出具体的断言。

# 关系

[](/code-smell-30-mocking-business-885299c32d6f) [## 代码气味 30——嘲弄商业

### 在测试行为时，嘲笑是一个很好的帮助。像许多其他工具一样，我们正在滥用它们。

blog.devgenius.io](/code-smell-30-mocking-business-885299c32d6f) [](/code-smell-52-fragile-tests-b917de33fd53) [## 代码气味 52 —易碎测试

### 测试是我们的安全网。如果我们不信任他们的正直，我们将处于极大的危险之中。

blog.devgenius.io](/code-smell-52-fragile-tests-b917de33fd53) 

# 更多信息

[](/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) [## 如何在遗留系统上挤压测试驱动开发

### 我们都喜欢 T.D.D .我们知道它的好处，我们已经阅读了一千篇关于如何使用它来构建系统的教程…

blog.devgenius.io](/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) [](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)  [## 突变测试-维基百科

### 变异测试(或变异分析或程序变异)被用来设计新的软件测试和评估新的软件测试。

en.wikipedia.org](https://en.wikipedia.org/wiki/Mutation_testing) 

# 信用

这种气味的灵感来自马里奥·塞尔韦拉，并在他的许可下使用。

> 一个程序产生错误结果的速度是原来的两倍，但速度却慢了无限。

*约翰·奥斯特胡特*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何品尝香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)