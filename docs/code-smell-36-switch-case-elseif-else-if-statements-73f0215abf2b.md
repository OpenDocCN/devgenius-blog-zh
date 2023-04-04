# 代码气味 36 — Switch/case/elseif/else/if 语句

> 原文：<https://blog.devgenius.io/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b?source=collection_archive---------0----------------------->

## 第一堂编程课:控制结构。高级开发人员的教训:避免它们。

![](img/0b7e1008b217f9c01912b6da257673be.png)

照片由[阿达什·库姆穆尔](https://unsplash.com/@akummur)在 [Unsplash](https://unsplash.com/s/photos/tree) 上拍摄

问题:

*   一起做太多决定
*   连接
*   重复代码
*   违反[开/关原理](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)。
*   新的条件不应该改变主算法。
*   无效的

# 解决方法

1.  多态性
2.  按照*开闭原则*创建层次/组合对象。
3.  使用[状态模式](https://en.wikipedia.org/wiki/State_pattern)模拟转换。
4.  使用[策略模式](https://en.wikipedia.org/wiki/Strategy_pattern) / [方法对象](https://wiki.c2.com/?MethodObject)为分支选择。

# 例子

*   离散值
*   状态迁移
*   算法选择。

# 示例代码

## 错误的

## 对吧

# 侦查

既然 If/else 用法有[有效案例](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484)，我们就不应该拔掉插头，禁止这些指令。我们可以将 if 语句/其他语句的比值作为警告。

# 关系

[](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c) [## 代码气味 12 —空

### 程序员使用 Null 作为不同的标志。它可以提示缺席、未定义的值、错误等。

medium.com](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c) 

# 更多信息

[](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484) [## 如何永远摆脱烦人的 if

### 为什么我们学习编程的第一条指令应该是最后使用的。

medium.com](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484) 

> 如果说调试是去除软件 bug 的过程，那么编程一定是把 bug 放进去的过程。

*埃德格·迪杰斯特拉*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)