# 代码气味 86 —可变常量数组

> 原文：<https://blog.devgenius.io/code-smell-86-mutable-const-arrays-97b1a7d72b6c?source=collection_archive---------4----------------------->

![](img/ac40763795f987bbab4399423b1dc996.png)

照片由 [Unsplash](https://unsplash.com/s/photos/zombie) 上的 [Zorik D](https://unsplash.com/@justzorik) 拍摄

*Const 声明某物是不变的。会变异吗？*

> *TL；DR:不要依赖语言欺骗指令。*

# 问题

*   意想不到的副作用
*   偶然复杂性

# 解决方法

1.  使用更好的语言
2.  使用[展开操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

# 示例代码

## 错误的

## 对吧

# 侦查

既然这是“语言特色”，我们可以明确禁止。

# 标签

*   易变性
*   Java Script 语言

# 结论

我们应该永远支持设计的不变性，并特别注意副作用。

# 更多信息

*   [变种人的邪恶力量](https://dev.to/mcsee/the-evil-powers-of-mutants-1noo)

# 信用

感谢 Oliver Jumpertz 的提示。

> 正确性显然是首要的品质。如果一个系统没有做它应该做的事情，那么关于它的其他事情都无关紧要。

伯特兰·迈耶

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)