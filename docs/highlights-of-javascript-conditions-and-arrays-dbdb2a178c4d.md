# JavaScript 亮点—条件和数组

> 原文：<https://blog.devgenius.io/highlights-of-javascript-conditions-and-arrays-dbdb2a178c4d?source=collection_archive---------5----------------------->

![](img/0229c9323c61054bcefe1438654e51dc.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

学习 JavaScript，必须先学基础。

在本文中，我们将研究 JavaScript 语言的最基本部分。

# 测试条件集

我们可以用一个`if`语句测试多个条件。

要组合它们，我们可以使用`&&`或`||`操作符。

`&&`是逻辑与运算符，`||`是逻辑或运算符。

例如，我们可以写:

```
if (weight > 300 && time < 6) {
  alert("athlete");
} else {
  alert("chef");
}
```

一起检查`weight`和`time`的值。

如果`weight`小于 300 且`time`小于 6，则运行第一个程序块。

否则，运行第二个模块。

我们可以将更多条件链接在一起:

```
if (weight > 300 && time < 6 && age > 17 && gender === "male") {
  alert("athlete");
} else {
  alert("chef");
}
```

这将测试所有条件是否都是`true`。如果都是`true`，那么运行第一个块，

否则，运行第二个程序块。

可以以类似的方式使用`||`操作符。

例如，我们可以写:

```
if (SAT > avg || GPA > 2.5 || sport === "football") {
  alert("accepted");
} else {
  alert("not accepted");
}
```

如果任一条件返回`true`，则`||`操作符返回`true`。

我们可以把`||`和`&&`混在一起。

例如，我们可以写:

```
if (age > 65 || age < 21 && res === "U.S.")
```

`&&`优先，所以先评估 `res === “U.S.”`。

并且接下来评估`||`操作符。

为了使这一点更清楚，我们应该用括号将表达式括起来，这样每个人都可以很容易地阅读:

```
if ((age > 65 || age < 21) && res === "U.S.")
```

# 嵌套的 if 语句

我们可以嵌套`if`语句。例如，我们可以写:

```
if (c === d) {
  if (x === y) {
    g = h;
  } else if (a === b) {
    g = h;
  } else {
    v = u;
  }
} else {
  v = u;
}
```

我们用两个空格嵌套内部表达式，使它们易于键入和阅读。

我们不应该有太多层次的嵌套，因为它很难阅读。

# 数组

我们的程序中经常需要存储许多值。

为了更简单，我们可以使用数组。

例如，代替重复声明变量来存储相似的值:

```
let city0 = "Oslo";
let city1 = "Rome";
let city2 = "Toronto";
let city3 = "Denver";
let city4 = "Los Angeles";
let city5 = "Seattle";
```

我们可以将它们放入一个数组中:

```
let cities = ["Oslo", "Rome", "Toronto", "Denver", "Los Angeles", "Seattle"];
```

数组对于存储固定大小或动态大小的数据列表非常有用。

我们可以通过数组的索引来访问它。

例如，我们可以写:

```
cities[2]
```

JavaScript 可以是不同类型的数组。例如，我们可以写:

```
let mixedArray = [1, "james", "mary", true];
```

第一个条目的索引是 0。变量的相同命名规则也适用于数组。

像其他变量一样，数组只能声明一次。

# 结论

我们可以用`if`语句测试多个条件。

它们也可以嵌套。

数组对于存储数据集合很有用。