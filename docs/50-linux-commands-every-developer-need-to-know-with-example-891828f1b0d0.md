# 每个开发人员都需要知道的 50 个 Linux 命令及示例

> 原文：<https://blog.devgenius.io/50-linux-commands-every-developer-need-to-know-with-example-891828f1b0d0?source=collection_archive---------0----------------------->

这里是每个开发人员都应该知道的 50 个 Linux 命令，以及每个命令的简短描述和示例:

![](img/b668e8a55c6ab0cf7c37d19f5b7e458b.png)

1.  `ls` -列出一个目录的内容。
    -举例:`ls`会列出当前目录的内容。`ls /usr/local`会列出`/usr/local`目录的内容。
2.  `pwd` -打印当前工作目录。
    -示例:`pwd`将打印当前工作目录的完整路径。
3.  `cd` -改变当前工作目录。
    -例如:`cd /usr/local`将当前工作目录改为`/usr/local`。
4.  `mkdir` -创建新目录。
    -例如:`mkdir mydir`将创建一个名为`mydir`的新目录。
5.  `mv` -移动文件或目录。
    -例如:`mv file.txt /usr/local/`将文件`file.txt`移动到`/usr/local`目录。
6.  `cp` -复制文件或目录。
    -例:`cp file.txt /usr/local/`将文件`file.txt`复制到`/usr/local`目录。
7.  `rm` -删除文件或目录。
    -例如:`rm file.txt`将删除文件`file.txt`，而`rm -r mydir`将删除目录`mydir`及其所有内容。
8.  `touch` -创建一个新的空文件。
    -例子:`touch file.txt`会新建一个名为`file.txt`的空文件。
9.  `ln` -创建一个文件或目录的链接。
    -示例:`ln -s /usr/local/file.txt file.txt`将在当前目录下创建一个指向`/usr/local/file.txt`的符号链接，名为`file.txt`。
10.  `cat` -显示文件的内容。
    -示例:`cat file.txt`将在终端显示文件`file.txt`的内容。
11.  `clear` -清除终端屏幕。
    -例如:`clear`将清除终端屏幕的内容。
12.  `echo` -向终端打印消息。
    -例:`echo "Hello, world!"`将把消息`"Hello, world!"`打印到终端。
13.  `less` -查看带分页的文件。
    -例如:`less file.txt`将允许您一次一页地查看`file.txt`的内容。
14.  `man` -显示命令的手册页。
    -示例:`man ls`将显示`ls`命令的手册页，描述其用法和选项。
15.  `uname` -显示当前系统的信息。
    -示例:`uname -a`将显示当前系统的所有信息，包括内核版本和机器硬件名称。
16.  `whoami` -显示当前用户。
    -例如:`whoami`将显示当前用户的用户名。
17.  `tar` -归档并压缩文件和目录。
    -示例:`tar -czf archive.tar.gz directory/`将从`directory`目录的内容中创建一个名为`archive.tar.gz`的压缩档案。
18.  `grep` -在文件中搜索模式。
    -示例:`grep "error" log.txt`将在文件`log.txt`中搜索图案`"error"`，并打印任何匹配的行。
19.  `head` -显示文件的前几行。
    -例如:`head -n 10 file.txt`将显示`file.txt`的前 10 行。
20.  `tail` -显示文件的最后几行。
    -例如:`tail -n 10 file.txt`将显示`file.txt`的最后 10 行。
21.  `diff` -比较两个文件之间的差异。
    -例:`diff file1.txt file2.txt`会比较`file1.txt`和`file2.txt`的内容，并打印出两者的差异。
22.  `cmp` -逐字节比较两个文件的内容。
    -示例:`cmp file1.txt file2.txt`将逐字节比较`file1.txt`和`file2.txt`的内容，并报告任何差异。
23.  `comm` -逐行比较两个排序文件的内容。
    -示例:`comm file1.txt file2.txt`将比较`file1.txt`和`file2.txt`的内容，这两个文件都应该排序，并打印每个文件唯一的行。
24.  `sort` -对文件的行进行排序。
    -示例:`sort file.txt`将按字母顺序对`file.txt`的行进行排序。
25.  `export` -导出一个外壳变量。
    -示例:`export VARNAME="value"`将创建一个名为`VARNAME`的 shell 变量，其值为`"value"`。
26.  `zip` -将文件压缩成 ZIP 存档。
    -示例:`zip archive.zip file1.txt file2.txt`将创建一个名为`archive.zip`的 ZIP 存档，包含文件`file1.txt`和`file2.txt`。
27.  `unzip` -从 ZIP 存档中提取文件。
    -例子:`unzip archive.zip`将解压`archive.zip` ZIP 文件的内容。
28.  `ssh` -使用 SSH 协议连接到远程服务器。
    -例如:`ssh user@example.com`将以用户`user`的身份连接到`example.com`的服务器。
29.  `service` -控制系统服务。
    -示例:`service apache2 start`将启动 Apache web 服务器。
30.  `ps` -显示正在运行的进程的信息。
    -示例:`ps aux`将显示所有正在运行的进程及其资源使用情况的列表。
31.  `kill` -向进程发送信号以终止进程。
    -例如:`kill 12345`将发送信号终止进程 ID 为`12345`的进程。
32.  `killall` -终止指定名称的所有进程。
    -例如:`killall firefox`将终止所有名为`firefox`的进程。
33.  `df` -显示已挂载文件系统上可用磁盘空间的信息。
    -例如:`df -h`将以人类可读的格式显示可用的磁盘空间(例如，以千兆字节或兆字节为单位)。
34.  `mount` -挂载文件系统。
    -示例:`mount /dev/sda1 /mnt/mydisk`将在挂载点`/mnt/mydisk`挂载分区`/dev/sda1`。
35.  `chmod` -更改文件或目录的权限。
    -例如:`chmod 755 file.txt`将文件`file.txt`的读、写和执行权限授予所有者，其他人则授予读和执行权限。
36.  `chown` -更改文件或目录的所有权。
    -例如:`chown user:group file.txt`会将`file.txt`的所有者更改为`user`，将组所有权更改为`group`。
37.  `ifconfig` -配置网络接口参数。
    -示例:`ifconfig eth0 up`将启用网络接口`eth0`。
38.  `traceroute` -跟踪数据包到达目的地的路径。
    -例如:`traceroute example.com`将跟踪数据包从当前系统到目的地的路径`example.com`。
39.  `wget` -从互联网下载文件。
    -举例:`wget https://example.com/file.zip`会从`[https://example.com](https://example.com.)` [下载文件`file.zip`。](https://example.com.)
40.  `ufw` -管理防火墙的前端。
    -示例:`ufw allow ssh`将允许传入的 SSH 服务连接。
41.  `iptables`-Linux 的防火墙管理工具。
    -例如:`iptables -A INPUT -p tcp --dport 80 -j ACCEPT`将允许传入连接到 TCP 端口 HTTP 的默认端口)。
42.  基于 Debian 系统的软件包管理器。
    -例如:`apt update`将更新可用软件包列表。
43.  `sudo` -允许用户以超级用户(root)的权限运行命令。
    -示例:`sudo apt update`将更新具有 root 权限的可用软件包列表。
44.  `cal` -显示日历。
    -例如:`cal`将显示当前月份的日历。
45.  `alias` -为命令创建一个别名。
    -示例:`alias ll='ls -alF'`将创建一个运行命令`ls -alF`的别名`ll`。
46.  `dd` -将数据从一个位置复制到另一个位置。
    -示例:`dd if=/dev/sda of=disk.img`将创建一个名为`disk.img`的镜像文件，该文件包含设备`/dev/sda`的内容。
47.  `whereis` -显示命令的位置。
    -例如:`whereis ls`将显示`ls`命令在系统中的位置。
48.  `whatis` -显示命令的简短描述。
    -示例:`whatis ls`将显示`ls`命令的简短描述。
49.  `top` -显示正在运行的进程的信息。
    -例如:`top`将实时显示正在运行的进程及其资源使用情况的列表。
50.  `passwd` -更改用户的密码。
    -例如:`passwd user1`将提示您输入并确认用户的新密码`user1`。

有关特定命令的更多详细信息，请访问下面的链接:

Bash 网址:`[https://linux.die.net/man/1/change_command_name](https://linux.die.net/man/1/change_command_name)`

例如，要查看 ls 命令的手册页，您可以访问以下链接:[https://linux.die.net/man/1/ls](https://linux.die.net/man/1/ls)

命令的手册页包括该特定命令或实用程序的综合参考指南，可在 Linux 或类似 Unix 的操作系统上获得。它包括命令及其选项的描述，以及如何使用该命令的示例。它还可能包含有关命令语法、返回值以及使用命令时可能出现的任何错误的信息。

您可以通过在命令提示符下键入 man 后跟命令名称来访问命令的手册页。手册页分为几个部分，每个部分包含一个特定的主题。第一部分编号为 1，包含所有用户都可以使用的命令。第二部分编号为 2，包含系统调用，这是由操作系统内核提供的功能，允许程序从内核请求服务。编号为 3 的第三部分包含库函数，这些函数是由程序使用的库提供的。

如果本指南对您和您的团队有所帮助，请与他人分享！

**关注我，了解我最新最有趣的文章。**