# 初始化和构造特殊方法:Python OOP 完整教程—第 13 部分

> 原文：<https://blog.devgenius.io/initialization-and-construction-special-methods-python-oop-complete-course-part-13-78b62305101?source=collection_archive---------6----------------------->

![](img/cd2ab44ff77fb5d6af21cf4abc381399.png)

[代工厂](https://pixabay.com/users/foundry-923783/)在 [Pixabay](https://pixabay.com/images/search/%20construction/) 拍摄的照片

在我们开始之前，让我告诉你

*   这篇文章是 Python 面向对象编程完整课程的一部分，你可以在这里找到它。
*   所有资源都可以在下面的“资源”部分找到。
*   这篇文章在 YouTube 上也有视频[点击这里](https://youtu.be/syX5BiOwriY)。

[https://youtu.be/syX5BiOwriY](https://youtu.be/syX5BiOwriY)

## 介绍

假设你有一个`DummyClass`类，你想从这个类定义一个对象。你要做的是:

`obj = DummyClass()`

你知道场景之外发生了什么来创造这个物体吗？通常调用一组特殊的方法来创建这个对象。要了解更多，请继续阅读。

**目录**

1.  [什么是初始化和构造特殊方法？](#8233)
2.  [可用初始化和构造特殊方法](#927c)
3.  [如何重写初始化和构造特殊方法？](#feda)

## 1.什么是初始化和构造特殊方法？

**初始化和构造特殊方法**是当你从你的类中实例化一个新对象或者当你想要删除那个对象时被调用的方法。

它们的意思非常简单，这就是你所要知道的关于它们的定义。

## 2.可用的初始化和构造特殊方法

在 Python 中，有三种方法属于这种类型。为了理解这些方法，你必须想象在 Python 中创建一个对象是一个分两个阶段的过程，如下所示:

*   **在第一阶段，**`__new__(cls, …)`方法被调用，一个新的对象将被创建，这个对象将没有任何属性。换句话说，`__new__()`负责创建 raw 对象。
*   **在第二阶段，**调用`**_**_init__(self, …)`方法。一旦创建了一个对象，这个对象将被传递给`__init__()` 方法，这个方法被称为**构造器方法**。该方法负责将实例属性附加到对象上，并将初始值赋给这些实例属性。

简单地说，`__new__()`方法将创建并返回对象，而`__init__()`只是给实例属性赋予初始值。因此，它不返回任何内容。

前两种方法用于创建对象。对于删除对象，`__del__()` 方法被调用。这个方法被称为**析构函数方法，**，当一个对象被删除并且没有返回任何东西时，这个方法就会被调用。在这个方法中，您定义了当一个对象被删除时您想要做的行为。

通常，使用`del`命令手动删除一个对象，或者在当前进程结束时由垃圾收集器自动删除。

**注意** : `__del__()`当你的对象与外部资源如数据库进行通信时，这个方法很有用。在这种情况下，当您想要删除该对象时，您必须在删除该对象之前清除所有资源/终止所有连接。

> 大多数情况下，你不会使用一个 **__new__()** 方法，你会保留它而不覆盖它。

## 3.如何重写初始化和构造特殊方法？

*   **覆盖** `init`**和** `new`**方法。**

回到上一课已经定义的`Student class`，让我们覆盖这个类的`__init__()`方法和`__new__()` 方法。然后从`Student class`中定义一个对象。

输出:

```
Hi, I am NEW
Hi, I am from the type :  <class '__main__.Student'>
Hi I am init
```

您可以从这个输出中了解到在构造函数方法之前已经调用了`new`方法。该方法创建了一个 raw 对象，该对象被传递给附加了实例属性并初始化它们的`init`方法。

另外，您可以注意到,`new`方法可以访问您想要从中创建对象的类。所以写`print('Hi, I am from the type : ', cls)`就可以简单知道这个类的类型了

*   **覆盖 del 方法**

假设您有一个用一些实例属性和方法包装文件的`FileResource`类。如果您想删除从该类创建的任何对象，您必须首先关闭该文件。为了实现这一点，您必须覆盖`__del__`方法。

现在，让我们通过定义一个对象，向文件中写入一些东西，最后使用`del`关键字删除这个对象来尝试这个类。

```
fil = FileResource('dummy')
fil.write()
del fil
```

输出:

```
File has been deleted
```

您可以看到，当执行了`del`命令后，调用了`__del__`方法。因此，该文件在删除对象之前已经关闭。

**酷吧？**

现在，让我们总结一下你在这篇文章中学到了什么。

![](img/dfe3babda390356c9538b69c6f042deb.png)

照片由[安 H](https://www.pexels.com/@ann-h-45017/) 在[像素](https://www.pexels.com/)上拍摄

在本文中，您已经了解到:

*   **初始化和构造特殊方法**是当你从你的类中实例化一个新对象或者当你删除那个对象时被调用的方法。
*   无论何时定义一个新对象，都会调用`__new__`来创建一个新的原始对象。
*   在`__new__`之后调用`__init__`来添加和初始化实例属性。
*   `__del__`在手动或自动删除对象时调用。

***附言*** *:万分感谢您花时间阅读我的故事。在你离开之前，让我快速地提两点*

*   *首先，要想直接在你的收件箱里看到我的帖子，请在这里订阅*[](https://medium.com/@samersallam92/subscribe)**，你可以在这里关注我*[](https://medium.com/@samersallam92)**。***
*   ***第二，作家在媒介上制造了数以千计的****$****。为了无限制地访问媒体故事并开始赚钱，* [***现在就注册成为媒体会员***](https://medium.com/@samersallam92/membership)**其中* *每月只需花费 5 美元。通过此链接* *报名* [***，可以直接支持我，不需要你额外付费。***](https://medium.com/@samersallam92/membership)***

**![Samer Sallam](img/7d756fa3da76843e747e5ecde71b84d0.png)

萨梅尔·萨拉姆** 

## **Python 面向对象编程的完整教程**

**[View list](https://medium.com/@samersallam92/list/the-complete-course-in-objectoriented-programming-in-python-7b54126a7f4e?source=post_page-----78b62305101--------------------------------)****24 stories****![A magnifying glass with the Python logo and a set of objects and arrows connecting between them to indicate that everything in Python is an object](img/ce97e46734c67f4f565df3877a88bd11.png)****![A dummy image for better reading and navigation.](img/926249cc12e1f2c807e7711382599fb6.png)****![A dummy image for better reading and navigation.](img/2d5e5c2a38605b91a621b6b26d9ab546.png)**

**要回到上一篇文章，您可以使用以下链接:**

**[第 12 部分:特殊方法介绍](/introduction-to-special-methods-python-oop-complete-course-part-12-e5f8d5ebd79b)**

**要阅读下一篇文章，您可以使用以下链接:**

**[第 14 部分:操作员特殊方法](https://levelup.gitconnected.com/operators-special-methods-python-oop-complete-course-part-14-b5acd58f1fdf)**

## **资源:**

*   **GitHub [这里的**这里的**](https://github.com/samersallam/The-Complete-Course-in-Object-Oriented-Programming-in-Python/tree/main/Initialization%20and%20Construction%20Special%20Methods)**。****