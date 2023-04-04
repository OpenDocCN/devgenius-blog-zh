# EC2:设备上没有剩余空间错误

> 原文：<https://blog.devgenius.io/ec2-no-space-left-on-device-error-539fac18ac24?source=collection_archive---------3----------------------->

## 亚马逊 EC2 Ubuntu

![](img/7ef65f8728b8ec6edf97cb512ffa81cd.png)

由 [Unsplash](https://unsplash.com/s/photos/spill?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Curology](https://unsplash.com/@curology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

就在昨天，迎接我的是一件令我吃惊的事情，完全出乎我的意料。我连接到 AWS 上的一个远程服务器，提交我对 EC2 上运行的 PDF 报告生成器服务所做的一些更改。当我试图从 Github 中取出我的改变时，我被迎接了

```
[log_config:warn] [pid 10720:tid 140282565658368] (28)No space left on device: 
```

我感到困惑的是，在这个服务器上运行的应用程序相当小。我不知道从哪里开始找。以下是最终解决问题的步骤。

*   **首先，使用下面的命令检查您有多少可用空间以及哪个分区被填满:**

```
$ df -h
```

输出将如下所示

```
Filesystem      Size  Used Avail Use% Mounted on
udev            992M     0  992M   0% /dev
tmpfs           200M   21M  180M  11% /run
/dev/xvda1       10G   10G     0 100% /
tmpfs          1000M     0 1000M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs          1000M     0 1000M   0% /sys/fs/cgroup
tmpfs           200M     0  200M   0% /run/user/1000
```

从表面上看，该实例已经用完了磁盘空间，但是是什么填满了所有应该是空的空间呢？

*   **第二，找出哪些深藏不露的大文件被吸走储存。**

```
$ sudo du -shx /* | sort -h
```

这可能需要一点时间，一旦完成运行，它将返回以下内容

```
...
4.0K    /srv
8.0K    /snap
16K     /lost+found
28K     /root
6.5M    /etc
7.1M    /tmp
14M     /sbin
16M     /bin
21M     /run
75M     /boot
185M    /lib
218M    /home
550M    /var
4.8G    /usr
```

*   看来`/usr`目录还需要进一步检查，调整之前的命令。

```
$ sudo du -shx /usr/* | sort -h
```

然后我们会看到类似

```
...
4.0K    /usr/games
20M     /usr/include
35M     /usr/sbin
221M    /usr/bin
221M    /usr/share
3.8G    /usr/src
461M    /usr/local
488M    /usr/lib
```

在我的例子中，`/usr/src`目录已经满了。

再次调整命令，这样我们可以进一步检查`/usr/src`目录。

```
$ sudo du -shx /usr/src/* | sort -h
```

`/usr/src`目录中有大量的 Linux 头文件，占据了整个驱动器。

```
13M     /usr/src/linux-headers-4.4.0-1085-aws
13M     /usr/src/linux-headers-4.4.0-1087-aws
13M     /usr/src/linux-headers-4.4.0-1107-aws
106M    /usr/src/linux-aws-headers-4.4.0-1085
106M    /usr/src/linux-aws-headers-4.4.0-1087
106M    /usr/src/linux-aws-headers-4.4.0-1107
```

据我所知，这是由于 Ubuntu 服务器的自动更新。我仍然不确定为什么服务器会慢慢地自我毁灭而没有发出任何警报，但至少这是你的实例被填满的原因之一。

*   最后，使用下面的命令安全地清除那些内存吸取头，并庆祝您的胜利。

```
$ sudo apt autoremove
```

运行`$ df -h`来检查并确保你已经重新获得了空间。

```
Filesystem      Size  Used Avail Use% Mounted on
udev            992M     0  992M   0% /dev
tmpfs           200M   21M  180M  11% /run
/dev/xvda1      9.7G  2.9G  6.8G  30% /
tmpfs          1000M     0 1000M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs          1000M     0 1000M   0% /sys/fs/cgroup
tmpfs           200M     0  200M   0% /run/user/1000
```

您也可以使用`$ lsblk`获得更好的视角

```
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/xvda1     ext4  9.7G  2.9G  6.8G  30% /
```

如果您仍然需要更多的空间，只需重复前面概述的步骤，找到更多的大目录或扩展您的分区。一旦通过 AWS 控制台扩展了分区，记得重新启动实例以应用更改。