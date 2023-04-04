# 有用的 Linux 命令—网络和压缩

> 原文：<https://blog.devgenius.io/useful-linux-commands-network-and-compression-f86f712c3abf?source=collection_archive---------5----------------------->

![](img/7ffad27f91d1893369e169f0c49afc25.png)

Marc-Olivier Jodoin 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# 出口

`export`命令让我们将变量导出到子进程。

例如，我们运行:

```
export TEST="test"
```

将`TEST`变量设置为`'test'`。

我们还可以通过使用右侧表达式中的`$`来引用另一个变量。

例如，我们运行:

```
export PATH=$PATH:/new/path
```

引用右侧`PATH`环境变量的现有值。

# 水手

我们可以运行`tar`命令来归档文件。

为了创建一个 tar 归档文件，我们运行:

```
tar -cf archive.tar file1 file2
```

用 tar 档案中的`file1`和`file2`文件创建一个 tar 档案。

我们可以通过运行以下命令从当前文件夹的归档文件中提取文件:

```
tar -xf archive.tar
```

我们可以使用以下命令将归档文件提取到特定目录:

```
tar -xf archive.tar -C dir
```

`x`代表提取，`f`表示写文件。

`c`意为创造。

`z`选项让我们创建一个 gzip 归档文件。

例如，我们运行:

```
tar -czf archive.tar.gz file1 file2
```

创建一个包含`file1`和`file2`的 gzip 文件。

我们可以用以下内容来压缩 gzip 档案:

```
tar -xf archive.tar.gz
```

# traceroute

`traceroute`命令列出了到达主机所经过的所有节点。

例如，我们运行:

```
traceroute google.com
```

来查看数据包到达谷歌的路径。

# 砰

`ping`命令让我们 ping 一个网络主机，看看它是否响应。

例如，我们运行:

```
ping google.com
```

看看谷歌是否上线了。

我们可以用`-c`开关持续 ping 它:

```
ping -c google.com
```

只有当我们按 ctrl+c 时，它才停止 ping。

# gunzip

`gunzip`命令让我们解压缩 zip 存档文件。

例如，我们可以运行:

```
gunzip filename.gz
```

提取`filename.gz`文件中的文件。

我们可以使用`-c`开关提取一个文件到不同的文件名:

```
gunzip -c filename.gz > bar
```

然后我们将`filename.gz`提取到`bar`文件中。

# gzip

`gzip`命令让我们创建一个压缩文件。

例如，我们运行:

```
gzip filename
```

压缩`filename`文件。

我们可以用`-c`开关重命名 gzipped 文件:

```
gzip -c filename > filename.gz
```

然后我们将文件压缩成`filename.gz`文件。

`-k`选项也让我们改变压缩文件的名称:

```
gzip -k filename
```

我们可以用`-<number>`开关设置压缩级别。

例如，我们运行:

```
gzip -1 filename
```

用一级压缩来压缩`filename`。

我们可以通过添加用空格分隔的文件名来压缩多个文件:

```
gzip file1 file2
```

我们用一个`gzip`命令压缩`file1`和`file2`。

此外，我们可以使用`-r`开关递归压缩目录中的所有文件:

```
gzip -r folder
```

`-v`开关打印压缩百分比信息。

`-d`开关解压缩文件:

```
gzip -d filename.gz
```

# 结论

我们可以压缩文件和文件夹，用一些基本的 Linux 命令完成基本的网络任务。