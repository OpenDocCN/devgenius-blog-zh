# 代码气味 71 —伪装成小数的魔法浮动

> 原文：<https://blog.devgenius.io/code-smell-71-magic-floats-disguised-as-decimals-31271b957ac5?source=collection_archive---------5----------------------->

## 不要相信 JavaScript 之类不成熟语言上的数字。

![](img/73428bb63454b7c19cdea813cbc0b516.png)

斯蒂芬·拉德福德在 [Unsplash](https://unsplash.com/s/photos/explosion) 上拍摄的照片

# 问题

*   [最小惊奇原则](https://en.wikipedia.org/wiki/Principle_of_least_astonishment)违反
*   [偶然的复杂性](https://medium.com/dev-genius/no-silver-bullet-65cba1775f9b)
*   错误的十进制表示。

# 解决方法

1.选择成熟的语言。

2.用小数表示[小数。](https://medium.com/@mcsee/the-one-and-only-software-design-principle-5328420712af)

# 示例代码

## 错误的

## 对吧

# 侦查

由于这是一种语言特征，所以很难察觉。我们可以要求我们的棉绒防止我们以这种方式操纵数字。

# 标签

*   Java Script 语言
*   过早优化

# 结论

我的第一个编程语言是 1985 年的 Commodore [的](https://en.wikipedia.org/wiki/Commodore_64)'s) basic。

我非常惊讶地发现，1+1+1 并不总是 3。然后他们引入了整数类型。

JavaScript 年轻 30 岁，也有同样的不成熟问题。

# 关系

[](/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) [## 代码气味 69 —大爆炸(JavaScript 荒谬的铸件)

blog.devgenius.io](/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) 

# 更多信息

这里是技术上的(也是偶然的)解释。

[](https://blog.pankajtanwar.in/do-you-know-01-02-03-in-javascript-here-is-why) [## 博客- Pankaj Tanwar -软件工程师

### 嘿👋，我从事 JavaScript 已经有一段时间了。昨天，我经历了一个非常奇怪的行为…

博客. pankajtanwar.in](https://blog.pankajtanwar.in/do-you-know-01-02-03-in-javascript-here-is-why) 

请不要争辩，告诉这是好的和预期的，因为这是二进制表示。

这些数是小数，我们应该用小数来表示。

如果您认为将它们表示为 floats 是一个巨大的性能改进，那么您就错了。

过早优化是万恶之源。

有此缺陷的语言列表:

 [## 浮点数学

### 你的语言没有坏，它在做浮点数学。计算机只能原生存储整数，所以它们需要…

0.30000000000000004.com](https://0.30000000000000004.com/) 

> 计算的目的是洞察，而不是数字。

理查德·海明

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)