# 代码气味 60 —全局类

> 原文：<https://blog.devgenius.io/code-smell-60-global-classes-3ae3bb2582ac?source=collection_archive---------2----------------------->

## 上课很方便。我们可以随时调用它们。这样好吗？

![](img/8e076b4f72eaa06274b136c1162674f8.png)

由[阿尔方斯·莫拉莱斯](https://unsplash.com/@alfonsmc10)在 [Unsplash](https://unsplash.com/s/photos/large-library) 上拍摄的照片

# 问题

*   连接
*   除非我们使用名称空间，否则类是全局的。
*   污染名称
*   静态方法
*   静态常数
*   一个

# 解决方法

1.  使用名称空间、模块限定符或类似的东西
2.  避免命名空间污染，保持全局名称尽可能短。
3.  类的单一职责是创建实例。

# 示例代码

## 错误的

# 对吧

# 侦查

我们可以使用几乎任何 linter 或者创建依赖规则来搜索错误的类引用。

# 标签

*   全局

# 结论

我们应该将我们的类限制在小的域中，只向外部暴露一些外观。这大大降低了耦合。

# 关系

[](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76) [## 代码味道 18 —静态函数

### 又一个全球接入加上懒惰。

medium.com](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76) [](https://medium.com/dev-genius/code-smell-17-global-functions-d32b8796d284) [## 代码味道 17 —全局函数

### 受到面向对象编程的阻碍，许多混合语言支持它。开发者滥用它们。

medium.com](https://medium.com/dev-genius/code-smell-17-global-functions-d32b8796d284) 

# 更多信息

[](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 辛格尔顿:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [## 耦合:唯一的软件设计问题

### 对我们软件的所有故障进行根本原因分析，会发现一个有多种伪装的单一罪魁祸首。

codeburst.io](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) 

> 编写害羞的代码——模块不会向其他模块透露任何不必要的东西，并且不依赖于其他模块的实现。

*迪夫·托马斯*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)