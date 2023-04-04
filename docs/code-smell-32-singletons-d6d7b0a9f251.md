# 代码气味 32 —单件

> 原文：<https://blog.devgenius.io/code-smell-32-singletons-d6d7b0a9f251?source=collection_archive---------2----------------------->

## 世界上最常用和最著名的设计模式正在给我们带来巨大的伤害。

![](img/4b610410a7fd725d23029b2324a3dd0f.png)

玛利亚·特内娃在 [Unsplash](https://unsplash.com/s/photos/rotten) 上拍摄的照片

# 问题

*   [联轴器](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04)
*   易测性
*   意外的实现问题。
*   多线程问题。
*   静态方法污染。
*   违反对象创建协定。
*   [双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)错配。
*   记忆问题。
*   过早优化。

[](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

medium.com](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) 

# 解决方法

1.  避免它。
2.  使用上下文唯一对象。
3.  基准对象创建。

# 例子

*   数据库访问
*   全局
*   记录器
*   助手

[](https://medium.com/dev-genius/code-smell-22-helpers-1d5057137ddb) [## 代码气味 22 —助手

### 你需要帮助吗？你要给谁打电话？

medium.com](https://medium.com/dev-genius/code-smell-22-helpers-1d5057137ddb) 

# 示例代码

## 错误的

上帝是典型的单身例子。

## 对吧

# 侦查

这是一种设计模式。我们应该通过政策来避免。

我们可以为像*‘getInstance()’*这样的模式添加 linter 规则，这样新开发人员就不会用这个[反模式](https://en.wikipedia.org/wiki/Singleton_pattern)感染代码。

# 标签

*   全局

# 结论

这是社会已经承认的历史错误。尽管如此，懒惰的开发者一次又一次地带来它。我们需要就其缺点达成共识。

# 关系

[](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

medium.com](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [](https://medium.com/dev-genius/code-smell-25-pattern-abusers-bc0204b277b7) [## 代码气味 25 —模式滥用者

### 图案很赞。权力越大，责任越大

medium.com](https://medium.com/dev-genius/code-smell-25-pattern-abusers-bc0204b277b7) [](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76) [## 代码味道 18 —静态函数

### 又一个全球接入加上懒惰。

medium.com](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76) 

# 更多信息

[](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 辛格尔顿:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) 

> *图不是模型。模型不是图表。它是一种抽象，是一组概念和它们之间的关系。*

埃里克·埃文斯

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)