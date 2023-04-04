# 有用的 Linux 命令—用户和进程

> 原文：<https://blog.devgenius.io/useful-linux-commands-users-and-processes-94e627139f6d?source=collection_archive---------6----------------------->

![](img/beba9b78ae8dc6c9db667a62ded2ed81.png)

由[奥斯汀·迪斯特尔](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# 须藤

我们可以作为另一个用户使用`sudo`命令运行一个命令。

它让我们以 root 用户身份运行命令。

例如，我们可以运行:

```
sudo nano /etc/hosts
```

以`nano`为根打开`/etc/hosts`文件。

我们还可以运行`sudo -i`以 root 身份启动一个 shell。

我们可以使用`-u`标志作为另一个用户运行:

```
sudo -u bob ls /users/bob
```

我们将`ls`作为`bob`运行。

# 苏联（USSR 的缩写）

`su`命令让我们将 shell 切换到另一个用户。

例如，我们运行:

```
su bob
```

以`bob`的身份运行 shell。

完成后，我们键入`exit`返回我们自己的 shell。

# 清楚的

`clear`让我们清除终端的屏幕。

ctrl+l 是清除屏幕的快捷方式。

`clear -x`清除屏幕，但让我们通过向上滚动返回查看之前的工作。

# 谁

`who`命令让我们显示登录到系统的用户。

每个打开的外壳都将被列出。

我们可以看到使用的终端以及会话开始的时间和日期。

`-aH`标志告诉`who`显示更多信息，如终端的空闲时间和进程 ID。

`who am i`Li 删除当前终端会话的详细信息。

# 显示本用户信息

`whoami`命令打印当前用户名。

# 哪个

`which`显示命令存储的位置。

例如，我们运行`which ls`来查看`ls`存储在哪里。

# 类型

命令让我们决定我们正在运行的命令的类型。

命令可以是 4 种类型之一:

*   可执行文件
*   外壳内置程序
*   外壳函数
*   化名

# 细粒

`fg`工具将一个在后台运行的任务放到前台。

我们可以用`job`命令获取作业的作业 ID。

而且我们可以用作业 ID 作为`fg`的自变量。

所以我们跑:

```
fg 2
```

将 ID 为 2 的作业置于前台。

# 锥齿轮

恢复已暂停的工作。

我们可以使用`job`命令获得作业的作业 ID。

我们可以使用作业 ID 作为`fg`的参数。

所以我们跑:

```
fg 2
```

恢复 ID 为 2 的作业。

# 工作

`jobs`让我们列出我们开始的工作的状态。

# 别名

`alias`命令让我们创建另一个命令的快捷方式。

例如，我们可以运行:

```
alias ll='ls -al'
```

创建别名`ll`来运行`ls -al`命令。

# 基拉耳

`killall`命令让我们向当前正在运行的多个进程发送信号。

那么一般的语法是:

```
killall <name>
```

其中`name`是程序的名称。

# 结论

我们可以运行命令来管理作业，并使用各种 Linux 命令进行处理。