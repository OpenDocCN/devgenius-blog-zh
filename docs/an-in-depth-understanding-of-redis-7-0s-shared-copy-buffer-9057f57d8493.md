# 对 Redis 7.0 共享拷贝缓冲区的深入理解

> 原文：<https://blog.devgenius.io/an-in-depth-understanding-of-redis-7-0s-shared-copy-buffer-9057f57d8493?source=collection_archive---------4----------------------->

Redis 主从复制相关问题的优化

![](img/c30a47b597defb32e1df5c1521480d43.png)

照片由[雷米 _ 洛兹](https://unsplash.com/@remyloz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/work-desk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

本文将主要分析 Redis 主从复制中内存消耗过大和拥塞的问题，以及 Redis 7.0(尚未正式发布)的共享复制缓冲区方案是如何解决这些问题的。

虽然本文的目的不是解释 Redis 主从复制的原理，但是在进入主题之前，让我们简单回顾一下 Redis 主从复制的基础知识。

Redis 主从复制分为两种主要情况。

***1。完全同步。***

当主库接收到来自从库的同步请求时，如果从库的复制历史与主库不一致，或者如果在复制积压中找不到从库请求的同步点，则它将执行与从库的完全同步。

主库通过 fork 子进程生成内存快照，然后将数据序列化为 RDB 格式同步到从库，使从库的数据在某一时刻与主库的数据一致。

***2。命令传播。***

当从库与主库完全同步时，它进入命令传播阶段。

主库向从库发送改变数据的命令，从库会执行相应的命令，保持从库和主库的数据一致。

接下来，我们将分析与 Redis 复制缓冲区相关的问题。

**问题 1:有多个从库时，主库占用内存太多。**

![](img/edb237f9f707fae43ee6f5e3e37d0c31.png)

如上图所示，对于 Redis 主库，当用户的写请求到达时，主库会将更改命令写入所有从库的缓冲区( **OutputBuffer** )和复制积压区( **ReplicationBacklog** )。

需要指出的是，完全同步时仍然会执行这个逻辑，所以在完全同步阶段往往会触发`client-output-buffer-limit`，主库与从库断开，导致主从同步失败，甚至出现连续循环。失败案例。

这里一个明显的问题是内存占用太多，所有从库的连接都是独立于主库的。

也就是说每个从库`OutputBuffer`占用的内存空间也是独立的，所以主从复制消耗的内存是所有从库缓冲区内存大小的总和。

假设我们将从库的`client-output-buffer-limit`设置为 **1GB** 。如果有三个从库，则主库可能会消耗 **3GB** 内存用于主从复制。

另外，真实环境中的从库数量不确定，也导致 Redis 实例的内存消耗不可控。

***问题二:OutputBuffer 复制释放的阻塞问题。***

为了提高多个从库完全复制的效率，减少 fork 生成的 RDB 数量，Redis 会尽量让多个从库共享一个 RDB。

如下面的代码所示，当已经有一个从库触发`RDB BGSAVE`时，后续需要完全同步的从库将共享这个 BGSAVE 的 RDB。

为了从库中复制数据的完整性，前一个从库的 OutputBuffer 将被复制到请求完全同步的从库的`OutputBuffer`中。

`copyClientOutputBuffer()`函数似乎是一个简单的缓冲复制，但可能会有阻塞问题。

因为 OutputBuffer 链表上的数据可以达到几百 MB 甚至几 GB，复制一次可能需要几百毫秒甚至几秒，阻塞问题无法通过日志或者延迟观察到，但是影响很大。

同样，当 OutputBuffer 大小达到极限时，Redis 将关闭从属库链接。

在释放 OutputBuffer 时，还需要释放几百 MB 甚至几 GB 的数据，这对于 Redis 来说需要很长的时间。

***问题三:复制积压的局限性。***

我们知道复制积压缓冲区 replication backlog 是 Redis 实现部分再同步的基础。

如果从设备可以执行增量同步，那么主设备会将缺少的数据从 ReplicationBacklog 复制到它的 OutputBuffer。

复制的数据至多是 ReplicationBacklog 的大小。为了避免数据拷贝过多的问题，我们通常不会将值设置得太大，通常是 100 兆左右。

但是在大容量的实例中，为了避免主从网络中断造成的完全同步，我们希望数值更大，这是矛盾的。

此外，当我们重置 ReplicationBacklog 大小时，会导致 ReplicationBacklog 的内容被完全清空。

因此，如果在配置更改期间断开并重新连接主从链，很可能会导致完全同步。

共享拷贝缓冲区的设计与实现。

每个从库在主库上都有自己的 OutputBuffer，但是它存储的内容是相同的。最直观的想法之一是，当命令被传播时，主库将这些命令放在全局副本数据缓冲区中。多个从库共享这些数据。

不同的从库将数据缓冲区中的不同内容复制到引用中，这是“共享复制缓冲区”方案的核心思想。

实际上，复制积压缓冲区(replication backlog)中的内容与从库输出缓冲区中的数据是相同的，所以在这个方案中，复制积压和从库共享复制缓冲区中数据的一个副本，这也避免了复制积压的内存开销。

借鉴 Redis 客户端输出缓冲区的实现方案，“共享复制缓冲区”方案中复制缓冲区(replication buffer)的表示也采用了链表的表示方法。

把 ReplicationBuffer 数据分成多个 16KB 的数据块(replBufBlock)，然后用链表来维护。

为了维护不同从库的 ReplicationBuffer 的使用信息，replBufBlock 中添加了以下字段:

*   `refcount`:块参考计数。
*   `id`:块的唯一标识符，一个单调递增的值。
*   `repl_offset`:复制块开始的偏移量。

![](img/2fa3ae3b8e00469f013fae2e6305eb7f.png)

如上图所示，ReplicationBuffer 是一个由多个 replBufBlocks 组成的链表。

当 ReplicatonBacklog 或从属库使用一个块时，它会增加正在使用的 replBufBlock 的引用计数。

可以看到，ReplicatonBacklog 使用的 replBufBlock `refcount`是 1，从库 A 和 B 使用的 replBufBlock `refcount`是 2。

当从模块使用完当前 replBufBlock(它已经向从模块发送了数据)时，它将把它的`refcount`减 1，并移动到下一个 replBufBlock，把它的`refcount`加 1。

到目前为止，上面提到的多从库过度消耗内存的问题已经通过共享复制缓冲区方案解决了。

关于输出缓冲区复制问题，目前从库的输出缓冲区描述只有对共享复制缓冲区的参考信息。

如上面的代码所示，之前所有的数据深度拷贝都变成了更新后的引用信息，也就是在使用中的 replBufBlock refcount 上加 1，这只是一个简单的赋值操作，很轻量。

OutputBuffer 释放问题怎么办？在当前的方案中，从库中释放输出缓冲区变成将它正在使用的 replBufBlock refcount 减 1，这也是一个没有任何阻塞的赋值操作。

问题 3 也很容易解决，因为 ReplicatonBacklog 只记录对 ReplicationBuffer 的引用信息。

如上面的代码所示，复制 ReplicatonBacklog 只是找到正确的 replBufBlock 并将其 refcount 加 1。

不用担心 ReplicatonBacklog 太大导致的复制阻塞问题。

此外，对 ReplicatonBacklog 大小的更改只是配置更改，不会清除数据。

感谢您阅读本文，如果您认为文章写得很好，请关注我。

如果发现本文有错误，欢迎留言讨论。

祝你愉快。