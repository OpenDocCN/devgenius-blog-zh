# 代码气味 12 —空

> 原文：<https://blog.devgenius.io/code-smell-12-null-64fbd7792a7c?source=collection_archive---------2----------------------->

程序员使用 Null 作为不同的标志。它可以提示缺席、未定义的值、错误等。多重语义导致耦合和错误。

![](img/1f17a6b283e09eccbf575f8e8d2cce90.png)

由 [Kurt Cotoaga](https://unsplash.com/@kydroon) 在 [Unsplash](https://unsplash.com/s/photos/black-hole) 上拍摄

# 问题

*   呼叫者和发送者之间的耦合。
*   呼叫者和发送者之间的不匹配。
*   if/开关/外壳污染。
*   Null 与真实对象不是多态的。因此*空指针异常*
*   现实世界中不存在空值。因此它违反了[双射原理](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)

[](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0) [## Null:十亿美元的错误

### 他不是我们的朋友。它不会简化生活，也不会让我们更有效率。只是更懒。是时候停止了…

codeburst.io](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0) 

# 解决方法

*   避免 Null。
*   使用 [NullObject 模式](https://en.wikipedia.org/wiki/Null_object_pattern)来避免 ifs。
*   使用[选项](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)。

# 例外

*   存在 NULL 的 API、数据库和外部系统。

# 示例代码

## 错误的

## 对吧

# 侦查

大多数短绒可以显示无效用法并警告我们。

# 标签

*   空

# 结论

*   Null 是十亿美元的错误。然而，大多数编程语言都支持它们，库也建议使用它们。

# 更多信息

*   [空:十亿美元的错误](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0)

> 我无法抗拒放入空引用的诱惑，只是因为它太容易实现了。这导致了数不清的错误、漏洞和系统崩溃，在过去的四十年里，这可能造成了数十亿美元的痛苦和损失。

*东尼·霍尔*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)