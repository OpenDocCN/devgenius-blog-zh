# 每个开发人员都应该知道的 12 个终端命令

> 原文：<https://blog.devgenius.io/12-terminal-commands-every-developer-should-know-5930b9da2161?source=collection_archive---------4----------------------->

使用这些终端快捷方式，最大限度地提高开发人员的工作效率

![](img/7d867b4da3907fe44ed1b9f21e2010e7.png)

作者图片

Linux 是一个开源操作系统，它甚至比 Windows 更容易使用。让我们看看作为开发人员应该知道的一些命令。

## 打印工作目录

这个命令将打印当前的工作目录。例如，如果我在我的终端上运行它，我会得到这样的结果。

```
$ pwd
/home/vaati
```

## 限位开关（Limit Switch）

ls 将显示当前目录的内容。我当前的目录如下所示:

```
~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

## 激光唱片

cd 用来改变目录。例如，如果我希望导航到桌面，我会这样做:

```
~$ cd Desktop/
```

该目录将更改为

```
~/Desktop$
```

## mkdir

mkdir 命令将在当前目录下创建一个新文件夹。例如，创建一个名为“项目”的文件夹。

```
~/Desktop$ mkdir projects
```

## 触控

触摸将创建一个新的空白文件。例如，在上面创建的项目文件夹中创建一个. txt 文件。
首先，您将使用 cd 命令导航到文件夹并创建文件。

```
~/Desktop$ cd projects
~/Desktop/projects$ touch new.txt
```

现在，如果您想检查文件是否已经创建，请使用 ls 命令

```
~/Desktop/projects$ ls
new.txt
```

## cp(副本)

cp 会将文件从当前目录复制到不同的目录。cp 命令有两个参数，源和目标，如下所示。

```
cp **[Option]** source destination
```

例如，如果您想要在 projects_1 中创建一个尚不存在的项目目录的副本，您将这样做:

```
~/Desktop$ cp -r projects projects_copy 
```

现在导航到新创建的目录，内容应该与原始文件相同。

```
~/Desktop$ cd projects_copy/
~/Desktop/projects_copy$ ls
new.txt
```

## mv(移动)

mv 用于将文件从当前目录移动到另一个目录。mv 有两个参数，如下所示

```
mv [Option] source destination
```

例如，将 projects_copy 内容移到名为 projects_2 目录的文件夹中

```
~/Desktop$ mv projects_copy projects_2
```

现在，如果您尝试访问 projects_copy，您将会得到一个错误，因为它已经被移动了

```
~/Desktop$ cd projects_copy
bash: cd: projects_copy: No such file or directory
```

## rm(移除)

rm 用于删除目录中丢失的所有内容。例如，在名为 2022 的文件夹中创建一个文件 diary.txt 并删除它

```
~/Desktop$ mkdir 2022~/Desktop$ cd 2022
~/Desktop/2022$ touch diary.txt
~/Desktop/2022$ rm diary.txt
```

如果您希望删除整个目录，请执行以下操作:

```
~/Desktop$ rm -r 2022/
```

`-r` 表示递归，用于目录

## netstat

nestat 用于显示您的计算机正在侦听的所有活动 TCP 连接。开发人员最常用的命令是 netstat -nlpt，它检查正在运行的 HTTP 服务器

```
~$ netstat -nlpt
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:5432          0.0.0.0:*               LISTEN      -                   
tcp6       0      0 ::1:6379                :::*                    LISTEN      -                   
tcp6       0      0 ::1:631                 :::*                    LISTEN
```

## 杀

顾名思义，kill 用于终止一个进程。让我们假设您有一个运行在 [127.0.0.1:5000](http://127.0.0.1:5000) 的本地 python 服务器，并且您希望终止它；您将首先发出我们在上面使用的 nestat 命令来查找进程 id

```
~$ netstat -nlpt
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:5000          0.0.0.0:*               LISTEN      9112/python3        
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:5432          0.0.0.0:*               LISTEN      -                   
tcp6       0      0 ::1:6379                :::*                    LISTEN      -                   
tcp6       0      0 ::1:631                 :::*                    LISTEN      -
```

进程 id 是 9112，要终止服务器，您将运行以下命令

```
~$ kill -9 9112
```

选项 `-9` 表示强制

## wget

wget 会从网上下载一些东西。例如，如果你想下载 chrome，你可以输入 wget，然后输入 chrome 在互联网上的网址

```
wget wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

您应该得到这样的结果:

```
--2022-04-20 11:30:20--  [https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb](https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb)
Resolving dl.google.com (dl.google.com)... 172.217.170.206, 2a00:1450:401a:805::200e
Connecting to dl.google.com (dl.google.com)|172.217.170.206|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 87064480 (83M) [application/x-debian-package]
Saving to: ‘google-chrome-stable_current_amd64.deb’
```

## chmod(改变模式)

chmod 用于更改文件和目录的读、写和运行权限。例如，要向文件添加权限，请发出以下命令

```
chmod +rwx filename
```

要添加可执行权限，发出以下命令

```
chmod +x filename
```

# 结论

本文介绍了开发人员在日常工作中使用的一些命令。欢迎评论并提出更多建议。