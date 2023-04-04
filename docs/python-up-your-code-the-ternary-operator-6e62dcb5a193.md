# Python Up 您的代码:三元运算符

> 原文：<https://blog.devgenius.io/python-up-your-code-the-ternary-operator-6e62dcb5a193?source=collection_archive---------7----------------------->

## 展示三元运算符及其在 Python 中的用法

![](img/57db6e5bff78aedb69df435c5948528e.png)

[Zak](https://unsplash.com/@zaks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/neon-abstract?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

## 什么是三元运算符？

Python 具有广泛的[操作符](https://medium.com/@deck451/python-up-your-code-operators-455164611481)，比如算术、按位、标识、逻辑、成员、比较等等。但是一些编程语言，比如 C，或者 Java，也有我们所说的**三元**运算符。

一般来说，三元运算符实现的是根据条件表达式将两个可能值中的一个赋给操作数。如果条件表达式评估为`True`，那么前一个值将被分配给我们的操作数。相反，如果条件表达式的值为`False`，那么后一个值将被赋给我们的操作数。

## Python 中的三元运算符是什么？

Python 也不例外。它也有这样一个运营商。除了不同于 C 或 Java，它们有自己的语法，在类似于`condition ? value_if_true : value_if_false`的表达式中使用类似于`?`和`:`的字符，Python 不需要这些特殊字符来实现这个功能。Python 通过我们称之为**条件语句**，或者更不夸张地说，一个**内联 if** 来确保三元运算符的功能。

## 使用三元运算符如何简化我的代码？

让我们考虑一段简单的代码:

```
a = 2**if** a == 2:
    b = "Yes"
**else**:
    b = "No"
```

通过这种方式，我们确保了`b`得到一个或另一个值，这取决于`a`是否等于`2`。这是 4 行代码。当然，简化它的一种方法是去掉结尾的`else`,写成:

```
a = 2
b = "No"**if** a == 2:
    b = "Yes"
```

我们会立即将`b`初始化为`No`的值，只有当条件评估为`True`时，才会将值更改为`Yes`。但是对于 Python 来说，这似乎还是有点太多了。

下面是我们实现三元运算符的几种方法。

*   **条件语句/内联 if:**

```
a = 2
b = "Yes" **if** a == 2 **else** "No"
```

和以前一样的功能，但是对于手头的问题有一个更简洁和优雅的解决方案。这实际上是这样一个条件语句的语法:

```
operand = value_if_true **if** condition **else** value_if_false
```

首先，对`condition`进行评估，然后做出决定。如果评估为`True`，则为`operand = value_if_true`。否则，如果`condition`被评估为`False`，则`operand = value_if_false`。

使用它有一些缺点:

1.  像`if-elif-else`这样的情况不能转换成三元运算符单行程序；
2.  简单的`if`语句也是如此。Python 中的三元运算符，以及任何支持它的编程语言中的三元运算符，都需要两个可供选择的值。这在 Python 中的意思是，条件语句确实需要采用`if-else`的形式。简单的`if`语句不能转换成条件表达式。例如，如果我们执行下面这行代码:`b = “Yes” if a == 2`，Python 不会很喜欢这样，它会给我们这样的代码:

```
b = "Yes" if a == 2
        ^^^^^^^^^^^^^^^
SyntaxError: expected 'else' after 'if' expression
```

*   **从元组中挑选正确的元素:**

```
a = 2
b = ("No", "Yes") [a == 2]
```

请注意，元组伴随着要评估的条件。如果条件评估为`True`，那么将选择索引`1`处的元素，并将其值分配给`b`。但是如果它是`False`，那么它是索引`0`处的元素，该元素将找到其被分配给`b`的值。

*   **从字典中选择正确的值:**

```
b = {**False**: "No", **True**: "Yes"} [a == 2]
```

这个甚至比元组挑选更简单。该条件显然将评估为`True`或`False`。这两个键在我们的字典中都是实际的键，根据条件评估结果，这个键后面的值将被分配给`b`。

*   **使用元组选择无参*λ*函数:**

```
b = (**lambda**: "No", **lambda**: "Yes") [a == 2]()
```

同用一个`tuple`的值一样，这次我们用了一个`lambda` 的`tuple`函数。对`a == 2`条件的评估将是`True`或`False`。如果条件为`True`，将选择第二个(index = 1)λ。如果条件是`False`，那么第一个(index = 0) lambda 将被选中。最后一个`()`在那里，因为我们需要实际运行 lambda 函数，而不是仅仅从元组中选择它。该λ的输出将被分配给`b`。

*   **使用 Lambda 函数的三元运算符:**

```
b = (**lambda** a: "Yes" **if** a == 2 **else** "No")
```

另一个很好的例子，来自[马丁·泰勒](https://medium.com/@mnjteller):

```
b = (**lambda** a: a == 2 **and** "Yes" **or** "No")
```

在这两种情况下，lambda 函数被分配给`b`。我们只需要调用`b`并给它提供`a`参数(例如`b(2)`，然后函数执行并给我们提供`Yes`或`No`。

下面，我为我刚才介绍的所有替代方案设置了一个合适的演示代码:

```
*# initialize our variable*
a = 2*# # original code
# if a == 2:
#     b = "Yes"
# else:
#     b = "No"**# ternary operator using conditional statement* b = "Yes" **if** a == 2 **else** "No"
**print**(b)*# picking an element from a tuple* b = ("No", "Yes") [a == 2]
**print**(b)*# picking a value from a dictionary*
b = {**False**: "No", **True**: "Yes"} [a == 2]
**print**(b)*# using argumentless lambda functions*
b = (**lambda**: "No", **lambda**: "Yes") [a == 2]()
**print**(b)*# using lambda functions*
b = (**lambda** a: "Yes" **if** a == 2 **else** "No")(2)
**print**(b)b = (**lambda** a: a == 2 **and** "Yes" **or** "No")(2)
**print**(b) Output:
Yes
Yes
Yes
Yes
Yes
Yes
```

这篇文章到此结束。希望这对你们有所帮助，我们下次[再见！编码快乐！](https://medium.com/@deck451/python-up-your-code-iterators-af5331f68ccd)

*Deck 是软件工程师、导师、作家，有时甚至是老师。他拥有 12 年以上的软件工程经验，现在是 Python 编程语言的真正倡导者，同时他的热情是帮助人们提高他们的 Python(以及一般的编程)技能。你可以在*[*Linkedin*](https://www.linkedin.com/in/deck451/)*，* [*脸书*](https://www.facebook.com/deck451/) *，* [*推特*](https://twitter.com/Deck45100) *，以及* [*不和谐*](https://discord.com) *: Deck451#6188，以及跟随他在这里写作的* [*媒体*](https://medium.com/@deck451)