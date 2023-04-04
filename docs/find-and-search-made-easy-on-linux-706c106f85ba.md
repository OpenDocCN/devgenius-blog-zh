# 在 Linux 上轻松查找和搜索

> 原文：<https://blog.devgenius.io/find-and-search-made-easy-on-linux-706c106f85ba?source=collection_archive---------1----------------------->

## 每个程序员都应该知道的终端命令

![](img/a6468dd4d3c6be96d79e95291ba72b27.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com/photos/afW1hht0NSs) 上拍摄的照片

终端是一个重要的工具，它使高级用户能够有效地导航文件系统。通常文件系统是如此的庞大和复杂，以至于使用图形文件浏览器进行搜索既慢又低效。如果你是一个总是开着终端的程序员，为什么不用它来运行搜索而不是打开文件浏览器呢？

两种最常见的与搜索相关的查询包括按名称搜索文件名或文件夹，以及搜索包含关键字的文件。在本文中，我们将回顾实现这一行为的命令。使用命令行运行这些查询最终会变得像输入`cd`来更改目录一样熟悉！

# `find`命令

按名称搜索文件是任何文件浏览器系统的核心功能。实现这一点的 Linux 命令是`[find](https://linux.die.net/man/1/find)`命令。`find`命令需要以下语法:

`find [-H] [-L] [-P] [-D debugopts] [-Olevel] [path...] [expression]`

简单来说，这相当于:

`find [path] [options] [name]`

该命令将从[path]开始递归搜索由选项指定的名称。以下是一些常见的查找命令:

## **1。搜索具有给定名称的文件**

find 命令可用于按名称搜索文件。

`find . -name [name]`

这将返回所有名为[name]的文件。名称字段中还可以提供一个正则表达式。

`find . -name "*.json"`将返回所有以“.”结尾的文件。json”。

## **2。搜索具有给定名称的目录**

-type 标志指示搜索应该返回哪种类型的文件。find 命令识别的类型包括:

```
f: regular file
d: directory
l: symbolic link
c: character devices
b: block devices
```

`find . -type d -name "src"`将返回所有名为“src”的目录。

# grep 命令

`[grep](https://linux.die.net/man/1/grep)`命令用于在文件系统中搜索文本。当在文件系统中搜索字符串时，这是一个非常强大的工具。

`grep`命令需要以下语法:

`grep [OPTIONS] PATTERN [FILE...]`

以下是一些常见的 grep 命令:

## **1。在文件中搜索给定的字符串**

`grep test test.py`

返回文件“test.py”中包含字符串“test”的行

## **2。在多个文件中搜索给定的字符串**

`grep test *.py`

返回当前目录下所有 python 文件中包含字符串“test”的行

## **3。在多个文件中递归搜索字符串**

在所有子目录中递归搜索字符串是一种常见的操作。`-r`选项用于指定搜索应该是递归的:

`grep -r test .`

返回所有递归子目录中包含字符串“test”的行。

## **4。不区分大小写的搜索**

在不区分大小写的搜索中使用选项`-i`是一个常见的选项:

`grep -r -i test .`

返回所有递归子目录中包含不区分大小写的字符串“test”的行。

# 结论

本文概述了如何使用 **find** 和 **grep** 命令从命令行运行基本的查找和搜索查询。当有效地导航文件系统时，使用命令行运行这些查询是必不可少的。查看下面的手册页以获得更多的文档。

# 资源

*   [找到手册页](https://linux.die.net/man/1/find)
*   [**grep** 手册页](https://linux.die.net/man/1/grep)