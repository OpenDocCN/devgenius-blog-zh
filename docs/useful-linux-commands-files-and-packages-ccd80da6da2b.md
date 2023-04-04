# 有用的 Linux 命令—文件和包

> 原文：<https://blog.devgenius.io/useful-linux-commands-files-and-packages-ccd80da6da2b?source=collection_archive---------8----------------------->

![](img/9728a5055195c172d9e9198ddd4f4305.png)

[Curology](https://unsplash.com/@curology?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# `head`

`head`命令输出文件的前几行。

`-n`标志指定要显示多少行。

默认显示的行数是 10。

# 尾巴

`tail`输出文件的最后几行。

我们也可以使用`-n`标志来指定显示多少行。

同样，我们可以从第`N`行开始得到文件的结尾:

```
tail -n +N
```

# 猫

`cat`连接文件列表并将它们发送到标准输出流。

# `less`

`less`是让我们查看文件的快捷工具。

它用只读窗口打开文件内容。

# `nano`

`nano`是一个小型文本编辑器。

对于初学者来说很容易使用，因为我们不需要学习很多快捷方式。

# `nedit`

`nedit`是一个小型的图形化 rextr 编辑器。

它让我们可以通过点击、拖放和语法高亮来编辑文本。

# `touch`

`touch`让我们修改现有文件的时间戳并快速创建新文件。

# `logout`

`logout`让我们退出登录的 shell。

# `ncdu`

`ncdu`让我们显示文件空间使用情况。

它会打开一个窗口，显示每个文件的磁盘使用情况。

# `top`

显示所有当前正在运行的进程及其所有者、内存使用等。

# `htop`

`htop`是`top`的互动版。

我们可以通过`-u username`来显示用户`username`拥有的进程。

# `whereis`

让我们搜索与特定命令相关的文件。

例如，我们运行:

```
whereis ls
```

找到`ls`命令和相关联机帮助页的路径。

# `whatis`

`whatis`打印手册页中的命令描述。

# `locate`

`locate`命令通过搜索缓存的文件列表来查找系统中任何位置的文件。

# 发现

`find`遍历文件系统找到我们要找的文件。

它查看系统中当前存在的文件。

`find`允许我们按文件年龄、大小、所有权、类型、时间戳、权限、文件系统深度、正则表达式等进行搜索。

# `wget`

让我们从互联网上下载一个文件

# `curl`

`curl`可以像`wget`一样使用，但是我们需要`--output`标志。

`curl`支持许多 moe 协议，比`wget`更容易获得。

`wget`只能接收数据，但是`curl`也可以发送数据。

`wget`可以递归下载文件，而`curl`不能。

# `apt`

基于 Debian 的发行版中提供了`apt`命令。

它可以用来在我们的机器上安装、升级或删除软件。

我们可以运行`apt search`来搜索包。`apt install`让我们安装软件包。

# 结论

Linux 发行版附带了许多命令，我们可以用它们来管理文件和下载文件。