# 代码气味 101 —与布尔比较

> 原文：<https://blog.devgenius.io/code-smell-101-comparison-against-booleans-958b112a560?source=collection_archive---------3----------------------->

## 当与布尔比较时，我们执行魔法转换并得到意想不到的结果

![](img/29b09dd92a50b5501f758977ad24692e.png)

迈克尔在 [Unsplash](https://unsplash.com/s/photos/disguise) 上抱着

TL；大卫:不要和真实相比。要么你是真的，要么你是假的，要么你不应该比较

# 问题

*   隐藏铸件
*   最不令人惊讶的违反原则
*   [废掉快](https://codeburst.io/fail-fast-3f3f036032b0)违反原则

# 解决方法

1.  使用布尔值
2.  不要将布尔与*布尔可转换对象*混合

# 语境

许多语言将值转换为布尔交叉域。

# 示例代码

## 错误的

## 对吧

# 侦查

[X]自动

Linters 可以检查显式比较和警告。

# 标签

*   铸件

# 结论

将许多非布尔值用作布尔值是一种常见的行业惯例。

我们在使用布尔时应该非常严格。

# 关系

[](/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) [## 代码味道 69 —大爆炸(JavaScript 荒谬的铸件)

blog.devgenius.io](/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) 

# 更多信息

[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

> 如果不行，再快也没用。

米歇尔·拉韦拉

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)