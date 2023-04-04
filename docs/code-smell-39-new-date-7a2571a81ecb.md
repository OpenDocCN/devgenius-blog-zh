# 代码气味 39 —新日期()

> 原文：<https://blog.devgenius.io/code-smell-39-new-date-7a2571a81ecb?source=collection_archive---------10----------------------->

## *70s 第一教程:getCurrentDate()。小菜一碟。我们处在 20 年代，时代不再全球化。*

![](img/d3fca3dbaa1474bc779c4eaaf193cb3f.png)

# 问题

*   连接
*   脆弱的测试
*   时区问题

# 解决方法

1.  使用依赖注入来分离时间源。

# 示例代码

## 错误的

## 对吧

# 侦查

我们应该禁止全局函数策略。我们需要耦合到偶然的和可插入的时间源。

# 结论

`Date.today() , Time.now()`等全局系统调用都是耦合气味。

因为测试必须在完全的环境控制下进行。我们应该很容易地设置时间，来回移动时间等等。

日期和时间类应该只创建不可变的实例。给出实际时间不是他们的责任。这违反了单一责任原则。

时间的流逝总是被程序员嗤之以鼻。这使得对象是可变的，设计是糟糕的和耦合的。

# 关系

[](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76) [## 代码味道 18 —静态函数

### 又一个全球接入加上懒惰。

medium.com](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76) 

# 更多信息

[](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [## 变种人的邪恶力量

### 变异就是进化。它是由查尔斯·达尔文爵士提出的，我们在软件行业中使用它。但是有些事情是…

codeburst.io](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) 

# 标签

*   全局

> 在编程中，困难的部分不是解决问题，而是决定解决什么问题。

*保罗·格拉厄姆*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)