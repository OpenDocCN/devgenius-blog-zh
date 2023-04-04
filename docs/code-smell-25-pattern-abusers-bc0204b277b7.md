# 代码气味 25 —模式滥用者

> 原文：<https://blog.devgenius.io/code-smell-25-pattern-abusers-bc0204b277b7?source=collection_archive---------8----------------------->

## *图案很赞。权力越大，责任越大*

![](img/3612dc4fae986add42d34397169335c8.png)

由[内森·杜姆劳](https://unsplash.com/@nate_dumlao)在 [Unsplash](https://unsplash.com/s/photos/addict) 上拍摄的照片

# 问题

*   过度设计
*   可读性

# 解决方法

1.  衡量模式使用的权衡。
2.  创建基于真实世界名称的解决方案([本质](https://medium.com/dev-genius/no-silver-bullet-65cba1775f9b) l)而非架构(偶然)。
3.  选择[好名字](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1)。
4.  使用[映射器](https://codeburst.io/what-is-software-9a78c1172cf9)技术找到[双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)真实实体。

# 示例代码

## 错误的

## 对吧

# 侦查

创建自动检测规则将非常困难。

包含多个模式的类名是一个警告。

# 标签

*   虐待者
*   命名

# 结论

选择何时应用模式解决方案。你不会因为使用太多模式而变得更聪明。如果你为每个人选择了正确的机会，你就是聪明的。

# 关系

[](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

medium.com](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 辛格尔顿:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) 

# 更多信息

[](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) [## 如何分离遗留系统

### 改进遗留代码的练习

medium.com](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) 

> 当你有一把锤子时，每个问题看起来都像钉子。

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)