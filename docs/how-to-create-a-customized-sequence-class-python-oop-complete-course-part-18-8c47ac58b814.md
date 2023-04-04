# 如何创建定制的序列类:Python OOP 完整教程—第 18 部分

> 原文：<https://blog.devgenius.io/how-to-create-a-customized-sequence-class-python-oop-complete-course-part-18-8c47ac58b814?source=collection_archive---------3----------------------->

## 了解 Python OOP 中的特殊方法序列以及如何覆盖它们。

![](img/6b3457c232e3400ffd6ec7c4bee8ccab.png)

Artur Voznenko 在 [Unsplash](https://unsplash.com/s/photos/sequences) 上拍摄的照片

在我们开始之前，让我告诉你:

*   这篇文章是 Python 面向对象编程完整课程的一部分，你可以在这里找到它。
*   所有资源都可以在下面的“资源”部分找到。

## 介绍

你有没有尝试过定义一个类似于 Python 中常见序列的类，比如列表和字典，这些类的对象可能支持索引、切片或使用`for` 循环的迭代？

如果你想学习如何做到这一点，你来对地方了。

**目录**

1.  [什么是序列特殊方法？](#5028)
2.  [可用序列特殊方法](#b0cd)
3.  [如何覆盖序列特殊方法？](#625e)

## 1.什么是序列特殊方法？

它们是让你的 Python 类表现得像**内置**序列(`dict`、`tuple`、`list`、`str`等)的方法..).

这些数据类型支持索引、迭代、成员运算符(in)和其他功能。为了让你的对象支持这些特性，你必须覆盖这些**序列特殊方法**。

## 2.可用序列特殊方法

让我们探索其中的一些功能。

*   `__len__()`:是让你的类对象支持`len()`函数的方法。
*   `__getitem__()`:是让你的类对象支持索引的方法。
*   `__setitem__()`:这是一个让你的类对象可变的方法。

为了保持文章简洁明了，所有其他方法的定义都被移到了这个 pdf 文件中。

![](img/de7dd2a152be289eedd29af0359ea1e5.png)

由[塔索·米萨拉基斯](https://unsplash.com/@tassomitsarakis)在 [Unsplash](https://unsplash.com/s/photos/available-lights) 上拍摄的照片

## 3.如何重写序列特殊方法？

首先假设您有一个包含多个文本文件的数据集，您需要一个类来帮助您方便快捷地访问这些文件(set 和 get)。

让我们从定义一个简单的类`Dataset`开始，它有两个实例属性`dataset_path`和`samples`。

```
import osclass Dataset:
    def __init__(self, dataset_path):
        self.dataset_path = dataset_path
        self.samples = os.listdir(dataset_path)dataset = Dataset(r'E:\new deeep team\python-oop\Sections\[3] Special Methods (Dunder Methods) in Python OOP\[8] Sequences Special Methods\Article\dataset')
print(dataset.samples)
```

`os.listdir()`已用于获取所选位置的可用文件和文件夹。

在这个简单的例子中，如果你打印`dataset.samples`，你将得到`['Doc 0.txt', 'Doc 1.txt', 'Doc 2.txt']`。换句话说，我们的数据集有三个文本文件。

现在，让我们检查数据集中有多少文件。

```
# Use len method
print(len(dataset))
```

输出:

```
**TypeError**                                 Traceback (most recent call last)
**<ipython-input-6-78696a0140a2>** in <module>
      1 **# Use len method**
**----> 2** print**(**len**(**dataset**))**

**TypeError**: object of type 'Dataset' has no len()
```

因为对象不支持方法`len()`，所以得到了`TypeError`。

现在让我们通过覆盖方法`__len__()`使`Dataset`对象支持该方法。(提醒一下，`samples`属性有一个数据集文件的列表)

输出:

```
3
```

在这种情况下，不再有错误，您得到了 **3** ，因为它是文本文件的数量。

现在，让我们尝试访问这个数据集中的第一个文件。

```
print(dataset[0])
```

输出:

```
**TypeError**                                 Traceback (most recent call last)
**<ipython-input-10-458771d72ad1>** in <module>
**----> 1** print**(**dataset**[0])**

**TypeError**: 'Dataset' object does not support indexing
```

您得到了`TypeError`,因为`Dataset`对象不支持索引。为了支持索引，您应该覆盖方法`__getitem__()`。

现在让我们通过执行以下步骤来覆盖`__getitem__()`方法:

*   所有样品都有这种图案的名称`Doc {number}.txt`。因此，它将作为类属性添加。
*   下一个`__getitem__()`方法将被覆盖。该方法接受对象本身`self`和另一个参数来表示索引`index`，并返回所选择的值。

输出:

```
Hi, I am Doc 2
```

您获得了数据集中第三个文件的内容。

覆盖`__getitem__()`方法的另一个好处是类对象变得可迭代。下一个例子展示了如何在`for` 循环中使用类对象。

```
for txt_file in dataset:
    print(txt_file)
```

输出:

```
Hi, I am Doc 0
Hi, I am Doc 1
Hi, I am Doc 2
```

现在，让我们看看如何使用我们的类来更新文件的内容。

首先，让我们尝试下面的代码。

```
dataset[2] = "Hello, new value!!"
```

输出:

```
**TypeError**                                 Traceback (most recent call last)
**<ipython-input-34-dcffeaa6152f>** in <module>
**----> 1** dataset**[2]** **=** **"Hello, new value!!"**

**TypeError**: 'Dataset' object does not support item assignment
```

您得到了一个`TypeError`,因为`Dataset`对象不支持项目分配。换句话说，`Dataset`类是**不可变的**。

要将其转换成一个**可变的**类，您应该覆盖方法`__setitem__()`。让我们在下面的代码片段中实现这一点。参考最后一个方法，看到我们打印了更新前后的值。

输出:

```
Before update
Hi, I am Doc 2

Updating a file

Afeter update
Hi I am update Doc 2
```

您现在可以看到，第三个文件的内容已经更新。

在前面的例子中，您可以看到`__set__()`已经被覆盖，如果它不存在，允许创建一个新项目。让我们试试。

```
print(dataset[4])
```

输出:

```
**FileNotFoundError**                         Traceback (most recent call last)
**<ipython-input-37-50f447d6a573>** in <module>
**----> 1** print**(**dataset**[4])**

**<ipython-input-35-6d0cd1f29eb1>** in __getitem__**(self, index)**
     13         file_name **=** self**.**sample_name**.**format**(**index**)**
     14         file_path **=** os**.**path**.**join**(**self**.**dataset_path**,** file_name**)**
**---> 15         with** open**(**file_path**,** **'r')** **as** f**:**
     16             **return** f**.**read**()**
     17 

**FileNotFoundError**: [Errno 2] No such file or directory: 'E:\\new deeep team\\python-oop\\Sections\\[3] Special Methods (Dunder Methods) in Python OOP\\[8] Sequences Special Methods\\Article\\dataset\\Doc 4.txt'
```

你得到了`FileNotFoundError`，因为没有一个文件叫做`Doc 4.txt`。

现在，让我们创建文档`Doc 4.txt`

```
dataset[4] = "Hi, I am Doc 4"

print(dataset[4])
```

输出:

```
Creating a new file
Hi, I am Doc 4
```

如您所见，文件`Doc 4`已经创建，您已经获得了它的内容。

现在，让我们总结一下我们在这篇文章中学到了什么。

![](img/ccb7fc677cd37fec2b5d19924bb08e77.png)

照片由[安 H](https://www.pexels.com/@ann-h-45017/) 在[像素](https://www.pexels.com/)上拍摄

*   **序列特殊方法**是让你的 Python 类像内置序列一样工作的方法(`dict`、`tuple`、`list`、`str`等等)..).
*   使用这样的方法将使你的类对象支持像**索引、循环迭代、成员操作符……等等**这样的特性。

***附言*** *:万分感谢您花时间阅读我的故事。在你离开之前，让我快速地提两点*

*   *首先，要想直接在你的收件箱里看到我的帖子，请在这里订阅*[](https://medium.com/@samersallam92/subscribe)**，你可以在这里关注我*[](https://medium.com/@samersallam92)**。***
*   ***第二，作家在媒介上制造了数以千计的****$****。为了无限制地访问媒体故事并开始赚钱，* [***现在就注册成为媒体会员***](https://medium.com/@samersallam92/membership)**其中* *每月只需花费 5 美元。通过此链接* *报名* [***，可以直接支持我，不需要你额外付费。***](https://medium.com/@samersallam92/membership)***

**![Samer Sallam](img/7d756fa3da76843e747e5ecde71b84d0.png)

萨梅尔·萨拉姆** 

## **Python 面向对象编程的完整教程**

**[View list](https://medium.com/@samersallam92/list/the-complete-course-in-objectoriented-programming-in-python-7b54126a7f4e?source=post_page-----8c47ac58b814--------------------------------)****24 stories****![A magnifying glass with the Python logo and a set of objects and arrows connecting between them to indicate that everything in Python is an object](img/ce97e46734c67f4f565df3877a88bd11.png)****![A dummy image for better reading and navigation.](img/926249cc12e1f2c807e7711382599fb6.png)****![A dummy image for better reading and navigation.](img/2d5e5c2a38605b91a621b6b26d9ab546.png)**

**要回到上一篇文章，您可以使用以下链接:**

**[第 17 部分:当你获取、设置或删除一个实例属性时，场景之外会发生什么](https://levelup.gitconnected.com/what-happens-beyond-the-scene-when-you-get-set-or-delete-an-instance-attribute-d88be5610456)**

**要阅读下一篇文章，您可以使用以下链接:**

**第 19 部分:[如何在 Python OOP 中创建自定义上下文管理器](https://medium.com/gitconnected/how-to-create-a-custom-context-manager-in-python-oop-python-oop-complete-course-part-19-6f6647a97f5c)**

## **资源:**

*   **GitHub **在这里。****
*   **序列特殊方法 [pdf 文件](https://samersallam.gumroad.com/l/bewew)。**