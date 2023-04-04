# 有用的 Linux 命令—进程和链接

> 原文：<https://blog.devgenius.io/useful-linux-commands-processes-and-links-57c27fcda236?source=collection_archive---------7----------------------->

![](img/86260abf5990a813747b8918ab982582.png)

照片由[肯尼罗](https://unsplash.com/@kennyluoping?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# 杀

命令让我们向进程发送信号。

一般格式是:

```
kill <PID>
```

其中`PID`是进程 ID。

我们可以发出这样的信号:

```
kill -HUP <PID>
kill -INT <PID>
kill -KILL <PID>
kill -TERM <PID>
kill -CONT <PID>
kill -STOP <PID>
```

`HUP`意为挂断。当启动一个进程的终端窗口在终止它之前被关闭时，它被发送。

`INT`表示中断。当我们按 ctrl+c 时，它发送信号。

`KILL`被发送到 OS 内核来停止和终止进程。

`TERM`表示终止。接收它的进程将终止。

`CONT`表示继续。它让我们恢复一个停止的进程。

`STOP`被发送到 OS 内核而不是进程，它停止但不终止进程。

我们也可以用一个数字来代替这些信号。1 是`HUP`，2 是`INT`，9 是`KILL`，15 是`TERM`，18 是`CONT`。

# 顶端

`top`命令让我们列出实时运行的进程。

我们可以用 ctrl+c 退出`top`。

我们可以通过运行以下命令，根据使用的内存量对进程进行排序:

```
top -o mem
```

# 回声

`echo`让我们打印传递给它的参数。

例如，我们运行:

```
echo "hello" >> output.txt
```

将`hello`放入`output.txt`文件中。

此外，我们可以在字符串中插入环境变量:

```
echo "path=$PATH"
```

我们必须对命令中的特殊字符进行转义，如`$`。

我们可以将当前文件夹中的文件回显为:“

```
echo *
```

我们可以回显以`a`开头的文件:

```
echo a*
```

我们可以使用以下命令打印主文件夹路径:

```
echo ~
```

我们还可以打印带有`$()`的命令的结果，如下所示:

```
echo $(ls)
```

# 著名图象处理软件

`ps`命令让我们列出系统中当前运行的进程。

我们可以用`ps ax`命令列出所有进程。

`a`用于列出其他用户的进程。

`x`显示未链接到任何终端的进程。

`ps axww`在新的一行上继续命令列表，而不是将其截断。

输出的`PID`是进程 ID，`TT`告诉我们使用的终端 ID。

`STAT`告诉我们流程的状态。

`I`表示进程空闲。`R`是一个可运行的流程。`S`是睡眠时间少于 20 秒的进程。

`T`是停止的进程，`U`是处于不间断等待状态的进程，`Z`是死进程。

`+`表示进程在前台。`s`表示该流程是一个会话领导者。

# ln

`ln`让我们在文件系统中创建链接。

该命令的语法是:

```
ln <original> <link>
```

例如，我们跑步

```
ln foo.txt newfoo.txt
```

创建到`foo.txt`文件的`newfoo.txt`链接。

从用户的角度来看，链接与常规文件没有什么区别。

# 结论

我们可以用 Linux 命令管理进程和创建符号链接。