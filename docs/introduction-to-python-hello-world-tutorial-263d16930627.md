# Python 编程介绍:Hello World 教程

> 原文：<https://blog.devgenius.io/introduction-to-python-hello-world-tutorial-263d16930627?source=collection_archive---------4----------------------->

## 推荐初学者使用的编程语言

![](img/449380d69a2e351cca57e0af11304c30.png)

摄影:unsplash.com 的大卫·克劳德

Python 是我熟悉的第一种编程语言，这要归功于它的可读性。对于初学者来说，开始学习如何编码是很容易理解的。但这并不意味着 python 不被专家使用。作为一种通用编程语言，它足够强大，专业人士将其用于商业应用、web 开发、自动化、数据科学和机器学习。

就个人而言，每当我需要为我的 unix/linux sysadmin 任务创建复杂的自动化时，我更喜欢使用 python。我的理由是因为 python 有广泛的可用模块，让我可以轻松地自动高效地完成工作。和我一样，你也有你想学 python 的理由。我很高兴这一系列教程将帮助您学习如何使用 python 编码。

> 这篇文章是我的 [Python 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad)系列的一部分。

但在此之前，让我们设置您的机器。

# 如何获得 Python？

如果您想快速学习 python 并在以后设置它。网上有大量的模拟器，在那里你可以使用方便的网络浏览器来练习 python。

本教程假设您已经熟悉终端或命令提示符环境。如果没有，我也建议您首先使用基于浏览器的模拟器，因为熟悉终端环境需要一些时间，并且需要单独的教程。

默认情况下，大多数基于 unix 或 linux 的操作系统，如 **Ubuntu** 和 **macOS** 已经预装了 python。您可以通过在终端中执行以下命令来检查是否安装了 python 以及版本:

```
python --version 
```

**Windows** 操作系统没有预装 python。可以从其[官网](https://www.python.org/)下载。windows 版本有关于如何设置 python 的简单逐步说明。

如果您是编程新手，并且将在您的机器上使用安装的 python，我建议您选择一个好的文本编辑器进行编码。我个人推荐要么是 **VS 代码**、 **Atom.io** 要么是 **Notepad ++** 。

# 像往常一样“你好，世界”

一个编程教程，没有“Hello World”就永远不一样。这一部分将教你如何在你的机器上运行 python 代码。

在文本编辑器或模拟器中键入以下代码。

```
print(“Hello World”)
```

如果您使用的是基于浏览器的模拟器，只需在那里运行它，它就会在输出中显示 Hello World。

如果你正在使用你的机器，用**保存文件。py** 分机。对于本教程，将文件命名为 HelloWorld.py。同样，我假设您熟悉如何使用终端/命令提示符环境。使用**终端**或 **powershell** 导航到您保存文件的位置，然后执行以下命令:

```
python HelloWorld.py
```

这将运行 python 程序并显示输出。

```
Hello World
```

# 如何处理权限被拒绝错误？

对于大多数 Linux 或 macOS 用户，您可能会遇到权限被拒绝的错误。这是因为您需要在保存文件后首先允许它的权限。

导航文件的保存位置。然后通过运行 **chmod 750 file.swift** 将文件权限改为可执行。

例如

```
chmod 750 HelloWorld.py
```

然后再次执行该文件。

运行您的第一个 python 代码做得很好。在下一篇文章中，我们将处理 python 的变量。

愿法典与你同在，

-电弧