# 5 个鲜为人知的有用的 Linux 命令

> 原文：<https://blog.devgenius.io/5-lesser-known-useful-linux-commands-3b7f3644248d?source=collection_archive---------3----------------------->

## Linux 操作系统是开发者最喜欢的操作系统之一。以下是该操作系统中 5 个不太为人知但非常有用的命令。

![](img/fb31077f20e14d08ca6c74f36e0a743b.png)

[疾控中心](https://unsplash.com/@cdc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 1.nl 命令

“nl”命令枚举文件中的行。“nl”命令是查看文件内容的一种不同方式，对于许多任务非常有用。

**T5【服务器@ ssh ~】$ nl 州
1 阿拉巴马
2 阿拉斯加
3 亚利桑那州
4 阿肯色州
5 加利福尼亚州
6 科罗拉多州
7 康涅狄格州。
8 特拉华州**

## 2.shuf 命令

“shuf”命令随机打乱文件中的行，或者随机选择一行。同样，随机排序文件夹中的文件，或在该文件夹中选择一个随机文件

***【server @ ssh ~】$ shuf 州
阿拉斯加
科罗拉多州
特拉华州
康涅狄格州。
亚利桑那州
加利福尼亚州
阿肯色州
阿拉巴马州*州**

***【server @ ssh ~】$ shuf States-n1
科罗拉多***

***【server @ ssh ~】$ shuf States-N2
阿拉斯加
特拉华***

## 3.须藤！！命令

在没有 sudo 的情况下运行需要权限的命令会导致权限错误。重复相同的命令有时会很困难。相反，如果您运行命令“sudo！!"，您可以重新运行开始时作为 sudo 运行的最后一个命令。

***【server @ ssh ~]$ df-KH
-bash:/usr/bin/df:权限被拒绝
【server @ ssh ~]$ sudo！！
已用文件系统大小 Avail Use%安装在
devtmpfs 1.9G 0 1.9G 0%/dev
tmpfs 1.9G 52K 1.9G 1%/dev/shm
tmpfs 1.9G 175m 1.7G 10%/run
tmpfs 1.9G 0 1.9G 0%/sys/fs/cgroup
/dev/sda 1 1014m 276m 739m 239m***

## 4.curl ifconfig.me

了解你的外部公共 IP 地址可能非常重要，尤其是在 AWS、Azure 和 GCP 等云平台上。有可能用一个简单的命令而不是写一些额外的脚本来学习你的外部公共 IP 地址。

***【server @ ssh ~】$ curl ifconfig . me
10 . 10 . 10 . 10 . 10***

同样，可以通过命令“curl ipinfo.io”了解地理位置的详细信息。

## 5.战术指令

您可以使用命令“tac”而不是“cat”来以相反的顺序查看文件的内容。

***【server @ ssh ~】$ tac 州
特拉华州
康涅狄格州。
科罗拉多
加利福尼亚
阿肯色州
亚利桑那
阿拉斯加
阿拉巴马***