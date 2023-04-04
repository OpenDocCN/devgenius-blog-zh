# 用简单的英语解释存储结构

> 原文：<https://blog.devgenius.io/03-storage-structure-explained-in-plain-english-c6db1b785a60?source=collection_archive---------7----------------------->

## 查看我在[我的技术文章](https://yumingchang1991.medium.com/technical-article-structure-on-medium-954850e1ef4d)中的所有其他帖子

![](img/0124206cdf0e98b404e5db7b1080aadf.png)

由[尼古拉·纳托尔](https://unsplash.com/@nicnut?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

本文介绍入门级存储结构。目的是让我们掌握硬件存储在计算机中是如何工作的。

# 等级制度

存储通常分为**一级**存储、**二级**存储和**三级**存储:

*   **主存储器**又称*主存储器*、*主存储器*和*内存。*都是指 RAM，随机存取存储器。大多数 ram 都是易失性存储器，这意味着一旦断电，数据就会丢失
*   **辅助存储器**是硬盘，或者所谓的*非易失性存储器*，这意味着即使断电，数据仍然存在
*   第三级存储是一个系统，通常包括一个机器人机构，用于连接或断开可移动大容量存储介质与第二级存储的连接。它通常用于企业存储以保存档案

# 单位

## 位和触发器

虚拟数据的最小存储单位是一位，它存储计算机可以解释的二进制数据 0 或 1。一位物理上存储在触发器或锁存器上，这是一种用于存储状态(开或关，0 或 1)的电子电路。

## 注册

一位数据存储的信息太少，不能单独使用。工程师们开始将插销组合在一起。一组锁存器被称为*寄存器*。

因此，现代计算机需要读取寄存器中的所有锁存器来确定其中的数据。例如，如果计算机系统组 4 锁存在寄存器中，这意味着该计算机系统中的一个寄存器可以存储 2⁴ = 16 位的数据。

寄存器实际上就放在 CPU 旁边。当 CPU 将要执行计算时，它将所需的数据从主存储器放到寄存器中。提醒一下，CPU、主内存和辅助内存是通过同一条公共总线连接的。

## 高速缓冲存储器

缓存在物理上靠近 CPU，容量大于寄存器。我们可以想象高速缓冲存储器不像寄存器那样靠近 CPU，而是更靠近主存储器。

## 主存储器

主存储器通过公共总线连接到 CPU。

## 辅助存贮器

磁盘、磁带和光盘都是非易失性存储器，与主存储器相比容量大得多。

# 计算机中的存储是如何工作的

想象你在一个书房里，书架上放着书，一张学习桌和一把舒适的椅子供你阅读。

你一次只能读一本书。如果你想读一本不同的书，你需要合上手边的书，从椅子上站起来，把它放回书架，从书架上拿起另一本，走回书桌，坐在你舒适的椅子上阅读。

在这种情况下:

*   你就是 *CPU*
*   手边的书就是要读的资料，你的手就像一个*登记簿*
*   书架上剩下的书是存储在*一级存储器*或*二级存储器*中的数据
*   你从学习桌走到书架的路径就是*公共汽车*

也许你对学习桌感到疑惑。我们可以把一些精选的书放在学习桌上，这样我们就不必每次都站起来走到书架前。这就是高速缓存发挥作用的地方。它将数据放在离你(CPU)很近的地方，这样它就节省了获取另一本书的时间，从而产生更好的性能。

希望你喜欢阅读这篇短文！

母亲节快乐！

# 参考

*   **YouTube**:[*OS(存储结构)基础知识*](https://www.youtube.com/watch?v=YcRd3WMbXnE&list=PL9hkZBQk8d1zEGbY7ShWCZ2n1gtxqkRrS&index=3)*Neso Academy，2017*
*   ***文章** : [*微处理器如何工作*](https://computer.howstuffworks.com/microprocessor.htm) 作者:Marshall Brain&Chris Pollette，2021*
*   ***文章** : [*记忆层次*](https://www.tutorialandexample.com/memory-hierarchy) 由教程和例子，2019*
*   ***文章:** [*什么是触发器*](https://www.computerhope.com/jargon/f/flipflop.htm) 由计算机希望，2017*
*   ***文章** : [*关于触发器电路你需要知道的一切*](https://www.circuitbasics.com/what-are-flip-flops/) 作者 Louvil Abasolo*