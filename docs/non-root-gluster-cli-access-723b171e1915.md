# 非根 Gluster CLI 访问

> 原文：<https://blog.devgenius.io/non-root-gluster-cli-access-723b171e1915?source=collection_archive---------7----------------------->

Glusterfs 的二进制文件和文件，如果你不得不探索将属于根用户。在 nix 中，一切都是文件。因此，只有文件的所有者和允许组中的人才能执行二进制文件。这是否意味着非根用户不能运行 Glusterfs？

如果一个人在一个系统中工作，其中默认用户是不属于 sudoers 文件的**和**。这意味着他们将无法访问(无论是打开文件还是告诉系统执行该文件的内容)文件系统中不属于他们的组或者不属于**和**用户的文件。

如果**蚂蚁**运行一个 gluster 命令，比如说

```
# gluster vol status
```

他们会得到甜蜜的回应

```
ERROR: failed to create logfile "/var/log/glusterfs/cli.log" (Permission denied)
ERROR: failed to open logfile /var/log/glusterfs/cli.log
```

原因是 **ant** 没有运行 gluster cli 命令的必要权限。现在有各种各样的解决方法..

1.  将 ant 添加到 sudoers 组并使用 sudo 运行命令
2.  设置二进制文件的 setuid 位。

路径 1 非常简单，所以我们不会深入研究它，而且，这也回避了这样一个问题:如果系统管理员非常坚决地不为 **ant 提供 sudo 特权，该怎么办？**

所以，输入 setuid 位…

setuid 位是 nix 中的一个特殊权限，其中某些文件(同样，所有文件都是文件！！)可以被一个用户访问，该用户在正常情况下甚至不应该访问所述文件，但是由于该文件设置了一个位，所以能够访问。这个位允许非根用户以扩展权限运行文件(换句话说，与文件所有者相同的权限)。

因此，root 是 gluster 二进制文件的所有者，为了允许 **ant** 运行 gluster cli 命令，需要设置/usr/sbin/gluster(假设我们只是在谈论 gluster cli)二进制文件的一个位。

```
# chmod u+s /usr/sbin/gluster
```

在做一个 ls 的时候，我们看到..

```
-rwsr-xr-x. 1 root root 511312 Oct 23 12:07 gluster
```

因此，我们这里有一个 s，它表示已经授予了 setuid 权限。现在，如果 **ant** 运行 gluster vol status 命令，将会收到所需的输出。

要恢复到原来的状态，可以使用相同的 chmod 命令，只需稍作修改，

```
# chmod u-s /usr/sbin/gluster
```

随着更大的特权而来的是更多的威胁……setuid 必须在经过深思熟虑和与你的系统管理员讨论后使用(不管怎样，他是一个可以给予许可的人:p ),并考虑你提供这个特权的可执行文件或文件。

人们可以探索 setuid 丰富多彩的过去，以及人们如何利用它作为攻击整个系统的媒介。