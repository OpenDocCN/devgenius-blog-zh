# 代码气味 83 —变量重新分配

> 原文：<https://blog.devgenius.io/code-smell-83-variables-reassignment-6d3d4f4aa3b?source=collection_archive---------2----------------------->

## 变量重用是我们在大块代码中看到的。

![](img/cd04ae7dee326550a53fe2a36a1ba28e.png)

Robby McCullough 在 [Unsplash](https://unsplash.com/s/photos/spiral) 上的照片

> TL；DR:不要重用变量名。你破坏了可读性和重构的机会，除了懒惰什么也没得到。

# 问题

*   可读性
*   重构机会
*   错过的优化
*   易变性
*   垃圾收集错过了机会

# 解决方法

1.定义、使用和处理变量。

2.保持你的定义，使用和销毁变量简短。

# 示例代码

## 错误的

## 对吧

# 侦查

许多 linters 可以警告我们不要重用变量

# 标签

*   可读性

# 结论

重用变量是一种非上下文复制粘贴提示。

# 关系

[](/code-smell-03-functions-are-too-long-accea7eb4ae9) [## 代码气味 03 —函数太长

### 人类过了 10 线就烦了。

blog.devgenius.io](/code-smell-03-functions-are-too-long-accea7eb4ae9) 

# 更多信息

[](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [## 变种人的邪恶力量

### 变异就是进化。它是由查尔斯·达尔文爵士提出的，我们在软件行业中使用它。但是有些事情是…

codeburst.io](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [](https://softwareengineering.stackexchange.com/questions/115520/should-i-reuse-variables) [## 我应该重用变量吗？

### 首先，看你的开发环境。如果你调试有问题，因为你有五个变量在…

softwareengineering.stackexchange.com](https://softwareengineering.stackexchange.com/questions/115520/should-i-reuse-variables)  [## 软件设计/不要重复使用一个变量

### 核对表问题:每个变量都有一个单一的用途，没有被“回收”？这种做法符合规则…

en.wikiversity.org](https://en.wikiversity.org/wiki/Software_Design/Don%27t_reuse_a_variable) 

# 信用

> 不管你怎么看(干或懒)，想法都是一样的:让你的程序灵活。当变化来临时(它总是这样)，你会更容易适应它。

*克里斯·派恩*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)