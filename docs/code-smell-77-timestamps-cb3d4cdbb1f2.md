# 代码气味 77 —时间戳

> 原文：<https://blog.devgenius.io/code-smell-77-timestamps-cb3d4cdbb1f2?source=collection_archive---------2----------------------->

## 时间戳被广泛使用。他们有一个中央发行机构，不会回去，是吗？

![](img/760b4b37da01958449d2d2c445b84369.png)

> TL；DR:序列不要用时间戳。集中并锁定您的发行者。

# 问题

*   双射故障。
*   时间戳冲突。
*   时间戳精度。
*   数据包紊乱。

-一个本质问题(排序)的不良[意外实现](https://medium.com/dev-genius/no-silver-bullet-65cba1775f9b)(时间戳)。

# 解决方法

1.使用集中顺序压模。(不，不是一个[单身](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243))。

2.如果需要模拟一个序列，[模拟一个序列](https://medium.com/@mcsee/the-one-and-only-software-design-principle-5328420712af)。

# 示例代码

## 错误的

## 对吧

# 侦查

时间戳在许多语言中非常流行，并且无处不在。

我们需要用它们来建模…时间戳。

# 标签

*   双射

# 结论

这种气味的灵感来自最近的[独创性软件故障](https://www.hebergementwebs.com/transport/the-autonomous-helicopter-mars-named-ingenuity-is-confused-by-a-time-stamp-issue-providing-insightful-lessons-for-self-driving-cars-ai)。

如果我们不遵循我们的[映射器](https://medium.com/@mcsee/the-one-and-only-software-design-principle-5328420712af)规则和时间模型序列，我们将面临麻烦。

幸运的是,“独创性”是一种复杂的自主飞行器，拥有强大的故障安全着陆软件。

这段视频描述了故障

# 关系

[](/code-smell-39-new-date-7a2571a81ecb) [## 代码气味 39 —新日期()

### 70s 第一教程:getCurrentDate()。小菜一碟。我们处在 20 年代，时代不再全球化。

blog.devgenius.io](/code-smell-39-new-date-7a2571a81ecb) [](/code-smell-32-singletons-d6d7b0a9f251) [## 代码气味 32 —单件

### 世界上最常用和最著名的设计模式正在给我们带来巨大的伤害。

blog.devgenius.io](/code-smell-32-singletons-d6d7b0a9f251) [](/code-smell-71-magic-floats-disguised-as-decimals-31271b957ac5) [## 代码气味 71 —伪装成小数的魔法浮动

blog.devgenius.io](/code-smell-71-magic-floats-disguised-as-decimals-31271b957ac5) 

# 更多信息

[](https://ieeexplore.ieee.org/document/805196) [## 时间戳:关于它们的使用和实现的主要问题

### 本文讨论了创建和使用安全数字时间戳时出现的一些问题。从……

ieeexplore.ieee.org](https://ieeexplore.ieee.org/document/805196) [](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [](https://codeburst.io/what-is-software-9a78c1172cf9) [## 软件有什么问题？

### 软件正在吞噬世界。我们这些工作、生活和热爱软件的人通常不会停下来思考它的…

codeburst.io](https://codeburst.io/what-is-software-9a78c1172cf9) 

> 最美的代码，最美的函数，最美的程序，有时候根本就不存在。

乔恩·本特利

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)