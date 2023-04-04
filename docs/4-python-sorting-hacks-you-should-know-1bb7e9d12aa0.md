# 你应该知道的关于 Python 排序的 4 个技巧

> 原文：<https://blog.devgenius.io/4-python-sorting-hacks-you-should-know-1bb7e9d12aa0?source=collection_archive---------10----------------------->

## 了解 Python 中一些有用的高级排序选项。

![](img/6642b26ec6ed21ccaacc1fc89232ef73.png)

照片由[印尼 UX](https://unsplash.com/@uxindo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/sorting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

在本文中，您了解了一些您以前可能不知道的关于排序的技巧，但是我确信它们会让您的生活变得更加轻松。不再拖延，让我们直接进入正题。

**目录**

1.  [列表、元组和字典的 Sorted()与 Sort()](#ecfc)
2.  [根据您的标准排序](#bedd)
3.  [整理复杂物体的集合](#61dc)
4.  [排序用](#3d69) `[attrgetter](#3d69)` [功能](#3d69)

## 1.列表、元组和字典的 Sorted()与 Sort()

在 Python 中，有两个主要的函数，`sorted()`和`sort()`，用于对列表、元组和字典等集合进行排序。为了理解它们之间的区别，让我们举几个例子。

*   **排序列表**

假设你有下面的**数字列表**。

```
list_1 = [24, 54, -1, 4, 0, 76]
```

如果你想得到这个列表的排序版本，你可以使用`sorted()`函数如下:

```
list_1 = [24, 54, -1, 4, 0, 76]
sorted_list = sorted (list_1)print ('The origina list is: ', list_1)
print('The sorted list is: ', sorted_list)
```

输出:

```
The origina list is:  [24, 54, -1, 4, 0, 76]
The sorted list is:  [-1, 0, 4, 24, 54, 76]
```

在前面的例子中，您可以看到原始列表没有受到影响，而新的排序列表是使用新变量`sorted_list`存储的。

如果你想对原始列表进行排序，换句话说，在不需要新变量的情况下改变原始列表，你可以使用实例方法`sort()`。参考下面的例子。

```
list_1 = [24, 54, -1, 4, 0, 76]
print ('Before Sorting:', list_1)list_1.sort()
print('After Sorting:', list_1)
```

输出:

```
Before Sorting: [24, 54, -1, 4, 0, 76]
After Sorting: [-1, 0, 4, 24, 54, 76]
```

从前面的输出中，我们可以了解到`sort()`方法和`sorted()`函数的主要区别在于:

*   `sorted()`函数返回一个新的排序列表，这样你就可以把它赋给一个新的变量。
*   方法对列表进行排序，因此它不返回任何内容。

*   **对元组进行排序**

对于 Python 中的 tuple，只能使用`sorted()`函数，因为 tuple 是不可变的数据类型。因此，`sort()`不是受支持的方法。

```
tup_1 = (24, 54, -1, 4, 0, 76)
sorted_tuple = sorted(tup_1)print ('The origina tuple is: ', tup_1)
print('The sorting result is: ', sorted_tuple)
```

输出:

```
The origina tuple is:  (24, 54, -1, 4, 0, 76)
The sorting result is:  [-1, 0, 4, 24, 54, 76]
```

*   **整理字典**

在字典的情况下，`sorted()`函数将只对字典的关键字进行排序。让我们看一个简单的例子。

```
dic = {'course': 'Python Sorting', 'duration':'5 mins', 'trainer': 'Samer Sallam', 'level': 'Advanced'}sorted_dic = sorted(dic)print ('The sorting result: ', sorted_dic)
```

输出:

```
The sorting result:  ['course', 'duration', 'level', 'trainer']
```

注意，排序返回的值是传递的字典键的排序列表。

> 无论您在排序什么，您都可以通过使用参数'`reverse = True’`'按降序排序。

## 2.根据您的标准进行排序

在前面的例子中，项目是根据它们的**实际**值排序的，但是如果您想根据另一个标准排序呢？比方说，你想根据它们的绝对值对它们进行排序。

为此，您可以使用参数`key`来传递一个代表您的标准的可调用函数。让我们看看下一个使用内置函数`abs()`的例子。

```
list_1 = [24, -54, -1, 4, 0, -76]
sorted_list = sorted (list_1, key=abs)print ('The origina list is: ', list_1)
print('The sorted list according to the absolute values is: ', sorted_list)
```

输出

```
The origina list is:  [24, -54, -1, 4, 0, -76]
The sorted list according to the absolute values is:  [0, -1, 4, 24, -54, -76]
```

现在，项目已经按照升序排序，但是是根据它们的绝对**值。**

## 3.对象排序

前面所有的例子都涵盖了项是数字的集合，但是如果项是复杂的对象呢？接下来，您将看到在这种情况下应该怎么做。

假设您有下面的`Student`类。此外，假设您有一个来自同一个类的三个对象的列表，如下所示(`__repr__`已被覆盖以很好地打印对象):

现在，让我们尝试使用`sorted()`函数对列表`students_list`进行排序

```
sorted_students = sorted(students_list)
```

输出:

```
TypeError: '<' not supported between instances of 'Student' and 'Student'
```

我们得到了一个类型错误，因为解释器不知道如何对这些对象进行排序。

为了解决这个问题，我们应该向解释器解释如何对它们进行排序，这是通过再次使用参数`key`来完成的。此参数接受定义排序标准的函数。

在下面的例子中，假设我们想要根据学生的名字对对象进行排序(参见`key_sort`函数)。

输出:

```
[(Alex, 25), (Bob, 30), (John, 26)]
```

你也可以根据物品的年代来分类。我会留着这个给你试试。

另外，如果你熟悉 Python 中的 lambda 函数，你可以用它来代替定义`key_sort`函数。参考下面的例子。

输出:

```
[(Alex, 25), (Bob, 30), (John, 26)]
```

## 4.使用`attrgetter`功能进行分类

在前面的例子中，我们已经定义了自己的函数来获取对象的属性。如果这是你需要的，你可以用内置的`attrgetter`函数来代替。从它的名字你可以理解它得到了所需属性的值。您可以从`operator`模块导入该功能。

现在，让我们看一个根据学生年龄对项目进行排序的示例。

输出:

```
[(Alex, 25), (John, 26), (Bob, 30)]
```

现在，让我们总结一下你在这篇文章中学到了什么。

![](img/515b0a19c5684fe558ac106292fbbe77.png)

照片由[安 H](https://www.pexels.com/@ann-h-45017/) 在[像素](https://www.pexels.com/)上拍摄

*   Python 中有两个主要的排序函数`sorted()`和`sort()`。第一个函数返回一个新的排序集合，而第二个函数改变原始集合。
*   你可以使用参数`reverse`进行降序排序。
*   您可以使用参数`key`来定义您自己的分类标准。
*   `attrgetter`是一个有用的内置函数，当你想使用一个对象实例属性作为一个排序键。

***附言*** *:万分感谢您花时间阅读我的故事。在你离开之前，让我快速地提两点*

*   *首先，要直接在您的收件箱中获得我的帖子，请在此订阅*[](https://medium.com/@samersallam92/subscribe)**并且您可以在此关注我*[](https://medium.com/@samersallam92)**。***
*   ***第二，作家在媒介上制造了几千个****$*$***。为了无限制地访问 Medium stories 并开始赚钱，* [***现在就注册成为 Medium 会员***](https://medium.com/@samersallam92/membership)**，其中* *每月只需花费 5 美元。通过此链接* [***报名***](https://medium.com/@samersallam92/membership) *，可以直接支持我，不需要你额外付费。****

**你可能也会喜欢**

**[](https://betterprogramming.pub/advanced-python-string-formatting-ac0e8fa88b0f) [## Python 中的高级字符串格式

### 了解 Python 中 format()函数的高级选项

better 编程. pub](https://betterprogramming.pub/advanced-python-string-formatting-ac0e8fa88b0f) ![Samer Sallam](img/7d756fa3da76843e747e5ecde71b84d0.png)

萨梅尔·萨拉姆

## Python 面向对象编程的完整教程

[View list](https://medium.com/@samersallam92/list/the-complete-course-in-objectoriented-programming-in-python-7b54126a7f4e?source=post_page-----1bb7e9d12aa0--------------------------------)24 stories![A magnifying glass with the Python logo and a set of objects and arrows connecting between them to indicate that everything in Python is an object](img/ce97e46734c67f4f565df3877a88bd11.png)![A dummy image for better reading and navigation.](img/926249cc12e1f2c807e7711382599fb6.png)![A dummy image for better reading and navigation.](img/2d5e5c2a38605b91a621b6b26d9ab546.png)**