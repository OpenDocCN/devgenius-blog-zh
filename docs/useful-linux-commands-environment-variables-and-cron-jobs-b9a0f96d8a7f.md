# 有用的 Linux 命令—环境变量和 Cron 作业

> 原文：<https://blog.devgenius.io/useful-linux-commands-environment-variables-and-cron-jobs-b9a0f96d8a7f?source=collection_archive---------2----------------------->

![](img/1fca0d1034f7433403e2ecfba52449a3.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Linux 是许多开发人员都会使用的操作系统。

因此，学习一些 Linux 命令是个不错的主意。

在本文中，我们将看看一些我们应该知道的有用的 Linux 命令。

# 包封/包围（动词 envelop 的简写）

`env`命令让我们为当前会话设置环境变量。

例如，我们可以运行:

```
env USER=username node app.js
```

在当前会话中设置`USER`环境变量。

我们可以用`-i`开关清除环境变量:

```
env -i node app.js
```

我们可以使用`-u`开关使变量不可访问:

```
env -u USER node app.js
```

现在`USER`环境变量对于会话中的程序来说是不可见的。

# printenv

`printenv`命令让我们将环境变量打印到屏幕上。

我们可以通过在命令后指定它来打印一个特定的。

例如，我们运行:

```
printenv USER
```

打印`USER`环境变量的值。

# 基本名称

`basename`命令让我们返回路径的文件名部分。

例如，我们运行:

```
basename /users/foo/test.txt
```

将`test.txt`打印到屏幕上。

如果最后一段是一个目录，那么我们得到这个目录的名字。

# 目录名

`c`命令让我们获得路径的方向部分。

例如，如果我们有:

```
dirname /users/foo/test.txt
```

然后我们得到`/users/foo`显示在屏幕上。

# crontab

`crontab`命令让我们调度 cron 作业。

我们可以用`-l`开关列出安排的玉米作业:

```
crontab -l
```

我们可以跑:

```
crontab -e
```

添加新的 cron 作业。

为 cron 作业设置计划的语法如下。

要设置 cron 作业运行的分钟，我们可以添加以下一个或多个选项:

*   `*` —任何值
*   `,` —值列表分隔符
*   `-` —数值范围
*   `/` —步长值

要设置 cron 作业运行的时间，我们可以添加以下一个或多个选项:

*   `*` —任何值
*   `,` —值列表分隔符
*   `-` —数值范围
*   `/` —步长值
*   0–23

要设置 cron 作业在一个月中的哪一天运行，我们可以添加以下一个或多个选项:

*   `*` —任何值
*   `,` —值列表分隔符
*   `-` —数值范围
*   `/` —步长值
*   1–31

要设置 cron 作业运行的月份，我们可以添加以下选项之一:

*   `*` —任意值
*   `,` —值列表分隔符
*   `-` —数值范围
*   `/` —步长值
*   1–12

要设置 cron 作业在一周中的哪一天运行，我们可以添加以下选项之一:

*   `*` —任何值
*   `,` —值列表分隔符
*   `-` —数值范围
*   `/` —步长值
*   0–6—(0 表示周日，6 表示周六)
*   太阳卫星

例如，我们可以运行:

```
30 08 10 06 * /home/ramesh/full-backup
```

设置`full-backup`脚本在指定时间运行。

这个表达的意思是:

*   30–第 30 分钟
*   上午 8 点至 8 点
*   第 10 天
*   06-第六个月(6 月)
*   ***** —一周的每一天

# 结论

我们可以使用 Linux 命令来获取和设置环境变量，并查看 cron jons。