# Python 编程教程:变量和数据类型

> 原文：<https://blog.devgenius.io/introduction-to-python-programming-variables-and-data-types-ef29710fad9e?source=collection_archive---------8----------------------->

![](img/186a336f7bf6e6b3f2fdfe64dad4dc59.png)

摄影:unsplash.com 的伊恩·泰勒

# 什么是变量？

**变量**用于存储数据。可以在代码中引用或操作存储的数据。换句话说，您可以将变量视为保存数据的容器。

> 这篇文章是我的 [Python 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad)系列的一部分。

# 如何在 Python 中声明变量？

一旦赋值，就创建了一个变量。Python 没有用于声明变量的关键字或命令。

例如

```
firstVar = "Hello World"print(firstVar)
```

输出将是:

```
Hello World
```

上面的例子显示了单词 firstVar 被声明为变量，方法是对它赋值 Hello World。

您也可以通过分配一个较新的变量来更改变量中的值。

例如

```
firstVar = "Hello World"
firstVar = "Hi Earth"print(firstVar)
```

输出将是:

```
Hi Earth
```

# Python 数据类型

数据类型是可以存储在变量中的不同种类的值。如前所述，python 没有创建变量的关键字或命令。声明数据类型也是如此。它也是在赋值后创建的。

# 数据类型:字符串

字符串存储多个字符。在 python 中，字符串要么用单引号括起来，要么用双引号括起来。

例如

```
singleQuote = 'Hello World'
doubleQuote = "Hi Earth"print(singleQuote)
print(doubleQuote)
```

输出将是:

```
Hello World
Hi Earth
```

# 数据类型:整数

Integer 存储可以是正数也可以是负数整数。这也包括零。

例如

```
numberOne = 1
numberTwo = 2
numberThree = numberOne + numberTwoprint("One plus Two is equal to", numberThree)
```

输出将是:

```
One plus Two is equal to 3
```

# 数据类型:浮点型

用小数点或分数存储数字。

例如

```
withDecimal = 100.19
half = 1/2print(half)
print(withDecimal * half)
```

输出将是:

```
0.5
50.095
```

# 数据类型:复数

与任何编程语言不同，python 已经有了处理实数系统的默认支持。

例如

```
complexSample = 3.14*j* dataType = type(complexSample)print(dataType)
```

输出将是:

```
<class 'complex'>
```

> **注意:**我们用 **type** 来标识什么数据类型。我建议您尝试将它用于其他数据类型，如 string 和 integer，以了解它的更多功能。

# 数据类型:布尔型

布尔存储真或假。

例如

```
fact = True
fiction = False
combineFactFiction = True & Falseprint("Can we mix fact and fiction to tell the truth?", combineFactFiction)
```

输出将是:

```
Can we mix fact and fiction to tell the truth? False
```

> 符号&表示逻辑 AND，这将在单独的教程中讨论。

*看到了！就连计算机本身也知道，在谈到真相时，你不能把事实和虚构混为一谈。*

我们还没有完成数据类型。在下一篇文章中，我们将讨论[序列数据类型](https://arc-sosangyo.medium.com/introduction-to-python-programming-sequence-list-tuples-and-range-1caa03ffd55c)。

愿法典与你同在，

-电弧