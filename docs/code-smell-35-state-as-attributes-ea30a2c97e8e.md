# 代码气味 35 —作为属性陈述

> 原文：<https://blog.devgenius.io/code-smell-35-state-as-attributes-ea30a2c97e8e?source=collection_archive---------2----------------------->

## 当一个对象改变它的状态时，最好的解决方法是改变它的属性，不是吗？

![](img/68d237e3bc0ead679af6d195890cb1a0.png)

由[汤姆·克鲁斯](https://unsplash.com/@tomcrewceramics)在 [Unsplash](https://unsplash.com/s/photos/porcelain) 拍摄的照片

# 问题

*   易变性
*   污染属性
*   安装员

[](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) [## 气味代码 28 —设定器

### 初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

medium.com](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) 

# 解决方法

1.  作为数学集合包含的模型状态。
2.  状态是偶然的，把它从物体上拿走。

# 例子

*   状态图

# 示例代码

## 错误的

## 对吧

# 侦查

如果我们想极端一点，我们应该考虑每个*设置器*都是一个潜在的状态变化。棉绒可以警告我们。但是我们可能会得到太多的误报。

# 例外

*   过度设计
*   性能问题(如果有严格的基准支持的话)。

[](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

medium.com](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) 

# 标签

*   变化

# 结论

这种技术非常优雅，但会导致过度设计。例如改变一个视觉成分，它的颜色应该是这种气味的反例。

我们应该像对待其他气味一样小心谨慎。

它们是提示，而不是硬性规定。

# 关系

[](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) [## 气味代码 28 —设定器

### 初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

medium.com](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) 

# 更多信息

[](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [## 变种人的邪恶力量

### 变异就是进化。它是由查尔斯·达尔文爵士提出的，我们在软件行业中使用它。但是有些事情是…

codeburst.io](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) 

> 首先让改变变得容易(警告:这可能很难)，然后让改变变得容易。

肯特·贝克

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)