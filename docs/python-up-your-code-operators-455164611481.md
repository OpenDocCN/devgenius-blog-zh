# Python Up 您的代码:运算符

> 原文：<https://blog.devgenius.io/python-up-your-code-operators-455164611481?source=collection_archive---------8----------------------->

## Python 中运算符的概述

![](img/99993c3924d8298719e3985088ea632c.png)

Pawel Czerwinski 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

简而言之，Python 中(一般来说，编程中)的运算符是用于执行特定数学、关系或逻辑运算的符号/结构，这些运算将产生有形的具体结果值。

Python 中有多种类型的操作符，我们将逐一介绍它们。

## 算术运算符

这些运算符用于执行基本的算术运算，如求和、乘积、除法等。Python 中至少有 7 个算术运算符:

*   **加法** ( `+` ) —将两个操作数相加；
*   **减法** ( `-` ) —从一个操作数中减去另一个操作数；
*   **乘法** ( `*` ) —一个操作数乘以另一个操作数；
*   **除法** ( `/` ) —一个操作数除以另一个操作数；
*   **模数** ( `%` ) —两个操作数相除的余数；
*   **取幂** ( `**` ) —一个操作数的另一个操作数的幂；
*   **Floor****division**(`//`)—返回两个操作数相除的 Floor 输出

```
*# addition*
result = 3 + 2
**print**(f"3 + 2 = {result}")*# subtraction*
result = 2 - 3
**print**(f"2 - 3 = {result}")*# multiplication*
result = 3 * 2
**print**(f"3 * 2 = {result}")*# division*
result = 3 / 2
**print**(f"3 / 2 = {result}")*# modulus*
result = 3 % 2
**print**(f"3 % 2 = {result}")*# exponentiation*
result = 3 ** 2
**print**(f"3 ** 2 = {result}")*# floor division*
result = 3 // 2
**print**(f"3 // 2 = {result}")Output:
3 + 2 = 5
2 - 3 = -1
3 * 2 = 6
3 / 2 = 1.5
3 % 2 = 1
3 ** 2 = 9
3 // 2 = 1
```

## 比较运算符

顾名思义，这些运算符用于执行操作数之间的比较。Python 中目前有 6 个这样的比较运算符:

*   **小于** ( `<` ) —如果第一个操作数小于第二个操作数，则计算为`True`，否则为`False`；
*   **小于或等于** ( `<=` ) —如果第一个操作数小于或等于第二个操作数，则计算为`True`，否则为`False`；
*   **大于** ( `>` ) —如果第一个操作数大于第二个操作数，则计算为`True`，否则为`False`；
*   **大于或等于** ( `>=` ) —如果第一个操作数大于或等于第二个操作数，则计算为`True`，否则为`False`；
*   **等于** ( `==` ) —如果两个操作数的值相等，则计算为`True`，否则为`False`；
*   **不等于** ( `!=` ) —如果两个操作数的值不同，则计算为`True`，否则为`False`—这与**等于** ( `==`)正好相反。

```
*# lt* result = 1 < 2
**print**(f"is 1 less than 2?: {result}")*# lte* result = 3 <= 3
**print**(f"is 3 less than or equal to 3?: {result}")*# gt* result = 3 < 2
**print**(f"is 3 greater than 2?: {result}")*# gte* result = 3 >= 3
**print**(f"is 3 greater than or equal to 3?: {result}")*# eq* result = 4 == 4
**print**(f"is 4 equal to 4?: {result}")*# neq* result = 4 == 4.0
**print**(f"is 4 not equal to 4.0?: {result}")Output:
is 1 less than 2?: True
is 3 less than or equal to 3?: True
is 3 greater than 2?: False
is 3 greater than or equal to 3?: True
is 4 equal to 4?: True
is 4 not equal to 4.0?: True
```

## 按位运算符

它们用于执行逻辑运算，如与、或、非、异或等。让我们一个一个地看一下:

*   AND:`&`运算符对两个操作数执行按位 AND 运算:

```
*# the AND operation returns 1 when both operands are 1.
# Bitwise AND means we'll perform AND operations on the
# two numbers below, bit by bit, starting with the rightmost bits:
# 11 (decimal) = 1011 (binary)
# 13 (decimal) = 1101 (binary)
# ------------------------------------------
# 11 & 13      = 1001 (binary) = 9 (decimal)***print**(11 & 13)Output:
9
```

*   OR:`|`运算符对两个操作数执行按位 OR 运算:

```
*# the OR operation returns 1 when at least one operand is 1.
# Bitwise OR means we'll perform OR operations on the
# two numbers below, bit by bit, starting with the rightmost bits:
# 11 (decimal) = 1011 (binary)
#  9 (decimal) = 1001 (binary)
# ------------------------------------------
# 11 | 9       = 1011 (binary) = 11 (decimal)***print**(11 | 9)Output:
11
```

*   XOR:`^`运算符对两个操作数执行按位 XOR 运算:

```
*# the XOR operation returns 1 when exactly one operand is 1,
# while the other is 0.
# Bitwise XOR means we'll perform XOR operations on the
# two numbers below, bit by bit, starting with the rightmost bits:
# 11 (decimal) = 1011 (binary)
# 13 (decimal) = 1101 (binary)
# ------------------------------------------
# 11 ^ 13      = 0110 (binary) = 6 (decimal)***print**(11 ^ 13)Output:
6
```

*   NOT:`~`运算符对操作数执行 NOT 运算:

```
*# the NOT operation effectively turns all 0 bits into 1
# and vice-versa. Or, in other words, it returns one's
# complement of the number
# 11 (decimal) = 1011 (binary)
# ---------------------------------------------------
# ~11          = -(1011 + 1) (binary) = -12 (decimal)**# 11 (decimal) = 
# 0000000000000000000000000000000000000000000000000000000000001011
# ~11 = -12 (decimal) = 
# 1111111111111111111111111111111111111111111111111111111111110100***print**(~11)Output:
-12
```

*   左移:`<<`运算符将左操作数的所有位左移，其位数等于右操作数的值。空出的位置被有效地用 0 填充。实际上，`a << b`相当于以下内容:`a * (2 ** b)`(将`a`的位左移若干个`b`位置):

```
*# the left shift operation shifts all of the bits
# to the left, a number of positions:
# 11 (decimal) = 1011   (binary)
# 11 << 2      = 110100 (binary) = 44***print**(11 << 2)Output:
44
```

*   右移:`>>`运算符将左操作数的所有位向右移动，移动的位置数等于右操作数的值。移位会覆盖在此过程中丢失的许多位。本质上，`a >> b`操作相当于`a // (2 ** b)`(将`a`的位向右移动若干个`b`位置):

```
*# the right shift operation shifts all of the bits
# to the right, a number of positions:
# 44 (decimal) = 101100 (binary)
# 44 >> 2      = 001101 (binary) = 11***print**(44 >> 2)Output:
11
```

## 赋值运算符

这些运算符用于给变量赋值。我们会发现，实际上，只有一个真正的赋值操作符和十二个快捷操作符在实际赋值之前执行特定的操作。我们马上就会明白这意味着什么:

*   这是我们刚刚提到的赋值操作符。它有效地将右操作数(变量或表达式)的值赋给左操作数(通常是变量):

```
a = 2
**print**(a)b = a + 5
**print**(b)Output:
2
7
```

*   `+=`:加法赋值。将两个操作数之和赋给左操作数:

```
*# regular assignment*
a = 2
a = a + 3
**print**(a)*# addition assignment*
a = 2
a += 3
**print**(a)Output:
5
5
```

从上面可以看出，第一个例子中的`a`最初的值是 2。然后，`a = a + 3`被执行了。因此，`a + 3`被评估为 5，并被分配到`a`。第二个例子只是用快捷加法赋值运算符`a += 3`替换了`a = a + 3`，也可以理解为*将变量* `*a*`加 3，或者*将* `*a*` *增加 3* 。

*   `-=`:减法赋值。类似于加法赋值，但不是 sum，而是两个操作数之间的差被赋值给左操作数:

```
*# regular assignment*
a = 3
a = a - 2
**print**(a)*# subtraction assignment*
a = 3
a -= 2
**print**(a)Output:
1
1
```

加法赋值操作符使我们能够用快捷方式`a -= 2`成功地替换第一个例子中的常规赋值操作(`a = a — 2`)，这可以翻译成类似于*减 2* 。

*   `*=`:产品分配。将两个操作数的乘积结果赋给左操作数:

```
*# regular assignment*
a = 3
a = a * 2
**print**(a)*# product assignment*
a = 3
a *= 2
**print**(a)Output:
6
6
```

这个快捷操作符将`a = a * 2`翻译成可读性更好，但绝对更简单的`a *= 2`来获得相同的结果。

*   `/=`:分部分配。将两个操作数相除的结果赋给左边的操作数:

```
*# regular assignment*
a = 3
a = a / 2
**print**(a)*# division assignment*
a = 3
a /= 2
**print**(a)Output:
1.5
1.5
```

*   `%=`:模数分配。计算两个操作数相除的余数，并将其赋给左操作数:

```
*# regular assignment*
a = 3
a = a % 2
**print**(a)*# modulus assignment*
a = 3
a %= 2
**print**(a)Output:
1
1
```

*   `//=`:楼层划分分配。对两个操作数执行浮点除法，并将结果赋给左边的操作数:

```
*# regular assignment*
a = 3
a = a // 2
**print**(a)*# floor division assignment*
a = 3
a //= 2
**print**(a)Output:
1
1
```

*   `**=`:取幂赋值。对两个操作数执行取幂运算，并将结果赋给左边的操作数:

```
*# regular assignment*
a = 3
a = a ** 2
**print**(a)*# exponentiation assignment*
a = 3
a **= 2
**print**(a)Output:
9
9
```

*   `&=`:和赋值。对两个操作数执行按位 AND 运算，并将结果赋给左边的操作数。在下面的例子中，我们的两个操作数的值分别是 11 和 13。对这两个值进行简单的按位 AND 运算，得到的结果正好是 9:

```
*# the AND operation returns 1 when both operands are 1.
# Bitwise AND means we'll perform AND operations on the
# two numbers below, bit by bit, starting with the rightmost bits:
# 11 (decimal) = 1011 (binary)
# 13 (decimal) = 1101 (binary)
# ------------------------------------------
# 11 & 13      = 1001 (binary) = 9 (decimal)*
```

下面进一步演示了该示例:

```
*# regular assignment*
a = 11
a = a & 13
**print**(a)*# and assignment*
a = 11
a &= 13
**print**(a)Output:
9
9
```

*   `|=`:或分配。对两个操作数执行按位“或”运算，并将结果赋给左边的操作数。在下面的例子中，我们的两个操作数的值分别是 11 和 9。对这两个值进行简单的按位“或”运算正好得到 11:

```
*# the OR operation returns 1 when at least one operand is 1.
# Bitwise OR means we'll perform OR operations on the
# two numbers below, bit by bit, starting with the rightmost bits:
# 11 (decimal) = 1011 (binary)
#  9 (decimal) = 1001 (binary)
# ------------------------------------------
# 11 | 9       = 1011 (binary) = 11 (decimal)*
```

下面进一步演示了该示例:

```
*# regular assignment*
a = 11
a = a | 9
**print**(a)*# or assignment*
a = 11
a |= 9
**print**(a)Output:
11
11
```

*   `^=`:异或赋值。对两个操作数执行按位 XOR 运算，并将结果赋给左边的操作数。在下面的例子中，我们的两个操作数的值分别是 11 和 13。对这两个值进行简单的按位“或”运算正好得到 6:

```
*# the XOR operation returns 1 when exactly one operand is 1,
# while the other is 0.
# Bitwise XOR means we'll perform XOR operations on the
# two numbers below, bit by bit, starting with the rightmost bits:
# 11 (decimal) = 1011 (binary)
# 13 (decimal) = 1101 (binary)
# ------------------------------------------
# 11 ^ 13      = 0110 (binary) = 6 (decimal)*
```

下面进一步演示了该示例:

```
*# regular assignment*
a = 11
a = a ^ 13
**print**(a)*# xor assignment*
a = 11
a ^= 13
**print**(a)Output:
6
6
```

*   `>>=`:右移赋值。对操作数执行按位右移，并将结果赋给左操作数:

```
*# the right shift operation shifts all of the bits
# to the right, a number of positions:
# 44 (decimal) = 101100 (binary)
# 44 >> 2      = 001101 (binary) = 11*
```

从上面可以看出，**对于某种类型的数字来说——**这些数字被称为两个的**次方，我们都很熟悉其中的一些(2、4、8、16、32、64、128、256、512、1024 等等。)，相当于将数字除以 2 的`n`次方，其中`n`是要移位的位置数。在我们的例子中，我们将所有的位向右移动了两次，所以就像说*用 4 除 44*。**

不幸的是，对于奇数来说，这并不成立。尝试在比如 11 上执行 1 位置右移，将得到 5:

```
*# 11 (decimal) = 1011 (binary)
# 11 >> 1      = 0101 (binary) = 5*
```

然而，我们可以认为**右移**操作等同于我们之前经历的**地板分区**操作:

```
*#* ***a >> b*** *would be equivalent to* ***a // (2 ** b)***
```

最后，我们来总结一下如何成功使用右移位赋值运算符:

```
*# regular assignment* a = 44
a = a >> 2
**print**(a)*# right shift assignment* a = 44
a >>= 2
**print**(a)Output:
11
11
```

*   `<<=`:左移分配。对操作数执行按位左移，并将结果赋给左操作数:

```
*# the left shift operation shifts all of the bits
# to the left, a number of positions:
# 11 (decimal) = 1011   (binary)
# 11 << 2      = 110100 (binary) = 44*
```

从上面可以看出，这相当于将数字乘以 2 的`n`次方，其中`n`是要移动的位置数。在我们的例子中，我们将所有的位左移两次，所以就像说*用 4 乘以 11*。

下面进一步演示了该示例:

```
*# regular assignment*
a = 11
a = a << 2
**print**(a)*# left shift assignment*
a = 11
a <<= 2
**print**(a)Output:
44
44
```

## 逻辑运算符

Python 中的条件表达式广泛使用了三种逻辑运算符，我们将介绍如下:

*   逻辑与—应用于两个操作数，如果两个操作数都是`True`，则计算结果为`True`。如果其中至少有一个是`False`，则表达式求值为`False`；
*   逻辑 OR —应用于两个操作数，如果两个操作数中的任何一个为`True`，则计算结果为`True`。如果两者都是`False`，则表达式求值为`False`；
*   逻辑非—应用于一个操作数时，它反转操作数的布尔值。如果是`True`，操作评估为`False`，反之亦然:

```
*# initialize two boolean variables*
a = **True**
b = **False***# perform some logical operations* **print**(a **and** b)
**print**(a **or** b)
**print**(**not** b)Output:
False
True
True
```

## 成员运算符

这是本文中非常简短但极其重要的小节。因为 Python 中只有两个成员操作符，但它们被广泛用作检查一个值是否出现在序列中的最佳方式。这两个运算符是:

*   `in`；
*   `not in`。

我们可以有效地使用它们来检查一个值是否是整数列表的成员，是否在字符串中找到了一个字符，或者是否在字典中找到了一个键，这样的例子还可以继续下去。如果是这种情况，那么运算结果为`True`，否则，如果值不包含在我们的序列中，我们的结果将为`False`。

```
*# list of integers*
my_list = [0, 1, 2, 3, 4, 5]**print**(9 **in** my_list)
**print**(4 **in** my_list)
**print**(8 **not** **in** my_list)Output:
False
True
True
```

## 标识运算符

类似于成员操作符的重要性，Python 中的标识操作符也被广泛使用。他们只有两个人:

*   `is`；
*   `is not`。

它们用于检查两个操作数是否是同一个对象:

```
*# integers are immutable*
a = 2
b = 2
**print**(a **is** b)*# lists are mutable*
list_a = [1, 2, 3]
list_b = [1, 2, 3]
**print**(list_a **is not** list_b)Output:
True
True
```

第一个例子中有两个整型变量被声明为具有相同的值。因为整数是不可变的，它们有相同的 id，所以它们确实是同一个对象。

在第二个例子中，两个列表以相同的顺序被声明为具有相同的元素。然而，由于列表是可变的，两个 id 不会相同，所以即使列表碰巧具有相同的值，它们也不会指向同一个对象。

关于 Python 中的操作符的话题，这就差不多了。最重要的是，我们要先了解这些经营者，以便更好地了解我们可以通过他们做些什么来改进我们的工艺。

此外，可能比运算符本身更重要的是，运算符优先级表应该为我们提供很多帮助。当然，最可靠的来源是官方文档，[这里的](https://docs.python.org/3/reference/expressions.html#operator-precedence)是 Python 中运算符优先级的表格。它解释了哪些操作符首先被求值，我们是否应该有包含多类操作符的复杂表达式。

话虽如此，干杯，期待[下一个](https://medium.com/@deck451/python-up-your-code-scopes-72f367e26b09)一个。编码快乐！

*Deck 是软件工程师、导师、作家，有时甚至是老师。他拥有 12 年以上的软件工程经验，现在是 Python 编程语言的真正倡导者，同时他的热情是帮助人们提高他们的 Python(以及一般的编程)技能。你可以在* [*【领英】*](https://www.linkedin.com/in/deck451/)*[*【脸书】*](https://www.facebook.com/deck451/)*[*推特*](https://twitter.com/Deck45100)*[*不和谐*](https://discord.com) *: Deck451#6188，以及跟随他写在这里的* [*中*](https://medium.com/@deck451)***