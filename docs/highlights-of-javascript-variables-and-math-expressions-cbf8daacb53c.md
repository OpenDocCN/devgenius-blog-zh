# JavaScript 的亮点——变量和数学表达式

> 原文：<https://blog.devgenius.io/highlights-of-javascript-variables-and-math-expressions-cbf8daacb53c?source=collection_archive---------6----------------------->

![](img/52510c755ffe2f692af1f08683c9eadc.png)

照片由 [Antoine Dautry](https://unsplash.com/@antoine1003?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

学习 JavaScript，必须先学基础。

在本文中，我们将研究 JavaScript 语言的最基本部分。

# 变量命名

我们不能有以数字开头的变量名，但是我们可以有中间有数字的变量。

例如，我们可以写:

```
let name1 = "james";
```

但不是:

```
let 1name = "james";
```

JavaScript 变量命名有更多的规则。

变量名不能有任何空格。

变量名只能有字母、数字、美元符号和下划线。

变量名不能是保留的 JavaScript 关键字，但可以包含它们。

可以使用大写字母，但是我们必须知道变量名是区分大小写的。

所以`name`和`Name`不一样。

JavaScript 变量名通常是驼色。他们让我们用多个单词组成变量，我们可以很容易地读懂它们。

所以我们可以像写`firstName`一样写变量名，让变量名中的两个字清晰明了。

# 数学表达式

大多数 JavaScript 程序都会做数学。因此，JavaScript 附带了用于各种常见数学运算的运算符。

例如，我们可以用`+`操作符添加:

```
let num = 2 + 2;
```

要做减法，我们可以使用`-`运算符:

```
let num = 2 - 2;
```

乘法由`*`运算符完成:

```
let num = 2 * 2;
```

除法用`/`运算符完成:

```
let num = 2 / 2;
```

还有一个`%`模数操作符，用来计算左操作数除以右操作数的余数。

例如，我们可以写:

```
let num = 2 % 2;
```

我们得到 0 是因为 2 能被它自己整除。

取幂运算符用`**`表示。例如，我们可以这样使用它:

```
let num =  2 ** 2;
```

然后我们把 2 提高到 2 次方。

`++`运算符将变量值加 1，并将其赋给变量。

例如:

```
num++;
```

与以下内容相同:

```
num = num + 1;
```

我们可以使用带有赋值操作符的表达式，但是我们可能会得到意想不到的结果。

例如，如果我们有:

```
let num = 1; 
let newNum = num++;
```

那么`newNum`为 1，因为`num`的初始值被返回。

另一方面，如果我们有:

```
let num = 1; 
let newNum = ++num;
```

那么`newNum`为 1，因为如果`++`在`num`之前，则返回`num`的最新值。

同样，JavaScript 使用了`--`操作符将变量减 1，并将最新值赋给变量。

例如，我们可以写:

```
let num = 1; 
let newNum = num--;
```

我们为`newNum`得到 1，并且:

```
let num = 1; 
let newNum = --num;
```

然后我们为`newNum`得到 0。

返回值的工作方式与`++`相同。

# 结论

我们必须遵循 JavaScript 的规则来命名变量。

此外，JavaScript 附带了许多用于数学运算的运算符。