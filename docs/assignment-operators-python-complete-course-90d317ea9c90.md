# 赋值运算符:Python 完整教程—第 16 部分

> 原文：<https://blog.devgenius.io/assignment-operators-python-complete-course-90d317ea9c90?source=collection_archive---------15----------------------->

![](img/24831e3f2c5b032df49c301ebc9fa117.png)

照片由[福蒂斯·福托普洛斯](https://unsplash.com/@ffstop)在 [Unsplash](https://unsplash.com/s/photos/programming-langu) 拍摄

在我们开始之前，让我告诉你:

*   这篇文章是 Python 完全初学者到专家课程
    的一部分，你可以在[这里](https://medium.com/@samersallam92/python-complete-beginner-to-expert-course-f7626916df30)找到它。
*   所有资源都可以在下面的“资源”部分找到。

## 介绍

在这个讲座中，你将学习 Python 中的**赋值操作符**。

**目录**

1.  [赋值运算符](#b5af)
2.  [赋值(=)](#50fb)
3.  [加法和赋值(+=)](#e9cd)
4.  [减法和赋值(-=)](#9028)
5.  [乘法和赋值(*=)](#c7b2)
6.  [除法和赋值(/=)](#5b1a)
7.  [模数和赋值(%=)](#0bc1)
8.  [取幂和赋值(**=)](#61a4)
9.  [楼层划分和赋值(//=)](#7816)
10.  [AND and 赋值(& =)](#a1b0)
11.  [OR and 赋值(|=)](#21d0)
12.  [异或和赋值(^=)](#1dac)
13.  [右移位并赋值(> > =)](#057f)
14.  [左移和赋值(< < =)](#070d)

## 1.赋值运算符

赋值运算符用于更新变量并将值赋给变量。这些操作员(除了`=`)主要做两个工作。它以某种方式更新变量值，然后将这个值再次赋给同一个变量。

对于 Python 中的每个操作符，您必须知道该操作符接受的操作数数量、操作数数据类型和结果数据类型。

对于赋值运算符，每个运算符:

*   接受两个操作数。
*   操作数应该是数字。
*   结果是一个数字。

**现在让我们举一些真实的例子来更好地理解。**

## 2.赋值(=)

等号用于给变量赋值。

```
c = 5
print(c)
```

输出:

```
5
```

## 3.加法和赋值(+=)

代码示例:

```
c += 5 
print(c)
```

输出:

```
10
```

这个表达式`c += 5`正好等价于说`c = c + 5`，意思是说`c`的新值等于旧值`c` 加 5。`c`的旧值是 5，如果你把 5 加到 5，你会得到 10。

## 4.减法和赋值(-=)

代码示例:

```
c = 5
c -= 2 #c = c - 2
print(c)
```

输出:

```
3
```

您得到 3，因为 5–2 是 3，结果将再次被赋给变量`c`。

> **注**:本课不会解释每个操作符是如何工作的，因为你已经在前面的课中学到了，但是你应该明白的是，当你使用这些带等号的操作符 **=** 时，这意味着你将更新这个值，然后再把这个值赋给同一个变量。

## 5.乘法和赋值(*=)

代码示例:

```
c = 20
c *= 10
print(c)
```

输出:

```
200
```

## 6.除法和赋值(/=)

代码示例:

```
c = 20
c /= 10
print(c)
```

输出:

```
2.0
```

## 7.模数和赋值(%=)

代码示例:

```
c = 20
c %= 3
print(c)
```

输出:

```
2
```

## 8.取幂和赋值(**=)

代码示例:

```
c = 10
c **= 2
print(c)
```

输出:

```
100
```

## 9.楼层划分和分配(//=)

代码示例:

```
c = 20
c //= 3
print(c)
```

输出:

```
6
```

## 10.AND 和赋值(&=)

代码示例:

```
c = 5 
c &= 3
print(c)
```

输出:

```
1
```

## 11.OR and 赋值(|=)

代码示例:

```
c = 5 
c |= 3
print(c)
```

输出:

```
7
```

## 12.异或并赋值(`^=`)

代码示例:

```
c = 5 
c ^= 3
print(c)
```

输出:

```
6
```

## 13.右移位和赋值(> > =)

代码示例:

```
c = 7
c >>= 2
print(c)
```

输出:

```
1
```

## 14.左移并赋值(<<=)

Code example:

```
c = 7
c <<= 2
print(c)
```

Output:

```
28
```

## Now, let us summarize what we have learned in this article:

![](img/31163a52160e5f9c6653170385493d76.png)

Photo by [Ann H](https://www.pexels.com/@ann-h-45017/) on [pexels](https://www.pexels.com/)

*   **赋值运算符:**用于更新和赋值给变量。
*   赋值运算符只接受两个操作数，并且这些操作数应该是数字。
*   这些运算符运算的最终结果是一个数字。

***附:*** *:非常感谢您花时间阅读我的故事。在你离开之前，让我快速地提两点*

*   *首先，要直接在您的收件箱中获得我的帖子，请在此处订阅*[](https://medium.com/@samersallam92/subscribe)**，您可以在此处关注我*[](https://medium.com/@samersallam92)**。***
*   ***第二，作家在媒介上制造了数以千计的****$****。为了无限制地访问媒体故事并开始赚钱，* [***现在就注册成为媒体会员***](https://medium.com/@samersallam92/membership)**其中* *每月只需花费 5 美元。通过此链接* *报名* [***，可以直接支持我，不需要你额外付费。***](https://medium.com/@samersallam92/membership)***

**![Samer Sallam](img/7d756fa3da76843e747e5ecde71b84d0.png)

萨梅尔·萨拉姆** 

## **Python 初学者到专家的完整课程**

**[View list](https://medium.com/@samersallam92/list/python-complete-beginner-to-expert-course-32d3a941c05e?source=post_page-----90d317ea9c90--------------------------------)****21 stories****![A white desk with a laptop, its screen shows some lines of code, phone, cup of coffee, a screen and a white lamp on it.](img/a990b710cc6c33085894453e6e0b10b9.png)****![A dummy image for better reading and navigation.](img/6423d57aa5d56c05169d2738a1592214.png)****![A dummy image for better reading and navigation.](img/f8ba979ab77abc24ff23c97dc0499b45.png)**

**要回到上一篇文章，您可以使用以下链接:**

**[第 15 部分:位运算符](https://python.plainenglish.io/bitwise-operators-python-complete-course-part-15-e688a24c2f0a)**

**要阅读下一篇文章，您可以使用以下链接:**

**[第 17 部分:操作员优先级](/operators-priorities-python-complete-course-part-17-1070b43d4127)**

## **资源:**

*   **GitHub [**此处**](https://github.com/samersallam/python-complete-beginner-to-expert-course/tree/main/Assignment%20Operators) **。****