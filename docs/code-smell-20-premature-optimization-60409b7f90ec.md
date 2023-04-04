# 代码味道 20 —过早优化

> 原文：<https://blog.devgenius.io/code-smell-20-premature-optimization-60409b7f90ec?source=collection_archive---------2----------------------->

提前计划需要一个没有开发者拥有的水晶球。

![](img/cecd142357637c59a58adadc657156fc.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske)在 [Unsplash](https://unsplash.com/s/photos/code) 上拍摄的照片

# 问题

*   连接
*   易测性
*   可读性
*   [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)

# 解决方法

1.  首先创建伟大的[模型](https://codeburst.io/what-is-software-9a78c1172cf9)和[双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)。
2.  一旦模型开始工作，就创建一个决定性的基准。
3.  程序员浪费大量的时间担心他们程序的非关键部分的速度，当考虑到调试和维护时，这些提高效率的尝试实际上有很大的负面影响。唐纳德·克努特
4.  [为性能而设计](https://wiki.c2.com/?DesignForPerformance)。
5.  使用测试驱动开发技术。它总是倾向于最简单的解决方案。

# 例子

*   奇怪的数据结构
*   隐藏所
*   一个

# 示例代码

## 错误的

## 对吧

# 侦查

这是一种设计气味，所以机械工具还检测不到。

# 标签

*   过早优化
*   反模式

# 结论

推迟性能决策，直到功能模型足够成熟。

Donald Knuth 创建/编译了最好/最快的算法和数据结构。他以极大的智慧警告我们不要滥用职权。为什么我们会觉得自己比他聪明？

# 关系

[](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

medium.com](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) 

# 更多信息

*   [c2 维基](https://wiki.c2.com/?PrematureOptimization)
*   [维基百科](https://en.wikipedia.org/wiki/Program_optimization)

[](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 辛格尔顿:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) 

> 过早优化是万恶之源。

唐纳德·克努特

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)