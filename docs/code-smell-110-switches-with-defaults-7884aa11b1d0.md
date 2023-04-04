# 代码气味 110 —带默认值的开关

> 原文：<https://blog.devgenius.io/code-smell-110-switches-with-defaults-7884aa11b1d0?source=collection_archive---------9----------------------->

## 默认的意思是‘我们还不知道的一切’。我们无法预见未来。

![](img/02ee80a7768a9f1886881656c41bb3b9.png)

Joshua Woroniecki 在 [Unsplash](https://unsplash.com/s/photos/crystal-ball) 上的照片

> TL；DR:不要在你的案例中加入默认条款。为一个例外改变它。明确一点。

# 问题

*   连接
*   [故障快速原理违反](https://codeburst.io/fail-fast-3f3f036032b0)
*   开闭原则违背

# 解决方法

1.用多态性替换 if 和 cases

2.将默认代码更改为异常

# 语境

当使用案例时，我们通常会添加一个默认案例，这样就不会失败。

失败总比没有证据就做决定好。

由于[外壳和开关](https://mcsee.medium.com/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b)也是一种气味，我们可以避开。

# 示例代码

## 错误的

## 对吧

# 侦查

[X]半自动

我们可以告诉 linter 在默认情况下警告我们，除非有例外。

# 标签

*   快速失败

# 结论

编写健壮的代码并不意味着我们需要在没有证据的情况下做决定。

# 关系

[](/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b) [## 代码气味 36 — Switch/case/elseif/else/if 语句

### 第一节编程课:控制结构。高级开发人员的教训:避免它们。

blog.devgenius.io](/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b) 

# 更多信息

[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

> 添加一个特性的成本不仅仅是编写代码所需的时间。成本还包括增加一个未来扩张的障碍。诀窍是挑选不互相争斗的特性。

*约翰·卡马克*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)