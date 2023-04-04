# 有用的 Linux 命令—文件和历史

> 原文：<https://blog.devgenius.io/useful-linux-commands-files-and-histories-52b31a4191d1?source=collection_archive---------9----------------------->

![](img/fcb4628b78fc5952d64709a548181fa7.png)

照片由[娜塔莉亚 Y](https://unsplash.com/@foxfox?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# 乌梅

`uname`命令让我们打印当前机器和其上运行的操作系统的详细信息。

我们可以使用`-m`开关来显示硬件名称。

`-p`开关打印处理器架构。

`-s`开关打印操作系统名称。

`-r`打印版本，`-v`打印版本。

`-n`打印节点网络名称。

`-a`打印打印一切。

# 男人

`man`让我们打开给定命令的帮助页面。

我们运行`man <command>`来获得命令帮助。

手册页分为 7 个不同的组，由一个数字标识:

*   `1`是用户命令
*   `2`是内核系统调用
*   `3`是 C 库函数
*   `4`是设备
*   `5` is 文件格式和文件系统
*   `6`是游戏
*   `7`是杂项命令、约定和概述
*   `8`是超级用户和系统管理员的命令

# 可做文件内的字符串查找

`grep`命令让我们搜索带有模式/的文本

例如，我们运行:

```
grep -n document index.md
```

在`index.md`文件中搜索`document`关键字。

`-n`开关让我们显示结果的行号。

我们还可以使用`grep`来过滤另一个命令的输出。

为此，我们运行:

```
less index.md | grep -n document
```

我们用`less`打开`index.md`文件，然后我们通过管道将大纲发送给`grep`命令，在`index.md`中搜索单词`document`。

此外，我们可以用`-v`选项反转结果，以排除特定的字符串。

# umask

命令让我们为文件设置默认权限。

不带参数运行`umask`将显示当前掩码。

掩码是代表当前权限的八进制数字。

`umask -S`用人类可读的符号向我们展示了权限。

返回的数字含义如下:

*   `0` —读、写、执行
*   `1` —读写
*   `2` —阅读并执行
*   `3` —只读
*   `4` —编写并执行
*   `5` —只写
*   `6` —仅执行
*   `7` —无权限

我们可以通过将它作为参数传入来设置新值:

```
umask 002
```

我们还可以按角色指定权限:

```
umask g+r
```

# 杜（姓氏）

我们可以运行`du`命令来计算文件和目录的空间使用情况。

我们运行它来测试项目列表及其字节大小。

`-a`开关让我们打印目录中每个文件的大小。

我们可以用`sort`对结果进行排序:

```
du -h <directory> | sort -nr
```

# 历史

`history`命令让我们查看命令行历史。

我们也可以使用`!<commnand number>`从`history`输出中用给定的数字重复命令。

为了清除命令历史，我们运行`history -c`。

# 结论

我们可以用各种 Linux 命令列出文件、搜索输出和命令历史。