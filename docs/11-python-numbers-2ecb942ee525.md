# Python 数字:Python 完整教程—第 11 部分

> 原文：<https://blog.devgenius.io/11-python-numbers-2ecb942ee525?source=collection_archive---------13----------------------->

![](img/3e26daca161ac23abac65ad002a6f34f.png)

[https://www . pexels . com/photo/calculator-and-pen-on-table-209224/](https://www.pexels.com/photo/calculator-and-pen-on-table-209224/)

**在我们开始之前，让我告诉你:**

*   这篇文章是 Python 完全初学者到专家课程
    的一部分，你可以在这里[找到它](https://medium.com/@samersallam92/python-complete-beginner-to-expert-course-f7626916df30)。
*   所有资源都可以在下面的“资源”部分找到。
*   这篇文章在 YouTube 上也有视频[点击这里](https://www.youtube.com/watch?v=7_0Vt4QkTOE)。

[https://www.youtube.com/watch?v=7_0Vt4QkTOE](https://www.youtube.com/watch?v=7_0Vt4QkTOE)

## 介绍

在本文中，您将了解 Python 中第一个可用的数据结构 numbers。

**本文将涵盖以下要点:**

1.  [什么是数字？](#760b)
2.  [数字类型](#6ae5)
3.  [如何创建号码？](#0473)
4.  [一些有用的数字函数](#b0b8)

## **1。什么是数字？**

number 是一个**单项式**数据结构，只用来存储一个项，这个项应该是 Python 支持的任何类型的数值。接下来，您将详细了解这些类型。

![](img/3ed24a2c0757ccd1a99b290787f52a5f.png)

照片由[艾库特·埃克](https://unsplash.com/@aykuteke?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/numbers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## **2。数字类型**

Python 支持四种不同的数字数据类型:

*   **布尔:**
    它只能取两个值**:真**或**假。**
    当你定义一个**布尔**变量的时候，你使用了**真**关键字或者**假**关键字，但是你要知道在幕后这个值是**一**或者**零**其中**一**指的是**真**零**指的是**假**。**
*   **Integer:**
    它通常只被称为 **int** ，用来存储**正数或负数**数字**，不含**小数点，如 3，-4 等等。
*   **Float:**
    Float 数字代表实数，用一个小数点来表示**除以**整数和小数**部分，Float 可以存储**正值或负值**。**
*   **复数:**
    通常，一个复数分为两部分，**实数和虚数。这个数字的每一部分都可以是整数或浮点数。**例如，你可以用这个形式 **a + b** 写出**复数**，其中 **a** 和 **b** 可以是**整数**或**浮点数**。复数通常用于复数代数中，而不是软件工程中。因此，在以后的文章中你不会看到复数。

![](img/b77221102a262a605951947cfb181596.png)

照片由[蒂莫·沃尔茨](https://unsplash.com/@magict1911?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/complex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## **3。** **如何创建号码**

你必须定义一个变量并给这个变量赋值。

*   **创建一个布尔数**

```
number = True
print(number)
print(type(number))
```

**输出**:

```
True
<class ‘bool’>
```

从打印出来的内容可以理解，变量值为 True，其数据类型为 boolean。

*   **创建一个整数**

```
number = 4
print(number)
print(type(number))
```

**输出**:

```
4
<class ‘int’>
```

*   **创建一个浮点数**

```
number = 4.5
print(number)
print(type(number))
```

**输出**:

```
4.5
<class ‘float’>
```

*   **创建一个复数:**之前已经提到过，一个复数分为两部分，**实数**和**虚数。**要在 Python 中定义一个复数，应该使用 **complex** 关键字，并传递实值和虚值。

**例如**:

```
number = complex(10, 4)
print(number)
print(type(number))
```

**输出**:

```
(10+4j)
<class ‘complex’>
```

很好，到目前为止，你已经学会了如何创建不同类型的数字。

现在，让我们了解一些在 Python 中与数字一起使用的预定义函数。

## **4。** **一些有用的数字函数**

*   **min():** 用于计算一个数的集合之间的**最小**数。

**语法**

```
min(arg1: int or float, arg2: int or float, *args: collection of int or float) -> int or floatReturns the minimum of all the arguments.
```

**例如**:

```
a = min(1, 2, 3, 4, -5)
print(a)
```

**输出**:

```
-5
```

*   **max():** 用于获取一个数字集合之间的**最大值**数。

**语法**

```
max(arg1: int or float, arg2: int or float, *args: collection of int or float) -> int or floatReturns the maximum of all the arguments.
```

**例如**:

```
a = max(1, 2, 3, 4, 0, 1.5, -4)
print(a)
```

**输出**:

```
4 
```

*   **round():** 返回传递的数字的舍入值。

**语法**

```
round(number: float, digits=None) -> int
```

**例如**:

```
a = round(10.7)
print(a)
```

**输出**:

```
11
```

仅供参考，Python 中还有很多其他函数可以用来处理数字，您可以根据自己的代码需求在未来学习它们。

## 现在，让我们总结一下我们在这篇文章中学到的内容:

![](img/31163a52160e5f9c6653170385493d76.png)

照片由[安 H](https://www.pexels.com/@ann-h-45017/) 在[像素](https://www.pexels.com/)上拍摄

*   **数字**:是一个**单项**数据结构，用来存储数值。
*   **数字类型:**布尔、整数、浮点、复数。
*   Python 中有一些预定义的函数来处理像 min()、max()、round()等数字。

***附:*** *:非常感谢您花时间阅读我的故事。在你离开之前让我快速提两点:*

*   *首先，要想直接在你的收件箱里看到我的帖子，请在这里订阅*[](https://medium.com/@samersallam92/subscribe)**，你可以在这里关注我*[](https://medium.com/@samersallam92)**。***
*   ***第二，作家在媒介上制造了成千上万的****$****。要获得无限制的媒体故事并开始赚钱，* [***现在就注册成为媒体会员***](https://medium.com/@samersallam92/membership)**，其中* *每月只需花费 5 美元。通过此链接* *报名* [***，可以直接支持我，不需要你额外付费。***](https://medium.com/@samersallam92/membership)***

**![Samer Sallam](img/7d756fa3da76843e747e5ecde71b84d0.png)

萨梅尔·萨拉姆** 

## **Python 初学者到专家的完整课程**

**[View list](https://medium.com/@samersallam92/list/python-complete-beginner-to-expert-course-32d3a941c05e?source=post_page-----2ecb942ee525--------------------------------)****21 stories****![A white desk with a laptop, its screen shows some lines of code, phone, cup of coffee, a screen and a white lamp on it.](img/a990b710cc6c33085894453e6e0b10b9.png)****![A dummy image for better reading and navigation.](img/6423d57aa5d56c05169d2738a1592214.png)****![A dummy image for better reading and navigation.](img/f8ba979ab77abc24ff23c97dc0499b45.png)**

**要回到上一篇文章，您可以使用以下链接:**

**[第十部分:变量、数据结构和运算符简介](/10-introduction-to-variables-data-structures-and-operators-in-python-1d434a401f71)**

**要阅读下一篇文章，您可以使用以下链接:**

**[第十二部分:算术运算符](/12-arithmetic-operators-in-python-711990e3ae47)**

## ****资源:****

*   **GitHub [**此处。**](https://github.com/samersallam/python-complete-beginner-to-expert-course/tree/main/Python%20Numbers)**