# 针对 Linux 初学者的 12 个简单的 Linux 命令

> 原文：<https://blog.devgenius.io/12-easy-linux-commands-for-linux-beginners-97f49ba444d4?source=collection_archive---------17----------------------->

#实际上，我在一次编码面试中被问到这个问题

![](img/48f5049095e7c654e084c2c230e7f0bc.png)

Gabriel Heinzer 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我不是 linux 专家——在我担任软件工程师期间，我曾不时地使用过它，但并不广泛。我在 linux 上是这样做的，一边搜索命令，一边通过反复试验让东西工作。

不久前，在一次编码面试中，我的面试官问我:

> 说出 Linux 中使用的 10 个命令

由于显而易见的原因，我不能谷歌它，所以我必须依靠我的记忆！这里有 12 个简单的 linux 命令，如果让你说出 10 个在 Linux 中使用的命令，这可能是个好主意。

## 1)残疾人

> 告诉我我现在在哪里

`pwd`代表*‘当前工作目录’*，打印出我们当前所在文件夹的完整路径(当前工作目录)。

## 2) cd<newpath></newpath>

> 将我的目录更改为<newpath></newpath>

`cd`代表*‘更改目录’*，它将我们当前的工作目录更改为我们给它的任何< newpath >。

*   `cd testing`表示*‘进入测试文件夹’*
*   `cd ..`表示*‘进入我的父文件夹’*

## 3) ls

> 打印出我当前工作目录中的所有内容

`ls`代表“列表”，显示当前工作目录下的文件和文件夹。

*   `ls <otherdirectory>`将显示另一个目录中的文件和文件夹
*   `ls -a`将显示隐藏的文件

## 4) mkdir

> 创建一个名为<newfolder>的新文件夹</newfolder>

`mkdir`代表‘制作目录’(目录==文件夹)。此命令创建一个新文件夹。

## 5)卡特彼勒

> 显示<file>的内容</file>

`cat`代表“concatenate ”,用于列出我们传递给命令的任何文件的内容。请注意，这不适用于目录。

## 6) rm <file1><file2>…</file2></file1>

> 删除文件 1，文件 2，…

`rm`代表“移除”，该命令将移除(删除)<文件 1 >、<文件 2 >、…(您赋予该命令的其他文件)。

## 7)须藤

> 我是‘超级用户’，你必须毫无疑问地服从我的 linux 命令

`sudo`代表“超级用户 do ”,我们在普通 linux 命令之前添加了这个命令，以强制执行它。

## 8) sudo rm -rf<foldername></foldername>

> 强制删除整个<foldername>目录</foldername>

`-r`表示递归，`f`表示强制。因此，我们告诉“超级用户”1)递归地 2)强制删除<文件夹名>。这将核爆(删除)文件夹名>中的所有内容。

## 9)回声

> 打印

如果我们`echo hello`，我们的终端将简单地打印`hello`。该命令单独使用时用于打印内容。`echo hello > testing.txt`将`hello`写入新文件`testing.txt`。

## 10)杀死

> 终止(完全停止)进程 ID 为<pid>的进程</pid>

在 Linux 中，每个进程都有一个进程 ID，即 PID。当我们`kill <PID>`时，我们实际上是用< PID >杀死了进程。

## 11)chmod<permissions></permissions>

> 更改<file>的读/写/执行权限</file>

`chmod`代表“更改模式”，该命令用于更改某个文件/文件夹的读/写/执行权限。如果我们运行命令`chmod u=rwx,g=rw,o=r mytestfile`:

*   用户`u`可以读取、写入和执行`mytestfile`
*   组`g`只能读写`mytestfile`
*   其他`o`只能读`mytestfile`

## 12)chown<user></user>

> 将<file>的主人改为</file>

`chown`代表“更改所有者”,它将文件的所有者更改为我们传递到命令中的任何用户。

# 一些最后的话

如果这篇文章有价值，并且你希望支持我，请考虑注册一个 Medium 会员——每月 5 美元，你可以无限制地访问 Medium 上的文章。如果你用我下面的链接注册，我会给你零额外费用赚一点佣金。

使用我的链接注册阅读无限量的媒体文章。T34

我写编码文章(主要是 Python ),我认为这些文章可能会帮助年轻的我加快我的学习曲线。请加入我的电子邮件列表，以便在我发布时收到通知。

[](https://zlliu.medium.com/subscribe) [## 每当刘发表文章时，就收到一封电子邮件。

### 每当刘发表文章时，就收到一封电子邮件。注册后，如果您还没有，您将创建一个中型帐户…

zlliu.medium.com](https://zlliu.medium.com/subscribe)