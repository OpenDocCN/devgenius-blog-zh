# 有用的 Linux 命令—软链接和文件

> 原文：<https://blog.devgenius.io/useful-linux-commands-soft-links-and-files-77d5dad72d0?source=collection_archive---------4----------------------->

![](img/1808bbe7872477f3757185ae729400f1.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# 软链接

我们可以用`ln -s`命令创建软链接。

一般语法是:

```
ln -s <original> <link>
```

软链接将随着原始文件的删除而断开。

例如，我们运行:

```
ln -s foo.txt newfoo.txt
```

创建`newfoo.txt`软链接。

我们可以看到，软链接应该有 th `@`后缀，当我们运行`ls -al`命令时，它的颜色不同。

# 发现

`find`命令让我们在文件系统中找到文件和文件夹。

例如，我们可以通过运行以下命令找到所有扩展名为`.txt`的文件:

```
find . -name '*.txt'
```

我们需要在像`*`这样的特殊字符周围加上引号，以阻止 shell 解释它们。

我们还可以使用`-type d`开关来查找目录:

```
find . -type d -name src
```

`-type f`让我们只搜索文件。

让我们只搜索符号链接。

我们还可以在多个根树下进行搜索:

```
find folder1 folder2 -name foo.txt
```

我们也可以使用`-or`搜索多个关键字:

```
find . -type d -name node_modules -or -name public
```

我们可以用`-not`排除结果:

```
find . -type d -name '*.md' -not -path 'node_modules/*'
```

并且我们可以使用`-size`搜索至少具有给定大小的文件:

```
find . -type f -size +100c
```

`c`表示字节。

我们可以搜索大小在以下范围内的文件:

```
find . -type f -size +100k -size -1M
```

我们可以使用`-mtime`搜索在给定天数之前编辑过的文件:

```
find . -type f -mtime +3
```

我们可以删除使用`-delete`选项找到的文件:

```
find . -type f -mtime -1 -delete
```

# 猫

`cat`让我们向文件添加内容。

要将内容打印到标准输出，我们运行:

```
cat file
```

要打印多个文件的内容，我们运行:

```
cat file1 file2
```

我们可以通过运行以下命令将输出重定向到一个文件:

```
cat file1 file2 > file3
```

如果文件不存在，我们可以将`>`改为`>>`来创建文件。

我们可以用`-n`打印行号:

```
cat -n file
```

# 触控

`touch`命令让我们创建一个空文件。

例如，我们运行:

```
touch file
```

创建一个名为`file`的文件。

如果它已经存在，它会以写模式打开文件，并更新时间戳。

# 尾巴

`tail`命令让我们显示文件末尾的内容，

`-f`开关监视文件变化并自动更新输出。

例如，我们运行:

```
tail -f /foo.txt
```

观看`foo.txt`并更新输出。

我们可以改变用`-n`打印的行数:

```
tail -n 10 <filename>
```

我们用`-n 10`打印最后 10 行。

我们可以从特定行开始打印整个文件内容，行号前有`+`:

```
tail -n +10 <filename>
```

# 结论

我们可以创建软链接，用 Linux 命令创建和操作文件。