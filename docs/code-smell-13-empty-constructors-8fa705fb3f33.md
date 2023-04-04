# 代码味道 13 —空构造函数

> 原文：<https://blog.devgenius.io/code-smell-13-empty-constructors-8fa705fb3f33?source=collection_archive---------0----------------------->

不完整的对象会导致很多问题。

![](img/cba388a812a63657a27c8e3eaca7a3f6.png)

布雷特·乔丹摄于 Pexels

# 问题

*   易变性
*   不完整的对象
*   创建和本质设置之间的并发性不一致。
*   安装员

[](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) [## 裸体模特第一部分:模特

### 可靠的数据结构及其有争议的(写)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) 

# 解决方法

*   在创作中传递对象的本质

[](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [## 变种人的邪恶力量

### 变异就是进化。它是由查尔斯·达尔文爵士提出的，我们在软件行业中使用它。但是有些事情是…

codeburst.io](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) 

# 例子

*   静态类型语言中的一些持久性框架需要空的构造函数。

# 例外

*   无状态对象。总是比静态类方法更好的解决方案。

# 示例代码

## 错误的

## 对吧

# 侦查

任何棉绒都可以警告这种(可能的)情况。

# 标签

*   本质
*   不完全的
*   易变的

# 结论

总是创建完整的对象。使它们的本质不可改变地经受时间的考验。从一开始，每个对象都需要它的本质是有效的。

> 编写一个没有契约的类就像生产一个没有规范的工程组件(电路、VLSI(超大规模集成)芯片、桥、引擎……)。没有专业工程师会考虑这个想法。

*贝特朗·梅耶*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)