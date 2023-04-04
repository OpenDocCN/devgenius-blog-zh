# 代码味道 69 —大爆炸(JavaScript 荒谬的铸件)

> 原文：<https://blog.devgenius.io/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322?source=collection_archive---------2----------------------->

## 这个灵巧的操作员是个麻烦制造者。

![](img/a448759f4aa2d5942550d99d01416abc.png)

由 [Unsplash](https://unsplash.com/s/photos/universe) 上 [Greg Rakozy](https://unsplash.com/@grakozy) 拍摄的照片

# TL；DR:不要把布尔型和非布尔型混在一起。

# 问题

*   不是声明性代码
*   难以调试
*   魔法铸件
*   偶然复杂性

# 解决方法

1.  明确一点
2.  不要将布尔值和非布尔值混合在一起。
3.  [快速失败](https://codeburst.io/fail-fast-3f3f036032b0)
4.  比你的编译器更聪明。
5.  忠于双射。

[](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

# 示例代码

## 错误的

## 对吧

# 侦查

因为这是某些语言中的一个“特性”,所以很难测试。我们可以设置编程策略或者选择更多的[严格语言](https://dev.to/tmaximini/typescript-bang-operator-considered-harmful-3hhi)。

我们应该检测一下*！* *！！*非布尔对象中的用法，并警告我们的程序员。

# 标签

*   铸造
*   强迫
*   java 描述语言

# 结论

像 JavaScript 这样的语言将它们的整个世界分为*真*或*假*值。这个决定隐藏了处理非布尔值时的错误。

我们应该非常严格，让布尔人(和他们的行为)远离非布尔人。

# 关系

[](/code-smell-24-boolean-coercions-1594cc1be008) [## 代码气味 24 —布尔强制

### 布尔应该只有真和假

blog.devgenius.io](/code-smell-24-boolean-coercions-1594cc1be008) [](/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

blog.devgenius.io](/code-smell-06-too-clever-programmer-bffec35daf0b) 

# 更多信息

[](https://www.bennadel.com/blog/3858-the-double-bang-operator-and-a-misunderstanding-of-how-javascript-handles-truthy-falsy-values.htm) [## 双响(！！)运算符以及对 JavaScript 如何处理真/假的误解…

### Ben Nadel 注意到开发人员使用了双爆炸(！！)运算符在 JavaScript 代码中出现的次数远远多于它们…

www.bennadel.com](https://www.bennadel.com/blog/3858-the-double-bang-operator-and-a-misunderstanding-of-how-javascript-handles-truthy-falsy-values.htm) [](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) [## 真理

### 在 JavaScript 中，真值是在布尔上下文中遇到时被认为是真的值。所有值都是…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) 

> 写一个不正确的程序比理解一个正确的程序更容易。

艾伦·J·珀利斯

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)