# 比较运算符:Python 完整教程—第 13 部分

> 原文：<https://blog.devgenius.io/python-complete-course-part13-comparison-operators-be85ac47361b?source=collection_archive---------10----------------------->

![](img/62e16815aaf62cc37eee4f37ab3ac37d.png)

照片由[耶鲁安穴獭](https://unsplash.com/@jeroendenotter)在 [Unsplash](https://unsplash.com/s/photos/comparison)

**在我们开始之前，让我告诉你:**

*   这篇文章是 Python 完全初学者到专家课程
    的一部分，你可以在这里[找到它](https://medium.com/@samersallam92/python-complete-beginner-to-expert-course-f7626916df30)。
*   所有资源都可以在下面的“资源”部分找到。
*   这篇文章也可以作为 YouTube 视频[在这里](https://youtu.be/JQYknV0k5mk)获得。

[https://youtu.be/JQYknV0k5mk](https://youtu.be/JQYknV0k5mk)

## 介绍

假设您正在构建一个为 20 到 40 岁年龄组设计的应用程序。为了实现这一点，你应该要求用户在应用程序注册表单中输入他的年龄，在你的后端代码中，你应该比较用户的年龄和你的应用程序的年龄范围。为了做到这一点，你需要比较操作符，要了解更多，请继续阅读。

**本文将涵盖以下要点:**

1.  [比较运算符](#eda3)
2.  [等于(==)](#124b)
3.  [不等于(！=)](#0837)
4.  [大于(> )](#58ce)
5.  [大于等于(> =)](#2cb5)
6.  [小于(< )](#36d2)
7.  [小于或等于(< =)](#e052)

## 1.比较运算符

从名字上你可以理解它们主要用于两个值之间的比较，和前面的操作符一样，你必须知道这些操作符接受的操作数的数量、操作数的数据类型和结果的数据类型。对于比较运算符，每个运算符:

*   接受两个操作数
*   操作数数据类型是数字
*   结果是一个数字(布尔值)

现在让我们看一些比较运算符的例子…

## 2.等于(==)

**输入:**

```
a = 10
b = 20c = a==b
print(c)
```

**输出:**

```
False
```

正如您在前面的示例中看到的，因为变量“a”的值不等于“b”的值，所以结果为“False”。

***注意:当你想给一个变量赋值时，你用“=”，但当你想比较两个变量时，你用“==”。***

## 3.不等于(！=)

**输入:**

```
c = a != b
print(c)
```

**输出:**

```
True
```

因为变量不相等，所以你得到了真值。

## 4.大于(>)

**输入:**

```
c = a > b
print(c)
```

**输出:**

```
False
```

这里你得到了 False，因为 a 的值大于 b 的值。

## 5.大于或等于(> =)

**输入:**

```
c = a >= b
print(c)
```

**输出:**

```
False
```

这里你得到假也是因为“a”不等于也不大于“b”。

## 6.小于(

**输入:**

```
c = a < b
print(c)
```

**输出:**

```
True
```

这里你得到了正确的答案，因为 a 小于 b。

## 7.小于或等于(<=)

**输入:**

```
#define a new variables 
a = 10
b = 10c = a <= b
print(c)
```

**输出:**

```
True
```

这里你得到了正确的答案，因为 a 等于 b。

## 现在，让我们总结一下我们在这篇文章中学到的内容:

![](img/31163a52160e5f9c6653170385493d76.png)

照片由[安 H](https://www.pexels.com/@ann-h-45017/) 在[像素上](https://www.pexels.com/)拍摄

*   比较运算符:用于比较两个值。
*   比较运算符只接受两个操作数，并且这些操作数应该是数字。
*   比较操作的最终结果是一个数字(布尔值)。

***附言*** *:非常感谢您花时间阅读我的故事。在你离开之前，让我快速地提两点*

*   *首先，要想直接在你的收件箱里看到我的帖子，请在这里订阅*[](https://medium.com/@samersallam92/subscribe)**，你可以在这里关注我*[](https://medium.com/@samersallam92)**。***
*   ***第二，作家在媒介上制造了几千个****$*$***。为了无限制地访问 Medium stories 并开始赚钱，* [***现在就注册成为 Medium 会员***](https://medium.com/@samersallam92/membership)**，其中* *每月只需花费 5 美元。通过报名* [***带此链接***](https://medium.com/@samersallam92/membership) *，可以直接支持我，不需要你额外付费。****

**![Samer Sallam](img/7d756fa3da76843e747e5ecde71b84d0.png)

萨梅尔·萨拉姆** 

## **Python 初学者到专家的完整课程**

**[View list](https://medium.com/@samersallam92/list/python-complete-beginner-to-expert-course-32d3a941c05e?source=post_page-----be85ac47361b--------------------------------)****21 stories****![A white desk with a laptop, its screen shows some lines of code, phone, cup of coffee, a screen and a white lamp on it.](img/a990b710cc6c33085894453e6e0b10b9.png)****![A dummy image for better reading and navigation.](img/6423d57aa5d56c05169d2738a1592214.png)****![A dummy image for better reading and navigation.](img/f8ba979ab77abc24ff23c97dc0499b45.png)**

**要回到上一篇文章，您可以使用以下链接:**

**第 12 部分:Python 中的算术运算符**

**要阅读下一篇文章，您可以使用以下链接:**

**[第 14 部分:逻辑运算符](/logical-operators-python-complete-course-part-14-fed8e4ddc415)**

## **资源:**

*   **GitHub[T5 这里](https://github.com/samersallam/python-complete-beginner-to-expert-course/tree/main/Comparison%20Operators) **。****