# 代码味道 107 —变量重用

> 原文：<https://blog.devgenius.io/code-smell-107-variables-reuse-222a15b1a819?source=collection_archive---------4----------------------->

## 重用变量使得作用域和边界更难遵循

![](img/7d9b9e68b48cc5fde3a2d92b6295fb47.png)

照片由[西格蒙德](https://unsplash.com/@sigmund)在 [Unsplash](https://unsplash.com/s/photos/recycle) 上拍摄

> TL；DR:不要为了不同的目的读写同一个变量

# 问题

*   可读性
*   隐藏的问题

# 解决方法

1.不要重用变量

2.[提取方法](/refactoring-002-extract-method-1478f421b35a)分离范围

# 语境

编写脚本时，重用变量是很常见的。

这导致了混乱，并使调试更加困难。

我们应该尽可能缩小范围。

# 示例代码

## 错误的

```
// print line total
double total = item.getPrice() * item.getQuantity();
System.out.println(“Line total: “ + total );// print amount total 
total = order.getTotal() — order.getDiscount();
System.out.println( “Amount due: “ + total );// variable is reused
```

## 对吧

```
function printLineTotal() {
 double total = item.getPrice() * item.getQuantity();
 System.out.println(“Line total: “ + total );
}function printAmountTotal() {
 double total = order.getTotal() — order.getDiscount();
 System.out.println( “Amount due: “ + total );
}
```

# 侦查

[X]自动

Linters 可以使用解析树来查找变量定义和用法。

# 标签

*   可读性

# 结论

避免重复使用变量名。使用更具体和不同的名称。

# 关系

[](/code-smell-03-functions-are-too-long-accea7eb4ae9) [## 代码气味 03 —函数太长

### 人类过了 10 线就烦了。

blog.devgenius.io](/code-smell-03-functions-are-too-long-accea7eb4ae9) 

# 更多信息

[](/refactoring-002-extract-method-1478f421b35a) [## 重构 002 —提取方法

### 找到一些可以分组并被原子调用的代码片段。

blog.devgenius.io](/refactoring-002-extract-method-1478f421b35a) 

> 先简单后通用，先使用后重用。

凯夫林·亨尼

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) 

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*