# 代码气味 24 —布尔强制

> 原文：<https://blog.devgenius.io/code-smell-24-boolean-coercions-1594cc1be008?source=collection_archive---------4----------------------->

*布尔值应该只是真和假*

![](img/64c058726891da676b5e30e1e49ccf0f.png)

# 问题

*   隐藏错误
*   偶然的复杂性与一种特殊的语言联系在一起。
*   可读性
*   在不同语言间跳跃的困难。
*   [国际单项体育联合会](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484)

# 解决方法

1.  明确一点。
2.  对布尔条件使用布尔。不是整数，不是[空值](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0)，不是字符串，不是列表。只有布尔人。
3.  [快速失败](https://codeburst.io/fail-fast-3f3f036032b0)

# 示例代码

## 错误的

## 对吧

# 侦查

这是一种语言特征。一些严格的语言用这种神奇的魔法显示警告。

# 标签

*   胁迫
*   原始的

# 结论

一些语言鼓励做一些神奇的缩写和自动转换。这是错误的来源，也是*过早优化*的警告。

我们应该总是尽可能明确。

# 关系

[](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

medium.com](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) 

# 更多信息

[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

> 不是语言让程序看起来简单。是程序员让语言显得简单！

[*罗伯特·马丁*](https://medium.com/u/ffa32c7b39eb?source=post_page-----1594cc1be008--------------------------------)

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)