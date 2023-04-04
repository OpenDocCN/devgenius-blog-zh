# 容器隔离的基础

> 原文：<https://blog.devgenius.io/basics-of-container-isolation-5eabdb258409?source=collection_archive---------4----------------------->

在过去的 5 年里，我一直在工作中使用容器，但我从来没有费心去检查引擎盖下面。对我来说,`docker`二进制文件是一个神奇的工具，它让我可以创建 docker 映像并快速旋转完全隔离的容器，没有任何麻烦。

最近我开始读一本关于集装箱安全的书，作者是利兹·赖斯。书中有一章专门讨论容器隔离，它将带您深入了解不同的基本 linux 结构，如`cgroups`、`namespaces`和`chroot`，它们是如何被一起用来将容器相互隔离的。

在这篇文章中，我将尝试提炼出一些概念，我们将看到一个小的演示，展示如何使用基本的 linux 构造来实现一个看起来像容器的过程。

![](img/e1d37843d734974d2e6782a6d1fa213c.png)

来源:[https://financial tribune . com/articles/domestic-economy/109723/Qom-exports-reach-5700 万四个月](https://financialtribune.com/articles/domestic-economy/109723/qom-exports-reach-57m-in-four-months)

# 目录

1.  资源隔离使用`cgroups`
2.  用名称空间进行资源分区
3.  组合名称空间和 chroot
4.  结论

# 1.使用 cgroups 的资源隔离

在 Linux 中，控制组(cgroups)是一个内核特性，它允许为一组进程隔离 CPU、内存、磁盘 IO、网络等资源的使用。

Linux 中每种类型的资源都有一个 cgroup 层次结构。这些层次在`/sys/fs/cgroup`被表示为伪文件系统。

您可以使用以下命令查看系统中存在的各种 cgroups。

```
sushil11gcp@isolation-demo:/sys/fs/cgroup$ lsblkio  cpu  cpu,cpuacct  cpuacct  cpuset  devices  freezer  hugetlb  memory  net_cls  net_cls,net_prio  net_prio  perf_event  pids  rdma  systemd  unified
```

正如文件夹名称所固有的，每个条目负责一种类型的资源。如果您进一步查看其中一个文件夹，您会看到可以控制的不同属性。例如，让我们看看`memory` cgroup。

```
sushil11gcp@isolation-demo:/sys/fs/cgroup$ ls memorycgroup.procs  memory.soft_limit_in_bytes
memory.limit_in_bytes  memory.max_usage_in_bytes                       memory.usage_in_bytes 
```

我截取了这个命令的输出，只向您展示了一些文件。每个文件控制一个特定的属性。例如`memory.limit_in_bytes`控制这个 cgroup 中的一个进程可以使用的最大内存。另一个名为`cgroup.procs`的文件列出了属于这个 cgroup 的所有进程。您可以操作这些文件中的一些来改变 cgroup 的行为，并且这些文件中的一些由内核编写来维护 cgroup 的当前状态。

## 创建群组

通过在 cgroup 目录下为特定资源创建一个目录，您可以为该资源创建自己的 cgroup。

```
root@isolation-demo:~# mkdir /sys/fs/cgroup/memory/sushil
```

内核将在这个目录中为属性和状态创建所有必要的文件。

```
root@isolation-demo:~# ls /sys/fs/cgroup/memory/sushilcgroup.clone_children  memory.kmem.failcnt             memory.kmem.tcp.limit_in_bytes      memory.max_usage_in_bytes        memory.move_charge_at_immigrate  memory.stat            tasks
cgroup.event_control   memory.kmem.limit_in_bytes      memory.kmem.tcp.max_usage_in_bytes  memory.memsw.failcnt             memory.numa_stat                 memory.swappiness
cgroup.procs           memory.kmem.max_usage_in_bytes  memory.kmem.tcp.usage_in_bytes      memory.memsw.limit_in_bytes      memory.oom_control               memory.usage_in_bytes
memory.failcnt         memory.kmem.slabinfo            memory.kmem.usage_in_bytes          memory.memsw.max_usage_in_bytes  memory.pressure_level            memory.use_hierarchy
memory.force_empty     memory.kmem.tcp.failcnt         memory.limit_in_bytes               memory.memsw.usage_in_bytes      memory.soft_limit_in_bytes       notify_on_release
```

## 为进程设置内存限制

现在让我们编辑`memory.limit_in_bytes`文件，并将最大内存设置为`100 kbs`。

```
echo 100000 > /sys/fs/cgroup/memory/sushil/memory.limit_in_bytes
```

通过将 shell 的`PID`写入`/sys/fs/cgroup/memory/sushil/cgroup.procs`，将当前 shell 添加到这个 cgroup 中。

```
root@isolation-demo:/sys/fs/cgroup# ps
    PID TTY          TIME CMD
   1973 pts/1    00:00:00 sudo
   1974 pts/1    00:00:00 su
   **1975 pts/1    00:00:00 bash**
   1983 pts/1    00:00:00 psroot@isolation-demo:/sys/fs/cgroup# echo 1975 > /sys/fs/cgroup/memory/sushil/cgroup.procs
```

接下来尝试任何命令，这将杀死外壳，因为 100kb 是非常少的内存工作。

```
root@isolation-demo:/sys/fs/cgroup# ls
Killed
```

使用 cgroups 容器运行时会限制每个容器的资源使用。当您启动一个容器时，运行时会创建一个单独的 cgroup 来放置资源限制。

## Docker cgroup 演示

让我们为此做一个小演示。让我们启动一个 docker 容器，看看它是否会创建一个单独的 cgroup。

```
sushil11gcp@isolation-demo:~$ **sudo docker run -d -m 100m nginx**
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
c229119241af: Pull complete 
2215908dc0a2: Pull complete 
08c3cb2073f1: Pull complete 
18f38162c0ce: Pull complete 
10e2168f148a: Pull complete 
c4ffe9532b5f: Pull complete 
Digest: sha256:2275af0f20d71b293916f1958f8497f987b8d8fd8113df54635f2a5915002bf1
Status: Downloaded newer image for nginx:latest
17fa32e039f2dcd8ad7dfbb0a9eb192ff01f38f47a8befd7cbf3040ef5a4d20a
```

这里我们启动一个 nginx 容器，内存限制为`100 MB`。接下来，我们将检查这个容器是否有内存组。将使用容器 ID 创建 cgroup。

```
sushil11gcp@isolation-demo:~$ lscgroup memory:/ | grep 17fa32e039f2dcd8ad7dfbb0a9eb192ff01f38f47a8befd7cbf3040ef5a4d20a**memory:/docker/17fa32e039f2dcd8ad7dfbb0a9eb192ff01f38f47a8befd7cbf3040ef5a4d20a**
```

确实有一个内存 cgroup。现在让我们检查一下`memory.limit_in_bytes`文件，看看是否传播了正确的限制。

```
sushil11gcp@isolation-demo:~$ cat /sys/fs/cgroup/memory/docker/17fa32e039f2dcd8ad7dfbb0a9eb192ff01f38f47a8befd7cbf3040ef5a4d20a/memory.limit_in_bytes 
**104857600**
```

通过将 100MB 四舍五入到最接近的 KB 来设置该值。任何违反 cgroup 限制的情况都将由内核通过杀死容器来处理，在这种情况下，容器运行时将抛出 OOM。

接下来让我们理解 Linux 名称空间的用法。

# 2.用名称空间进行资源分区

名称空间是 Linux 内核的一个特性，它允许为一组进程划分资源。简单地说，如果 cgroups 限制了资源的使用，那么名称空间就限制了进程可以看到的资源。通过将一个进程放在一个名称空间中，可以限制它可以看到的资源。

支持几种不同类型名称空间:

1.  挂载(`mount`)
2.  进程 ID ( `pid`)
3.  网络(`net`)
4.  进程间通信(`ipc`)
5.  Unix 分时系统(`uts`)
6.  用户 ID ( `user`)
7.  对照组(`cgroup`)

新的名称空间可以在 Linux 内核的未来版本中引入。

每个进程只能是特定类型的单个命名空间的一部分。子进程也继承父进程的名称空间。

使用`lsns`命令可以看到系统上不同的名称空间。当您启动系统时，每种类型只有一个名称空间。

```
sushil11gcp@isolation-demo:~$ lsns

        **NS TYPE   NPROCS   PID USER        COMMAND**
4026531835 cgroup      3  1588 sushil11gcp /lib/systemd/systemd --user
4026531836 pid         3  1588 sushil11gcp /lib/systemd/systemd --user
4026531837 user        3  1588 sushil11gcp /lib/systemd/systemd --user
4026531838 uts         3  1588 sushil11gcp /lib/systemd/systemd --user
4026531839 ipc         3  1588 sushil11gcp /lib/systemd/systemd --user
4026531840 mnt         3  1588 sushil11gcp /lib/systemd/systemd --user
4026531992 net         3  1588 sushil11gcp /lib/systemd/systemd --user
```

容器使用名称空间来划分不同的资源。例如，每个容器都有自己的主机名、自己的网络堆栈、自己的一组 cgroups，并且只能看到在其中运行的进程。这是通过使每个容器成为单独名称空间的一部分来实现的。在这一节中，我们将看到如何做到这一点。

首先让我们理解问题陈述。从您的 shell 启动另一个 shell，它将是当前 shell 的子进程，并将继承它的名称空间。在启动新的 shell 之前，发出一个`ps`命令来获取当前 shell 进程可以看到的进程列表。接下来使用`sh`命令启动一个新的 shell 进程，并列出这个子 shell 可见的进程。

```
**sushil11gcp@isolation-demo:~$ ps**
    PID TTY          TIME CMD
   **1815 pts/1    00:00:00 bash**
  16787 pts/1    00:00:00 ps
**sushil11gcp@isolation-demo:~$ sh**
$ ps
    PID TTY          TIME CMD
   **1815 pts/1    00:00:00 bash**
  16788 pts/1    00:00:00 sh
  16789 pts/1    00:00:00 ps
```

您可以验证子 shell 进程可以看到父进程可以看到的所有进程。说到容器，这是不可接受的。每个容器应该只能看到自己的子进程，仅此而已。

现在让我们来解决这个问题。

让我们使用`unshare`命令在新的`pid`名称空间中启动子流程。创建一个新的名称空间需要提升权限，因此我们将使用`sudo`来完成。

```
sushil11gcp@isolation-demo:~$ **sudo unshare --pid --fork sh**
# ps
    PID TTY          TIME CMD
  26168 pts/1    00:00:00 sudo
  26169 pts/1    00:00:00 unshare
  26170 pts/1    00:00:00 sh
  26171 pts/1    00:00:00 ps
```

> 注意:`--fork`标志是为了确保新的`sh`进程被正确附加为`unshare`的子进程，而不是`sudo`的子进程

您可以通过再次以 sudo 身份运行`lsns`命令来验证`sh`进程是否在新的`PID`名称空间中运行。

```
sushil11gcp@isolation-demo:~$ sudo lsns
        NS TYPE   NPROCS   PID USER            COMMAND
4026531835 cgroup    119     1 root            /sbin/init
4026531836 pid       115     1 root            /sbin/init
4026531837 user      119     1 root            /sbin/init
4026531838 uts       114     1 root            /sbin/init
4026531839 ipc       116     1 root            /sbin/init
4026531840 mnt       109     1 root            /sbin/init
4026531860 mnt         1    23 root            kdevtmpfs
4026531992 net       116     1 root            /sbin/init
4026532203 mnt         1   200 root            /lib/systemd/systemd-udevd
4026532204 uts         1   200 root            /lib/systemd/systemd-udevd
4026532251 mnt         1   427 systemd-network /lib/systemd/systemd-networkd
4026532252 mnt         1   432 systemd-resolve /lib/systemd/systemd-resolved
4026532253 mnt         2  1245 _chrony         /usr/sbin/chronyd -F -1
**4026532260 mnt         3 16228 root            nginx: master process nginx -g daemon off;
4026532261 uts         3 16228 root            nginx: master process nginx -g daemon off;
4026532262 ipc         3 16228 root            nginx: master process nginx -g daemon off;
4026532263 pid         3 16228 root            nginx: master process nginx -g daemon off;
4026532265 net         3 16228 root            nginx: master process nginx -g daemon off;**
4026532314 mnt         1   916 root            /lib/systemd/systemd-logind
4026532315 uts         1   916 root            /lib/systemd/systemd-logind
**4026532326 pid         1 26465 root            sh**
```

另外，您可以看到由我们在上一节中开始的`nginx`容器创建的不同名称空间。

让我们继续。如果您现在发出一个`ps`命令，您将看到新的 shell 进程仍然可以看到系统范围内的所有进程。这意味着在单独的`PID`名称空间中拥有一个进程是不够的。

答案在于`ps`命令如何获取进程信息。如果您阅读[手册页](https://man7.org/linux/man-pages/man1/ps.1.html)，您会看到它读取了`/proc`目录中的虚拟文件。如果您从子 shell 进程中列出`proc`目录，您将会看到它包含主机可用的所有信息。

```
sushil11gcp@isolation-demo:~$ sudo unshare --pid --fork sh
**# ls /proc** 1    115   128   16190  17     20     25     26286  3     321  4    579  79  87   941         buddyinfo  diskstats      interrupts  kmsg         misc          schedstat  sysrq-trigger      vmallocinfo
```

这意味着一个单独的`PID`名称空间是不够的，我们需要为我们的 shell 进程有一个单独的根目录，因为`/proc`文件在根目录中。就像一个容器看不到整个主机文件系统一样，我们必须为我们的进程创建一个新的根，以限制它读取主机的`/proc`文件或任何文件。

这就是`chroot`效用发挥作用的时候。`chroot`允许改变任何进程的根目录，一旦完成，这个进程就失去了对新根目录之上的任何东西的访问，因为根目录是任何进程的最顶层目录。

```
sudo chroot NEW_ROOT_DIR RUN_COMMAND
```

`chroot`运行 root 设置为新 root 的命令。如果未给出`RUN_COMMAND`，则默认为`${SHELL}`。

让我们试着看一个例子。

```
sushil11gcp@isolation-demo:~$ echo $SHELL
/bin/bash
sushil11gcp@isolation-demo:~$ mkdir new_root
sushil11gcp@isolation-demo:~$ **sudo chroot new_root**
**chroot: failed to run command ‘/bin/bash’: No such file or directory**
```

显然，当`new_root`目录为空并且没有`/bin/bash`文件时，您会得到这个错误。新根目录中没有命令/文件。

容器中也会发生这种情况。当您运行一个容器时，新的进程获得一个新的根目录，新的根目录由 docker 映像的内容填充，docker 映像成为容器的根文件系统。

让我们努力效仿这一点。下载 alpine linux inside `new_root`目录，看看我们的新进程能否使用它。

```
mkdir alpine
cd alpine
curl -o alpine.tar.gz [http://dl-cdn.alpinelinux.org/alpine/v3.10/releases/x86_64/alpine-minirootfs-3.10.0-x86_64.tar.gz](http://dl-cdn.alpinelinux.org/alpine/v3.10/releases/x86_64/alpine-minirootfs-3.10.0-x86_64.tar.gz)
tar xvf alpine.tar.gz
rm alpine.tar.gz
cd ..
```

现在让我们再次启动一个新的进程，但是以`alpine`目录作为根目录。

```
sushil11gcp@isolation-demo:~$ **sudo chroot alpine ls**
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
```

新进程现在可以看到由 alpine linux 实例化的根文件系统。用这个新根缝一个壳，然后修补一下。

```
sushil11gcp@isolation-demo:~$ **sudo chroot alpine sh**
/ # ps
PID   USER     TIME  COMMAND
/ #
```

发出一个`ps`命令，它将返回一个空列表。您可以通过列出同样为空的`/proc`目录来验证这一点。现在我们的新进程有了一个独立于主机的`/proc`目录。这个目录现在可以被单独的`PID`命名空间中的新进程用来写入进程信息。

# 3.组合名称空间和 chroot

现在让我们将`unshare`和`chroot`结合起来，给这个过程一个单独的`/proc`目录。

```
sushil11gcp@isolation-demo:~$ **sudo unshare --pid --fork chroot alpine sh**
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # ps
PID   USER     TIME  COMMAND
/ #
```

结果仍然看不到任何进程信息。这是因为您需要将`/proc`目录挂载为`proc`类型的伪文件系统。

```
mount -t proc proc proc
```

这个命令告诉将`/proc`目录挂载到类型为`proc`的`/proc`中。我知道这有点令人困惑。请参考 mount 命令的[手册页以更好地理解它。一旦完成，再次发出`ps`命令，您将开始看到进程信息。](https://man7.org/linux/man-pages/man8/mount.8.html)

```
/ # **mount -t proc proc proc**
**/ # ps**
PID   USER     TIME  COMMAND
    1 root      0:00 sh
    5 root      0:00 ps
```

您还可以看到,`PID`已经在这个 shell 中重启，这也巩固了对这个进程确实在一个单独的`PID`名称空间中的理解。

# 4.结论

在这篇文章中，我们看到了容器如何使用基本的 Linux 概念，比如 cgroups、namespaces 和 chroot 来实现隔离。除了本文中提到的名称空间，容器还可以使用更多的名称空间，比如`UTS`使用单独的主机名、`mount`使用卷挂载、`user`使用单独的用户列表、`net`使用单独的网络堆栈、`ipc`使用单独的通道进行进程间通信、`cgroup`名称空间在容器内部使用单独的 cgroups。

涵盖所有这些名称空间超出了本博客的范围，我强烈建议您研究更多的名称空间，并在互联网上阅读更多关于它们的内容。

# 参考

1.  [https://www . nginx . com/blog/what-are-namespaces-cgroups-how-do-they-work/](https://www.nginx.com/blog/what-are-namespaces-cgroups-how-do-they-work/)
2.  [https://en.wikipedia.org/wiki/Linux_namespaces](https://en.wikipedia.org/wiki/Linux_namespaces)