# 代码气味 84—Max< Min (Javascript)

> 原文：<https://blog.devgenius.io/code-smell-84-max-min-javascript-37470a3111db?source=collection_archive---------6----------------------->

## Some functions do not behave as expected. Sadly, most programmers accept them.

![](img/bb3648149600c063658cb8bdff675327.png)

Photo by [Cris Baron](https://unsplash.com/@cris024)on[Unsplash](https://unsplash.com/s/photos/infinite)

> TL；DR:不要相信 max()和 min()函数。别理他们。

# 问题

*   最小惊讶原则
*   双射违反。
*   意外的结果

# 解决方法

1.使用成熟的语言。

2.避免使用*max()*和*min()*函数。

3.模型无限仔细。

# 示例代码

## 错误的

## 对吧

# 侦查

这些函数属于标准数学库。因此，它们不容易避免。

我们可以用棉绒挡住它们。

# 标签

*   java 描述语言

# 结论

我们需要非常小心地使用违反真实世界概念的函数。

# 关系

[](/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) [## 代码味道 69 —大爆炸(JavaScript 荒谬的铸件)

blog.devgenius.io](/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) 

# 更多信息

 [## 最小惊讶原则-维基百科

### 最小惊讶原则(POLA)，又名最小惊讶原则(或者法律或规则)，适用于…

en.wikipedia.org](https://en.wikipedia.org/wiki/Principle_of_least_astonishment) [](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [](https://codeburst.io/what-is-software-9a78c1172cf9) [## 软件有什么问题？

### 软件正在吞噬世界。我们这些工作、生活和热爱软件的人通常不会停下来思考它的…

codeburst.io](https://codeburst.io/what-is-software-9a78c1172cf9) 

# 信用

灵感来自[奥立弗·亨伯茨](https://medium.com/u/2fa0442b89ce?source=post_page-----37470a3111db--------------------------------)

> 今天的编程是软件工程师努力构建更大更好的防白痴程序和宇宙试图制造更大更好的白痴之间的竞赛。到目前为止，宇宙赢了。

里克·库克

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)