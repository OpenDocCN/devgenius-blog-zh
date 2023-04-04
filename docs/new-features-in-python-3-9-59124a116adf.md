# Python 3.9 中的新功能

> 原文：<https://blog.devgenius.io/new-features-in-python-3-9-59124a116adf?source=collection_archive---------1----------------------->

Python3.9 已经发布，并带来了一些很酷的特性，您现在应该尝试一下。

![](img/00edf5a33205c4cdf0615246bcff032c.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

Python 是一种编程语言，因为它易于读写，所以越来越受欢迎；由于这一点，每个新版本都在寻找方法来改善如何执行某些操作的复杂性，以使我们的生活变得更容易一些。

10 月 5 日发布了 Python 3.9 的稳定版本，它带来了不同的特性，其中一些我将在下面介绍。也邀请大家安装一下，自己试试。

## 装置

这个安装是在 Linux 上执行的，要安装在另一个操作系统上请查看 [Python 页面](https://www.python.org/downloads/release/python-390/)。

## 字典中的联合运算符

字典是 Python 中广泛使用的数据结构，我们可以用不同的方式合并或更新它们；然而，这些选项可能有点混乱:

我们有更多的方法来执行这些操作；然而，这是我认为最常见的，第一个，因为 ***update*** 方法修改了字典，我们应该用 ***copy()*** 方法创建一个副本。

另一方面，我认为合并两个或更多字典的最佳方式(到目前为止)要求我们了解*字典解包*如何生成新字典。

根据 [PEP 584](https://www.python.org/dev/peps/pep-0584/) ，我们可以找到两个操作符，我们可以以更干净、更好的方式使用它们:

这些操作符的工作方式与前面的示例相同，更新字典(|=)，并将两个或更多字典合并成一个新字典(|)。

## 新的字符串方法

Python3.9 包括两个新的 strings 方法，允许您删除前缀或后缀。在查看如何使用这些新方法之前，让我们看看在以前的版本中如何使用这些新方法。

这些操作非常简单；然而，它们并没有正确地消除我们所需要的，因为它从来没有被验证过是否有前缀或后缀。

***。移除前缀*T21 和**。removesuffix** 方法不仅负责删除字符串开头或结尾的信息，而且在这样做之前，它会根据需要删除的内容检查它是否包含前缀或后缀。**

完整描述见 [PEP 616](https://www.python.org/dev/peps/pep-0616/)

## 标准集合中的类型提示泛型

因为 Python 是动态类型化的，所以有时很难识别我们在代码中使用的是什么类型的数据，这就是我们使用 ***类型提示*** 的地方，它帮助我们理解我们的代码应该如何工作，重要的是要强调，在给出不匹配的数据类型的情况下，类型提示不会引发异常。

如你所见，一些类型注释必须以大写形式从**输入**导入，例如(List 或 Dict)。

现在，您可以使用内置类型，如 *list* 或 *dict* ，而无需导入任何注释。

详见 [PEP 585](https://www.python.org/dev/peps/pep-0585/) 。

## 新模块 zoneinfo

datetime 模块允许我们以不同的方式处理日期和时间；然而，当需要使用时区时，我们必须使用第三方库来利用它们。 [PEP 615](https://www.python.org/dev/peps/pep-0615/) 详细介绍了包含这一新模块的动机和提议，该模块允许我们根据 [IANA 数据库](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)定义时区。

此外，该模块还提供了一种显示 IANA 数据库中每个时区的方法。

## 对现有模块的改进

标准 python 库中的不同模块有一些小的改进，如:

*   新的*状态代码*被添加到 **http** 模块中
*   在**数学**模块中，增加了三个新功能，并改进了一个功能。

## 结论

也许你从来都不需要这些功能；然而，这是一个简单的介绍，我认为它可能是日常生活中最常用的，并在某种程度上允许我们的代码越来越 pythonic 化。

如果你想知道这个新版本还能提供什么，看看下面的链接。

[](https://www.python.org/downloads/release/python-390/) [## Python 版本 Python 3.9.0

### 发布日期:2020 年 10 月 5 日 Python 3.9.0 是 Python 编程语言的最新主要版本，它包含…

www.python.org](https://www.python.org/downloads/release/python-390/)