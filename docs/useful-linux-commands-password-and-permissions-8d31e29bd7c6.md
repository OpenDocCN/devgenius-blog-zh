# 有用的 Linux 命令—密码和权限

> 原文：<https://blog.devgenius.io/useful-linux-commands-password-and-permissions-8d31e29bd7c6?source=collection_archive---------7----------------------->

![](img/1faa88549481ead1a87622c71df53728.png)

照片由 [Paulius Dragunas](https://unsplash.com/@paulius005?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# 厕所

命令让我们计算行数、字数或字节数。

例如，我们运行:

```
wc -l test.txt
```

统计`test.txt`中的行数。

我们用以下方法计算文件中的字数:

```
wc -w test.txt
```

我们用以下等式来检验文件中的字节数:

```
wc -c test.txt
```

`-m`标志获取正确的字节值。

# 打开

`open`命令让我们打开文件、目录和程序。

我们跑

```
open <filename>
```

打开一个文件。

它也适用于目录。

我们可以使用以下命令打开当前目录:

```
open .
```

# 密码

`passwd`命令让我们更改用户的密码。

我们运行它得到一个提示，要求输入当前密码和我们想要设置的新密码。

我们也可以运行:

```
passwd <username> <new password>
```

以超级用户身份登录时设置用户密码。

# chmod

`chmod`让我们更改文件权限。

为了改变它，我们用下面的字母运行`chmod`:

*   `a`代表所有
*   `u`代表用户
*   `g`代表团体
*   `o`代表他人

然后我们输入`+`来添加一个许可，或者输入`-`来删除它。

然后我们输入一个或多个许可符号:

*   `r` —阅读
*   `w` —写
*   `x` —执行

例如，我们运行:

```
chmod a+r filename
```

为所有用户添加对`filename`的读取权限。

我们可以通过在`+`或`-`前添加多个字母来为多个角色添加权限:

```
chmod og-r filename
```

现在，我们删除对其他和组的`filename`的读取权限。

我们可以用`-r`标志递归地应用权限。

我们也可以用数字设置文件权限。

它可以是下列之一:

*   `0`没有权限
*   `1`可以执行
*   `2`会写字
*   `3`能写，能执行
*   `4`可以阅读
*   `5`可以读取、执行
*   `6`会读，会写
*   `7`可以读、写和执行

然后我们运行:

```
chmod 777 filename
```

我们用数字为用户、组和其他人设置权限。

1、2 和 4 相加得到 7。

这给了每个人与`filename`一起工作的充分许可。

# chown

我们可以用`chown`命令改变文件或目录的所有者。

一般格式是:

```
chown <owner> <file>
```

然后，我们可以通过运行以下命令来更改权限:

```
chown user test.txt
```

我们把`test.txt`的主人换成`user`。

此外，我们可以使用`-R`标志递归地更改所有文件和目录的权限:

```
chown -R <owner> <file>
```

或者，我们可以将所有者更改为具有以下内容的组:

```
chown <owner>:<group> <file>
```

所以我们可以运行:

```
chown bob:users test.txt
```

将`test.txt`的`users`组中的所有者改为`bob`。

我们也可以用`chgrp`命令改变文件的组:

```
chgrp <group> <filename>
```

# 结论

我们可以用一些 Linux 命令更改文件权限、计算文件大小和更改密码。