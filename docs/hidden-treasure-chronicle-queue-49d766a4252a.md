# 藏宝编年史队列

> 原文：<https://blog.devgenius.io/hidden-treasure-chronicle-queue-49d766a4252a?source=collection_archive---------14----------------------->

![](img/24ad1f710f012df15cdb6c2739942051.png)

编年史是个很酷的名字，对吧？这就像科幻电影的人工智能。给我找到铀！

但不幸的是，这只是一个队列，这也是一个很好的队列。

在我们的项目*(这是一个网络项目，我们从设备中获取数据并对其进行分析)*有一个需求，1000 万个设备对象必须被混洗并发送以从设备中获取数据。它们必须被打乱，因为你不应该同时访问多个网络设备，这会导致过多的网络活动，影响设备的网络质量。

很简单，只要拿到物品，然后洗牌就行了。

```
Collections.*shuffle*(list);
```

没有。这样做你的堆会大得惊人。1000 万对象确实很大，对象有 30-35 个属性。

如果我没记错，我们第一次尝试时，堆大小超过 10GB，应用程序崩溃了。

那么 1000 万个数据，我怎样才能在不导致堆大小大幅增加的情况下对它们进行洗牌呢？

我开始研究。我想把它们存储在一个文件中，然后随机获取它们，这样会很快，而且堆是免费的。但是你怎么能这样做呢？好的，你把所有的对象一行一行的存储到一个文件中，但是你怎么能随机的获取它们呢？文件没有行号来获取像 *lines.get(1)* 这样的数据，我想我试图找到一个随机的行，但我什么也找不到。

我不想用 db。Mongo *(我们正在使用)*已经做了太多的工作，处理起来太慢了，随机获取数据仍然是个问题。

在搜索东西后，我偶然发现了编年史队列。

当我第一次读到它的时候，我就想“哦，这是我一直想做的事情。存储在文件中，每一行都有索引，并且是在堆外的。就这样，我找到了我的解决方案。

我不会讲太多细节，但我会写我是如何解决我的问题的。

首先，我们需要创建队列；

```
try (SingleChronicleQueue queue = SingleChronicleQueueBuilder.*binary*("/list").build()) {
...
} catch (Exception e) {
    e.printStackTrace();
}
```

这样做，就创建了一个名为*“list”*的文件，在这个文件中有一个队列文件。默认情况下，每天都会创建队列，因此队列名称默认为 20200718 *(今天)*。您不需要知道您的队列名称。你的文件名*(列表)*才是重要的。

创建队列后，我们需要一个写入器和读取器。为了读和写，我们有两节课。

要将我们的对象写入队列，我们需要创建一个 appender。

```
final ExcerptAppender appender = queue.acquireAppender();
```

下一步是将 1000 万个对象写入队列。

```
...
try (final DocumentContext documentContext = appender.writingDocument()) {
    documentContext.wire().write("values").marshallable(value);
    indexes.add(documentContext.index());
} catch (Exception e) {
    e.printStackTrace();
}
...
```

在我们的队列中，我们有一个类似于 mongo 集合的键集合列表。您的文件名是您的数据库，您的集合命名您的键。在这种情况下，我们的列表值将存储在*“值”中。*marshalable 函数接受对象，所以你可以写任何你想写的东西。

如果你仔细看我的代码，有一个*索引*列表，我添加了 *documentContext.index()。*那么这是做什么呢？

我需要重新摆放我的物品，对吗？所以通过在队列中保存每个对象写的行索引，我要通过这个索引来访问。1000 万个长值并不能容纳太多的堆大小。这样我就可以洗牌，一个一个的拿。

在将我们的对象写入队列时，我注意到在 kubernates 中，我们的应用程序的 ram 快用完了。我在 kubernates 中看到，应用程序的 ram 增加到 15 GB，我认为它会崩溃。但它没有，我也不明白。因为在此之前，如果我们的应用程序的内存达到 10 GB，它会立即崩溃。

之后，我看到了应用程序的实际内存大小，它只是 1.2 GB。我又没听懂。为什么在 kubernates 中显示 15 GB，但应用程序的实际 ram 大小只有 1.2GB

在我的理解中，当我尝试本地队列文件的大小接近 15 GB 时，历史队列将数据存储在堆外。在 kubernates 豆荚可能显示太多内存，因为这一点。但是在队列关闭且任务完成后，它会立即被丢弃。

现在我们要创建我们的阅读课。裁缝。

```
final ExcerptTailer tailer = queue.createTailer();
```

我们的 1000 万数据在队列中，我们创建了 tailer，现在我们需要获取它们。但是首先我们需要打乱我们的索引列表来随机获取我们的数据。*(也许有更有效的洗牌方式，但这对我来说已经足够了)*

```
...
Collections.*shuffle*(indexes);indexes.forEach(index -> {
    tailer.moveToIndex(index);
    try (DocumentContext documentContext = tailer.readingDocument()) {
        if (documentContext.isPresent()) {Map values = documentContext.wire().read("values").object(Map.class);
            System.*out*.println(values.get("value"));
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
});
...
```

在打乱我们的索引列表后，我们一个接一个地去那个索引所指向的地方并获取我们的数据。

如你所见，我们从键*(值)*中读取，在该文件的行中，我们需要通过 Map.class 获取对象。在该 Map 类中，有一个*“值”*键保存我们的数据。这样获得数据后，你可以把它发送到你喜欢的任何地方。

就这么简单。

这样我解决了我的堆问题和洗牌问题。

你应该自己去看看，它们真的很好，有很多很酷的东西可以用。

这里是 [Github](https://github.com/OpenHFT/Chronicle-Queue) 。