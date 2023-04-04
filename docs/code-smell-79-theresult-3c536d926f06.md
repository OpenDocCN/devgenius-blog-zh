# 气味代码 79 —结果

> 原文：<https://blog.devgenius.io/code-smell-79-theresult-3c536d926f06?source=collection_archive---------4----------------------->

## 如果一个名字已经被使用，我们总是可以在它前面加上“the”。

![](img/f4289789116fb02da0054439f96230dc.png)

Josue Michel 在 [Unsplash](https://unsplash.com/s/photos/chosen-one) 上拍摄的照片

> TL；DR:不要给你的变量加前缀。

# 问题

*   可读性
*   无意义的名字

# 解决方法

1.使用意图暴露姓名。

2.避免*模糊的干扰词*。

# 示例代码

## 错误的

## 对吧

# 侦查

正如我们的许多命名约定一样，我们可以指示我们的 linters 禁止使用像*theXxx…*这样的名字。

# 标签

*   可读性

# 结论

总是使用意图暴露的名字。

如果你的名字冲突，使用本地名字，提取你的方法，避免前缀。

# 关系

[](/code-smell-38-abstract-names-c2306d0846ca) [## 代码气味 38 —抽象名称

### 避免太抽象的名字。名字应该有真实的意义

blog.devgenius.io](/code-smell-38-abstract-names-c2306d0846ca) 

# 更多信息

[](/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

blog.devgenius.io](/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [](https://medium.com/shipmnts/how-to-be-great-at-giving-meaningful-names-54b19de66cdf) [## 如何擅长起有意义的名字

### 为什么有意义的名字？

medium.com](https://medium.com/shipmnts/how-to-be-great-at-giving-meaningful-names-54b19de66cdf) 

> 聪明的程序员和专业的程序员的一个区别是，专业的程序员明白清晰才是王道。专业人士善用他们的能力，编写他人可以理解的代码。

*罗伯特·马丁*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)