# 有用的 Linux 命令—文件操作和多个命令

> 原文：<https://blog.devgenius.io/useful-linux-commands-file-operations-and-multiple-commands-e57b32de7753?source=collection_archive---------3----------------------->

![](img/8570d83359e368b8944705c457a5ddc7.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# 较少的

`less`命令让我们显示文件的内容。

一般语法是:

```
less <filename>
```

我们可以用箭头键导航。

空格键和`b`逐页导航。

`/`让我们搜索内容。

`?`向后搜索。

`F`进入跟随模式。当文件发生更改时，更改会实时显示。

ctrl+c 退出跟随模式。

# 丙酸纤维素

`cp`让我们移动文件和文件夹。

例如，我们运行:

```
cp foo bar
```

将`foo`复制到`bar`。

我们还可以使用`-r`开关来复制文件夹:

```
cp -r fruits cars
```

我们将`fruits`文件夹的内容移动到`cars`。

# 平均变化

`mv`命令让我们移动文件和文件夹。

例如，我们运行:

```
mv foo bar
```

将`foo`移动到`bar`。

我们可以通过以下方式将文件移动到文件夹中:

```
mv grape banana fruits
```

我们将`grape`和`banana`文件移动到`fruits`文件夹。

# 限位开关（Limit Switch）

`ls`命令让我们列出文件和文件夹。

我们可以用以下方式列出给定文件夹的内容:

```
ls /bin
```

我们可以添加`a`开关来显示隐藏的文件。

`l`显示文件权限、文件和文件夹大小以及修改日期时间。

我们运行`ls -al`来显示所有的信息。

# 删除目录

`rmdir`命令让我们删除一个文件夹。

我们运行:

```
rmdir fruits
```

取下`fruits`文件夹。

我们可以用它来删除多个文件夹:

```
rmdir fruits cars
```

我们移除`fruits`和`cars`文件夹。

我们删除的文件夹必须是空的。

要删除非空文件夹，我们运行:

```
rm -rf fruits cars
```

`-r`表示递归，`f`表示强制。

# 显示当前工作目录

`pwd`显示当前工作目录。

# 激光唱片

让我们改变当前的工作目录。

例如，我们运行:

```
cd fruits
```

转到`fruits`文件夹。

`cd ..`移动到主文件夹。

`cd ../cars`移动到`cars`的父文件夹。

我们可以在`cd`中使用绝对路径，所以我们运行`cd /etc`转到`/etc`文件夹。

# mkdir

`mkdir`让我们创建一个文件夹。

例如，我们运行:

```
mkdir cars
```

在当前工作目录下创建一个`cars`文件夹。

我们运行:

```
mkdir dogs cars
```

创建`dogs`和`cars`文件夹。

我们可以使用`-p`开关创建多个嵌套文件夹:

```
mkdir -p fruits/apples
```

# !!

`!!`让我们运行最后一个命令。

# `; / && / &`

`;`让我们依次运行一个命令，如下所示:

```
ls; pwd
```

`&&`运行多个命令，但如果左边的命令失败，右边的命令将不会运行。

`&`让我们并行运行多个命令，而不是等待当前命令完成后再运行下一个命令。

# 结论

我们可以运行命令来显示文件内容，并使用一些操作符在 Linux 上运行多个命令。