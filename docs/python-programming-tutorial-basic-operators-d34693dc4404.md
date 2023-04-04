# Python 编程教程:基本运算符

> 原文：<https://blog.devgenius.io/python-programming-tutorial-basic-operators-d34693dc4404?source=collection_archive---------4----------------------->

![](img/c6259766cc5034be0d22899f0c6c3dd5.png)

摄影:Jannis Klemm 在 unsplash.com

> 这篇文章是我的 [Python 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad)系列的一部分。

运算符是可以执行特定操作(如数学、逻辑或关系运算)的单个或多个符号。

以下是基本的 python 运算符:

*   **任务**
*   **算术**
*   **对比**
*   **逻辑**

# 赋值运算符

赋值运算符从左操作数到右操作数赋值。声明赋值运算符的符号是 **=** 。

如果你还没有注意到，你在声明变量的时候使用了赋值操作符。这里有一个例子。

例如

```
one = 1
ten = 10
eleven = one + tenprint(eleven)
```

输出将是:

```
11
```

Python 还支持将=与另一个操作符结合的复合赋值操作符**。**

例如

```
number = 1
number += 2print(number)
```

输出将是:

```
3
```

# 算术运算符

算术是我们在数学中学习的第一个运算符，也称为**加、减、乘、**和**除**。在 python 中，提到的操作符还是一样的。下面是用于声明的算术运算符及其符号:

*   **加法+**
*   **减法-**
*   **乘法***
*   **师/**
*   **模数%**
*   **求幂****
*   **楼层划分//**

**注:**模数在其他编程语言中也称为余数。

声明算术运算符时，需要使用相应的符号。

例如

```
print(1 + 1) # Addition example
print(10 - 5) # Subtraction example
print(2 * 3) # Multiplication example
print(20 / 4) # Division example
print(20 % 3) # Modulus example
print(3 ** 3) # Exponentiation example
print(15 // 2) # Floor division example
```

输出将是:

```
2
5
6
5.0
2
27
7
```

# 比较运算符

该运算符比较左操作数和右操作数的值。下面是声明时的比较运算符及其对应的符号:

*   **等于==**
*   **不等于！=**
*   **大于>**
*   **小于<小于**
*   **大于等于> =**
*   **小于或等于< =**

当使用比较运算符时，结果要么为真，要么为假。

例如

```
print(1 == 1) # Equal to example
print(10 != 5) # Not equal to example
print(2 > 3) # Greater than example
print(20 < 4) # Less than example
print(20 >= 3) # Greater than or equal to example
print(30 <= 30) # Less then or equal to example
```

输出将是:

```
True
True
False
False
True
True
```

# 逻辑运算符

用于连接在程序中创建流的两个或多个逻辑表达式。逻辑运算符就是使用 AND、OR 和 NOT，还涉及布尔值 True 和 False。以下是它们所代表的符号:

*   **逻辑与& &**
*   **逻辑或||**
*   **逻辑不！**

例如

```
print(True and False) # AND example
print(False or True) # OR example
print(not True) # NOT example
```

输出将是:

```
False
True
False
```

# 还有其他运营商

如果您感到好奇，python 还支持特殊运算符，这不是本课的一部分。

*   **身份**
*   **会员资格**
*   **按位**

在我们的下一篇文章中，我们将通过处理[条件句](https://arc-sosangyo.medium.com/python-programming-tutorial-conditional-statement-6931855b917d)进行更深入的探讨。

愿法典与你同在，

*   弧

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*