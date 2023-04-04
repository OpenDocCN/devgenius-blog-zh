# PHP 基本编程第 8 卷:数组

> 原文：<https://blog.devgenius.io/php-basic-programming-vol-8-array-a802b3b5aaba?source=collection_archive---------4----------------------->

![](img/dce6798344e513765894de26feb8a295.png)

朋友们好。也许我们很多人已经熟悉了什么是编程语言，特别是什么是 PHP 编程语言。这次我们将继续讨论 PHP 编程的基础知识。我们直接进入第一个讨论。

# 使用数组创建程序

![](img/843e50f42e50b4cd2793b4a68ffd0710.png)![](img/32e1d4f518ad7c9260c3eb7f65962dc2.png)

# 索引数组

## 什么是数组？

数组是一种数据结构，它包含一组数据并有一个索引。索引用于访问数组值。数组索引总是从零(0)开始。

![](img/a5fe46e33a75fdfef76228b3176def27.png)

## 在 PHP 中创建数组

PHP 中的数组可以用 array()函数和方括号[]创建。数组可以用任何数据类型填充。

![](img/1963e2e3a2a9ef4f7d6ed7317d7ac480.png)![](img/c668b9c1747493aef0226211dded79cd.png)

## 显示数组的内容

为了显示数组的内容，我们可以通过索引来访问它。

![](img/e59f79216e3ee374988dddee3d1fdf3f.png)![](img/2e564c70e24d48a72119554510a9a520.png)

也可以使用循环

![](img/ac501752ef0d57b9ae050f99619e9863.png)

## 删除数组的内容

要删除数组的内容，我们可以使用 unset()函数。该功能也可用于删除变量。

![](img/f8f1c81f25cc3d7945035831c955a656.png)![](img/ca886ec097c6c35f68034a82c980894a.png)

## 添加数组

有两种方法可以增加数组的内容:直接填充到你想要添加的索引号直接填充到最后一个索引

![](img/e13f3325b5d838ab211d824bbea80021.png)![](img/fd4dceb44a6ec11ac5e78f33436ec525.png)

如果我们添加到已经有内容的索引中，那么新的内容将被覆盖。

![](img/f673242b8c6ef2dccf60fd5591d54001.png)![](img/6830426e3cabf3a00677d3818139252b.png)

## 排序数组

![](img/3e621e23dedad5e7fa9d287fbd41bd2a.png)

## 排序数组(反向)

![](img/99383016f9c09d3645e7d04f56c2423c.png)

# 关联数组

## 什么是关联数组？

关联数组是其索引不使用数字或数字的数组。关联数组索引是关键字的形式。

![](img/2c5646d7bfa72899de4c40534f97f9eb.png)![](img/7956967d3890ff438010a50d6942fc31.png)

在关联数组中，我们使用= >符号将关键字与数组的内容关联起来。除了使用= >符号，我们还可以创建一个这样的关联数组:

![](img/8b4dbba7e78cb2e921eee8efa5b03125.png)![](img/6e761681119e9466658a3c3fca45a03c.png)

# 多维数组

## 什么是多维数组？

多维数组是具有多个维度的数组。通常用于创建矩阵、图形和其他复杂的数据结构。

![](img/f9965d89c05411c67cba77810ba98ce1.png)![](img/18ce5c879fa8189773d3ab7161b38d23.png)![](img/df4af5d667a55e274d20d7ac09519ca2.png)![](img/b88ca7abff683a50d7b9868f4d4ecaa7.png)

## 显示多维数组

![](img/5003884145c35862846c884e5b35203c.png)

# 结论

我们已经得出结论。从我们的讨论中得出的结论是，数组可以用来存储多种类型的数据，因此它们非常灵活且易于组织..我们将在下一篇文章中继续讨论基本 PHP。希望这篇文章能有用。

谢谢你。

# 参考

*   [https://www.petanikode.com/php-array/](https://www.petanikode.com/php-array/)
*   https://www.w3schools.com/php/