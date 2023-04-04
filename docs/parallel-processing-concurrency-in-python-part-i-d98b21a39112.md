# Python 中的并行处理和并发性:第一部分

> 原文：<https://blog.devgenius.io/parallel-processing-concurrency-in-python-part-i-d98b21a39112?source=collection_archive---------9----------------------->

很多人说 Python 慢。他们不知道为什么，也不知道这是否是真的。但是我决定研究 Python 的内部工作原理来找出答案。我们能在 Python 中并行运行我们的代码吗？Python 的可扩展性如何？我们将在这篇深入的文章中讨论这一点。我的目标是通过使用 Python 创建一系列文章，从初级到高级学习这个数据工程主题。

这篇文章的灵感来自 Matthew Fowler 的《Python 与 Asyncio 的并发性》一书。强烈推荐它真正深入学习这个话题。

**为什么要并行处理？**

当我们创建应用程序时，我们希望我们的用户有最好的体验。换句话说，我们希望他们能够快速加载我们的网站应用程序，如果我们有大量的网络流量，我们希望能够承载尽可能多的请求。或者，如果我们正在管理一个数据库，我们希望确保我们的查询执行得足够快。我们希望以最佳的方式设计我们的应用程序，同时处理成百上千的请求。为了实现这一点，我们需要学习如何并行运行代码。换句话说，如果某一行代码特别慢，我们不希望它成为程序其余部分的瓶颈。回想一下，当你打开一个应用程序，它冻结了屏幕，有效地禁止你移动鼠标，这是多么令人讨厌！

**并发性**允许同时处理多个任务(例如，许多用户同时向您的数据库请求 SQL 查询)。我们可以通过*异步 I/O* 在 Python 中实现这一点。这是一个允许我们在代码中实现并发的库。

**I/O 操作**是从服务器或外部设备与我们的程序交互的入站-出站操作，而 **CPU 操作**通常是由我们的引擎执行的计算。当我们谈论 **I/O 限制的操作**时，我们的意思是我们的瓶颈是由程序中的 I/O 操作引起的(例如，我们的网络 web 请求太慢)。另一方面，当我们谈论 **CPU 限制的**操作时，我们的意思是我们程序中的限制因素或瓶颈在于*计算和处理来自我们引擎的代码。*

那我们怎么改进呢？对于 I/O 受限的操作，我们可以增加网络带宽。对于 CPU 受限的操作，我们可以用更高的时钟速度获得更快的 CPU，或者用更多的 CPU 并行运行我们的代码，从而分割工作。

在 Python 中，I/O 操作可能与解压缩和压缩文件有关，例如:

```
import gzip# Example I/O operation to open a compressed filewith gzip.open('big_file.gz', 'rt') as f:
     text = f.read()
```

N 注:现代机器包含升级的 SSD(用于磁盘存储的固态驱动器),使得向 CSV 写入数据帧等任务不再受 **IO 限制，而是受 **CPU 限制。**原因是每次你保存一个数据帧到一个 CSV 文件时，在后台会有一个叫做*序列化*的过程，将每个熊猫数据类型转换成一个 CSV 数据类型。当然，序列化也会消耗资源。**

**查看这篇文章了解更多信息:**

[](https://stackoverflow.com/questions/66805376/writing-multiple-csv-files-concurrently-using-threading) [## 使用线程并发写入多个 CSV 文件

### 感谢贡献一个堆栈溢出的答案！请务必回答问题。提供详细信息并分享…

stackoverflow.com](https://stackoverflow.com/questions/66805376/writing-multiple-csv-files-concurrently-using-threading) 

另一方面，如果我们要计算一个很长的列表的总和，这是一个 CPU 限制的操作:

```
long_list = [i for i in range(1_000_000)]sum(long_list)
```

在我们的程序中，我们倾向于把它们都放在一起:CPU 绑定的代码和 I/O 绑定的代码混在一起。我们的目标是了解在哪里可以并行或并发地运行任务，并优化我们的性能。

## **背景知识**

我首先要介绍一些我们以后会用到的术语。

*   并发:同时发生的任务。在现实生活中，可以一边烧水泡茶，一边洗澡。您在不同的时间开始了每个任务，但是您同时执行了两个任务的一部分。
*   并行性:任务完全在同一时间开始。你的搭档烧水泡茶，你冲了个澡。在许多情况下，并行性需要两个人才能同时启动(即在计算中可以类比于 CPU)。

用计算术语来说，如果一个 CPU 知道在等待另一个 CPU 加载时何时切换任务，我们就可以与这个 CPU 并发。并行性，另一方面，我们在一台计算机中至少需要两个核心。

所以，得出并行包括并发但并发不一定包括并行的结论(比如上面的例子)。

CPU 如何执行并发？我们有两个选择。如果正在运行的任务太慢，我们要么让 CPU 决定何时切换任务(这被称为*抢先多任务*)，这是通过*时间片来完成的。*另一个选择是，我们实际上**通过告诉 CPU 何时应该切换到*协作多任务来帮助***CPU。作为开发人员，我们希望以尽可能最佳的方式与我们的计算机一起工作，因此我们最好在代码中发出这样的信号:‘嘿，在这一部分，你可能会遇到耗时的冗长任务，让我们同时完成！’。

合作式多任务处理让我们过得更好的原因是:

*   我们通过准确地告诉计算机何时需要并发运行来消耗更少的资源。当我们的 CPU 来回切换时，它通过*消耗内存，这个术语叫做上下文切换*。它基本上将所有正在进行的进程保存在内存中，以便在完成时重新加载它们。
*   我们作为开发人员可以比计算机更好地预见瓶颈，这就是我们被雇佣的原因，对吗？通过告诉 CPU 在特定的时间点并发运行，我们帮助在应用程序最需要的部分获得并发，而不是并发运行简单的任务。

## **Python 中的线程**

你可能听说过也可能没有听说过 Python 中的**线程**模块。线程可以帮助我们改进 I/O 绑定的操作，例如从 URL 下载数据，它可以从一个网站请求数据，同时等待上一个网站加载。然而，对于 CPU 受限的**操作，我们最好使用 Python 中的**多重处理**模块。开始吧！**

线程是一个独立的执行流。螺纹的一些特征包括:

*   它们与创建它们的进程共享内存。
*   一个进程将总是包含一个与之关联的线程(通常称为主线程)，然而，一个进程仍然可以创建其他线程(称为*工作线程*)。
*   线程并发工作，在单核或多核机器上运行。

尝试运行我创建的这个示例:

```
**test.py**import threading

def print_sequence(x):
    for i in range(x):
        print(i)

print("Before creating thread...")
t = threading.Thread(target=print_sequence, args=(100,))
print("Thread has been created...")
t.start()
print("Ending all...")
```

然后和这个比较…

```
**test2.py**import threading

def print_sequence(x):
    for i in range(x):
        print(i)

print("Before creating thread...")
t = threading.Thread(target=print_sequence, args=(100,), daemon=True)
print("Thread has been created...")
t.start()
print("Ending all...")
```

我们看到了什么不同？

*   在我们的第一个脚本中，我们完成了整个循环，并打印了全部 100 个数字。我们的*结局全部…* 正文打印几乎在节目的最后但不完全在节目的最后。这是我所看到的:

```
**test.py output**
...
37
38
39
Ending all…
40
41
42
43
44
45
46
....
99
```

*   然而，在我们的 **test2.py 中，**我看到了一些非常不同的东西。

```
**test2.py**
...
40
41
42
43
44
Ending all...
45
46
47
48
49
...
73Fatal Python error: _enter_buffered_busy: could not acquire lock for <_io.BufferedWriter name=’<stdout>’> at interpreter shutdown, possibly due to daemon threads
Python runtime state: finalizing (tstate=0x00000001356091d0)Current thread 0x0000000102ee4580 (most recent call first):
 <no Python frame>
Abort trap: 6
```

我们没有在 test2.py 中完成循环，但是在 test.py 中我们完成了！理解这一点非常重要。

在我们的第一个脚本中， *test.py:*

*   我们通过线程模块启动一个线程，并在打印“在我们的线程开始之前…”之后启动它。如果你注意的话，我们没有在**守护进程上设置任何参数。**在我们的第二个脚本 *test2.py* 中，我们将**守护进程设置为 True。现在的问题是:什么是守护进程？**

守护进程可以被认为是一个在后台运行的线程，一旦程序关闭，它就会被自动终止。这意味着我们的脚本 test2.py 在完成循环之前就结束了，因为我们设置了`daemon=True`它实际上终止了循环，并且只到达了第 73 个数字而不是第 99 个。另一方面，在 *test.py* 中，我们没有把我们的线程当作守护进程，这会导致程序在完全终止之前等待整个循环结束。

*   要点提示:如果你想让你的线程完全终止，那么不要把它们设置成守护进程，因为一旦程序关闭，它们就会被杀死。在我们的例子中，程序在打印`"Ending all..."`时关闭。

考虑下面这个例子:

```
**test3.py**import threadingdef print_sequence(x):for i in range(x):print(i)print(“Before creating thread…”)t = threading.Thread(target=print_sequence, args=(100,), daemon=True)print(“Thread has been created…”)t.start()t.join()print(“Ending all…”)
```

在这种情况下，我们在程序`"Ending all..."`结束之前加入我们的过程(即确保它们结束)。我们的循环将在到达最后一行代码之前开始和结束。因此，`thread.join()`方法强制程序在继续运行之前等待所有线程**完成。**

最后一个练习是使用多线程(即同时使用多个线程)读取 CSV 文件，这将使我们能够实现并发。请注意，这只对 I/O 绑定的进程有用，因为我们受到了全局解释器锁(GIL)的约束——我们将在本文的最后一节处理这个问题。

我们将创建多个进程来为我们并行工作。

```
**test4.py** #make sure you don't name your file as 'multiprocessing.py'          #or 'threading.py'from multiprocessing.pool import ThreadPooldef row(r):
    print(r)def process_rows():
    with open("sample.csv", 'r') as f:
        for row in f:
            yield row

file = process_rows() # initiate generator function

threads_ = Pool(processes=8) # initiate with 8-worker processor

for l in file:
    threads_.map(row, (l,)) 
threads_.close()
threads_.join() # wait until all workers finish their executionSource inspiration: StackOverflow
```

[](https://stackoverflow.com/questions/44950893/processing-huge-csv-file-using-python-and-multithreading) [## 用 Python 和多线程处理巨大的 CSV 文件

### 我有一个函数可以从一个巨大的 CSV 文件中生成行:def get_next_line():用 open(sample_csv，' r ')作为 f…

stackoverflow.com](https://stackoverflow.com/questions/44950893/processing-huge-csv-file-using-python-and-multithreading) 

如你所见，多线程有很多步骤。在下一篇文章中，我们将学习更多关于 asyncio 的知识，以及如何利用 Python 的并行处理能力。

## **Python 中的全局解释器锁(GIL)**

这是 Python 社区中的一个大话题，所以我会尽量简短明了。在多线程中运行 Python 代码时，GIL 基本上会阻止您使用多个 CPU。本质上，在解释器中一次只能执行一个 Python 线程。GIL 本质上是你正在运行的每个线程在运行时获得对解释器内部的独占访问(来源:Python GIL 内部，作者 David Beazley)。

GIL 之所以存在，是因为它的 CPython 实现，这是使用一个叫做*引用计数的过程来管理的。*引用计数记录变量或数据结构(如字典)在代码中使用的次数。一旦不用，它就会从内存中被丢弃。如果两个线程同时引用 count，那么可能是一个线程停止使用它，并将其从内存中丢弃，而另一个线程开始使用它。这将导致应用程序崩溃。

尝试实现以下代码:

结果是:

*   总时间:0.57846 秒
*   穿带总时间:0.69025 秒

如你所见，多线程并没有加速我们的计算。它实际上比正常程序表现得更差。这是因为 GIL 和因为*上下文切换资源*被消耗以通过不同的线程并存储前一个线程的内存。

关键要点:

*   一般来说，对于纯 I/O 操作，GIL 被释放(即不再限制我们),但对于 CPU 受限的操作则不会。
*   如果我们不直接与 Python 对象交互(比如对 web 服务器的请求)。在操作系统级别，I/O 操作是并发执行的(但与 Java 或 C++不同，不是并行执行的)。

## **那么，如何在 Python 中规避这个问题呢？还是应该换编程语言？**

有一些方法(我们将在接下来的文章中探讨)可以用 asyncio、创建*协程，它们基本上是可以与 I/O 绑定操作并发运行的“线程”。对于 CPU 受限的操作，我们可以创建几个进程，并将其 GIL 分配给每个进程(因此，克服了 GIL 问题)。*

## 克服 I/O 限制操作:

我们克服 I/O 限制操作的方法与*非阻塞套接字有关。*什么是编程中的*套接字？*简单来说，它是向外部网络发送请求(即 web-request)并从服务器(即我们请求的网站)接收响应的容器。这个容器可以被认为是它与外部环境通信的方式。想象一座灯塔。灯塔向船只发送光信号，船只根据光以特定的方式做出反应。这个类比的关键是，在我们向某艘船发出一些光信号后，我们可以继续做其他事情，直到我们从船上得到回应。我们不需要停下来等船回复我们。 ***这叫非阻塞套接字*** *。*

相反，我们所做的是有一个通知系统，在响应准备好的时候通知我们，我们可以从服务器收集它。一旦我们有了回应，我们就可以继续我们最初对那个特定网站所做的工作。每个操作系统都有不同的通知系统(例如用于 Linux 的 epoll)。

这就是我们在 Python 中用 ayncio 能做的事情。

好的。你可能会说。我明白了。我们只是继续做其他的事情，同时等待网站的回应，一旦我们有了回应，我们就会得到通知。我们如何用 Python 做到这一点？使用*事件循环。*

敬请期待下一部分！

最好的，

马特奥·阿雷利亚诺