# 有用的 Linux 命令—用户、组和远程连接

> 原文：<https://blog.devgenius.io/useful-linux-commands-users-groups-and-remote-connections-b703e15aaf57?source=collection_archive---------5----------------------->

![](img/b4910cd4ca2a64563e5791f2dc98faf0.png)

由[梅丽莎·雷吉娜](https://unsplash.com/@tadmint?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# `printf`

`printf`是`echo`命令的改进版本。

它允许我们格式化并添加转义序列:

```
printf "1\n3\n2"
```

我们也可以用`<`来获取输入，并用`sort`进行排序:

```
sort <(printf "1\n3\n2")
```

# `0 / 1 / 2`

0、1 和 2 分别是标准的输入、输出和错误流。

例如，我们可以使用以下命令重定向到 stdout:

```
echo "stdout" >&1
```

我们可以用以下命令重定向到 stderr:

```
echo "stderr" >&2
```

# `users`

`users`命令显示当前登录的所有用户。

要查看系统上的所有用户，我们可以检查`/etc/passwd`。

# useradd

为了向系统添加用户，我们运行`useradd`。

例如，我们运行:

```
sudo useradd foo
```

添加`foo`用户。

# 用户模式

`userdel`让我们删除用户。

例如，我们运行:

```
sudo userdel foo
```

从系统中删除`foo`用户。

# 组

`groups`显示当前用户所属的所有组。

我们可以在`/etc/group`中看到所有组。

除非我们知道自己在做什么，否则我们不应该改变。

# groupadd

`groupadd`让我们将组添加到系统中。

我们可以跑:

```
sudo groupadd foo
```

添加`foo`组。

# groupdel

`groupdel`让我们删除一个组。

我们可以跑:

```
sudo groupdel foo
```

删除一个组。

# `cmp`

`cmp`获取两个文件之间的字节差。

例如，我们运行:

```
cmp a b
```

获取文件`a`和`b`的字节差异。

# `cut`

`cut`让我们把一条线切成几段，在上面划上一些分隔符。

`-d`标志让我们指定一个分隔符。

`-f`指定要打印的字段索引。

例如，我们运行:

```
printf "117.99.234.23" > c
cut -d'.' c -f1
```

将`117`打印到屏幕上。

# `sed`

让我们在文件中用另一个字符串替换一个字符串。

例如，我们运行

```
echo "old" | sed s/old/new/
```

用`new`移除`old`。

# `ssh`

`ssh`让我们从一台机器连接到另一台机器。

我们运行:

```
ssh –p <port> bob@1.2.3.4
```

使用用户`bob`连接到 IP 地址为 1.2.3.4 的机器。

# 单细胞蛋白质

我们可以使用`scp`将文件从一台机器安全地复制到另一台机器。

例如，我们可以运行:

```
scp –P <port> hello bob@1.2.3.4:~
```

从 IP 地址为 1.2.3.4 的机器上复制`hello`文件。

# `rsync`

`rsync`实用程序可以让我们用最少的数据传输 copt 文件。

这是通过查看文件之间的变化来完成的。

例如，我们运行:

```
rsync -av ../s/* .
```

使用`rsync`将`../s`目录中的文件与当前目录同步。

`rsync`也适用于`ssh`。

我们运行:

```
rsync -avz -e "ssh -p <port>" bob@1.2.3.4:~/s/* .
```

用 IP 1.2.3.4 把数据复制到机器上。

# 结论

我们可以打印文件，操纵用户和组，用一些 Linux 命令连接到其他机器。