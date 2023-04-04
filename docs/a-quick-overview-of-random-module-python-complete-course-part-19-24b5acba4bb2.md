# 随机模块快速概述:Python 完整教程—第 19 部分

> 原文：<https://blog.devgenius.io/a-quick-overview-of-random-module-python-complete-course-part-19-24b5acba4bb2?source=collection_archive---------11----------------------->

## 了解为什么使用以及如何使用 Python 中的 random 模块。

![](img/4fcdc07d0e0cc52cbaf90917932fdd44.png)

Andrew Ridley 在 [Unsplash](https://unsplash.com/s/photos/random) 上拍摄的照片

在我们开始之前，让我告诉你

*   这篇文章是 Python 完全初学者到专家课程
    的一部分，你可以在这里[找到它](https://medium.com/@samersallam92/python-complete-beginner-to-expert-course-f7626916df30)。
*   所有资源都可以在下面的“资源”部分找到。
*   这篇文章在 YouTube 上也有视频[点击](https://youtu.be/7pG192naSIY)。

【https://youtu.be/7pG192naSIY】

## 介绍

假设你写了一个代码，你想用任意(随机)值测试它。您可以使用`random` 模块来代替自己定义这些值。在本文中，您将学习如何做到这一点。

**目录**

1.  [随机模块](#784c)
2.  [一些有用的随机模块函数](#59d7)

## 1.随机模块

它是一个内置模块，这意味着它附带了原始的 Python 包，如果你想使用它，你不必安装它。这个模块充满了处理随机数的有用函数。因此，如果你的程序需要任何随机能力，很可能`random`模块就是你所需要的。

这个模块有很多函数可以灵活地生成随机数，(比如`random()`、`randint()`、…)。

让我们看一些代码示例。

## 2.一些有用的随机模块函数

首先，因为`random`是一个`module`，所以你要导入。

```
import random
```

第一个函数是`randint()`，它接受两个整数参数`a`和`b`，并在[a，b]范围内生成一个随机整数。例如:

```
import random
r1 = random.randint(3, 60)
print(r1)
```

输出:

```
57
```

如果你再跑一次，你会得到另一个数字

```
60
```

每次运行这段代码，你都会得到一个不同的值，因为这是它的工作。它产生随机值。

有时我们需要生成浮点数。在这种情况下，您可以使用`random()`函数，它不接受任何参数，默认返回一个在范围`[0–1]`内的随机浮点数。

```
import random
r2 = random.random()
print(r2)
```

输出:

```
0.919913724184831
```

现在的问题是:如何在用户定义的范围内生成浮点随机值？

答案是使用`uniform()` 函数，该函数接受两个参数`a`和`b`，并在范围【a，b】内生成一个随机浮点数。

```
import random
r3 = random.uniform(1, 3)
print(r3)
```

输出:

```
2.9476736301365563
```

您在给定的范围内获得了一个随机的 float 值，并且每次运行这段代码时，您都会在相同的范围内获得一个新的随机`float` 值。

> 请记住，`random`模块充满了给你巨大随机能力的函数。

本文中您将了解的最后一个想法是种子想法。

如果你想在每次从头开始运行程序时生成相同的随机数，换句话说，如果你想在每次运行代码时再现相同的随机值，你必须使用`seed()`函数。

`seed()`接受一个整数参数，不返回任何内容。该参数用于初始化随机数发生器。

```
import random
random.seed(10)
r4 = random.random()
r5 = random.randint(1, 5)
print(r4)
print(r5)
```

输出:

```
0.5714025946899135
4
```

如果您重新运行前面的代码，您将总是得到`**0.57**` 和`**4**`，因为在这种情况下已经使用了`seed()`函数。因此，每次运行这段代码时，随机数生成器将从同一个种子开始。

酷吧？

现在，让我们总结一下我们在这篇文章中学到了什么。

![](img/31163a52160e5f9c6653170385493d76.png)

照片由[安 H](https://www.pexels.com/@ann-h-45017/) 在[像素](https://www.pexels.com/)上拍摄

*   `random` module 是一个内置的模块，其中充满了处理随机数的有用函数。

***附言*** *:万分感谢您花时间阅读我的故事。在你离开之前，让我快速地提两点*

*   *首先，要想直接在你的收件箱里看到我的帖子，请在这里订阅*[](https://medium.com/@samersallam92/subscribe)**，你可以在这里关注我*[](https://medium.com/@samersallam92)**。***
*   ***第二，作家在媒介上制造了数以千计的****$****。为了无限制地访问媒体故事并开始赚钱，* [***现在就注册成为媒体会员***](https://medium.com/@samersallam92/membership)**其中* *每月只需花费 5 美元。通过此链接* *报名* [***，可以直接支持我，不需要你额外付费。***](https://medium.com/@samersallam92/membership)***

**![Samer Sallam](img/7d756fa3da76843e747e5ecde71b84d0.png)

萨梅尔·萨拉姆** 

## **Python 初学者到专家的完整课程**

**[View list](https://medium.com/@samersallam92/list/python-complete-beginner-to-expert-course-32d3a941c05e?source=post_page-----24b5acba4bb2--------------------------------)****21 stories****![A white desk with a laptop, its screen shows some lines of code, phone, cup of coffee, a screen and a white lamp on it.](img/a990b710cc6c33085894453e6e0b10b9.png)****![A dummy image for better reading and navigation.](img/6423d57aa5d56c05169d2738a1592214.png)****![A dummy image for better reading and navigation.](img/f8ba979ab77abc24ff23c97dc0499b45.png)**

**要回到上一篇文章，您可以使用以下链接:**

**第 18 部分:数学模块**

**要阅读下一篇文章，您可以使用以下链接:**

**(下一篇文章发表后，该链接将可用)**

## **资源:**

*   **GitHub [**此处**](https://github.com/samersallam/python-complete-beginner-to-expert-course/tree/main/random%20Module) **。****