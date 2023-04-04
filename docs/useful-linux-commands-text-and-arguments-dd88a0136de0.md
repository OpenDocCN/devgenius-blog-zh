# 有用的 Linux 命令—文本和参数

> 原文：<https://blog.devgenius.io/useful-linux-commands-text-and-arguments-dd88a0136de0?source=collection_archive---------5----------------------->

![](img/db71fa446353278357b50d977c6544a4.png)

[Max 陈](https://unsplash.com/@maxchen2k?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# xargs

`xargs`命令让我们传递一个命令的输出，并把它作为另一个命令的参数。

一般语法是:

```
command1 | xargs command2
```

例如，我们可以写:

```
cat deleteall.txt | xargs rm
```

到`deleteall.txt`中的文件路径，到`rm`命令删除`deleteall.txt`中列出的所有文件。

使用`-p`开关，我们会在命令运行前看到一个确认提示:

```
cat deleteall.txt | xargs -p rm
```

我们可以使用`-I`开关运行多个命令:

```
command1 | xargs -I % /bin/bash -c 'command2 %; command3 %'
```

这些命令是在`%`之后的命令。

# df

`df`命令让我们获得磁盘使用信息。

我们可以使用`-h`开关来获取人类可读格式的值。

# 诺胡普

`nohup`命令让我们运行一个在终端被终止时不会结束的命令。

我们只是把命令放在`nohup`之后运行它。

# 差速器

`diff`让我们比较两个文本文件的内容。

例如，我们运行:

```
diff dogs.txt dogs2.txt
```

比较`dogs.txt`和`dogs2.txt`的内容。

`-y`开关将使`diff`逐行比较 2 个文件。

我们还可以用它和`-r`开关比较目录:

```
diff -r dir1 dir2
```

我们可以用`r`和`q`开关一起比较哪些文件不同:

```
diff -rq dir1 dir2
```

# uniq

`uniq`命令让我们从文件中获得唯一的文本行。

例如，我们运行:"

```
uniq dogs.txt
```

从`dogs.txt`文件中获取唯一的文本行。

我们还可以用它从带有`|`操作符的命令中获得独特的输出行:

```
ls | uniq
```

我们可以在使用`sort`命令获得唯一行之前对行进行排序:

```
sort dogs.txt | uniq
```

我们可以用`-d`开关只显示重复的行:

```
sort dogs.txt | uniq -d
```

`-u`开关使其只显示唯一的线:

```
sort dogs.txt | uniq -u
```

`-c`开关让我们统计每条线的出现次数:

```
sort dogs.txt | uniq -c
```

# 分类

`sort`命令让我们对文本行或文本命令输出进行排序。

例如，我们运行:

```
sort dogs.txt
```

对`dogs.txt`中的行进行排序。

我们可以使用`-r`开关以相反的顺序对项目进行排序:

```
sort -r dogs.txt
```

我们可以使用`-u`开关只返回唯一的行:

```
sort -u dogs.txt
```

我们还可以使用`sort`对命令输出进行排序。

例如，我们可以用以下方式对`ls`命令的输出进行排序:

```
ls | sort
```

# 结论

我们可以向命令传递参数，用各种 Linux 命令区分和操作文本输出。