# 一言以蔽之，扮演皮弗林克

> 原文：<https://blog.devgenius.io/playing-pyflink-in-a-nutshell-3abd16467677?source=collection_archive---------7----------------------->

## 了解 PyFlink 的力量

![](img/13c527a98d50c00eb550926ebbddb3c6.png)

由 [Artturi Jalli](https://unsplash.com/@artturijalli) 在 [Unsplash](https://unsplash.com/photos/g5_rxRjvKmg) 上拍摄的照片

上周，我们介绍了[如何构建 PyFlink 实验环境](/playing-pyflink-from-scratch-65c18908c366)，今天我们将使用该实验环境来探索 PyFlink 的可能性。

PyFlink 是一个通用的流框架，将流处理抽象为四个层次。

1.  结构化查询语言
2.  表 API
3.  数据流
4.  有状态流处理

越靠近底部，灵活性越大，但也需要编写更多的代码。我希望能够用 PyFlink 做几乎所有的事情，所以让我们从数据流的角度开始了解 PyFlink 开发的基本概念。

本文将用简单的描述和例子介绍 PyFlink 开发的几个要点，但不会提及 Flink 的实现细节。

# 数据流概念

数据流的开发将遵循以下过程。

![](img/e27de6bda651d48a9cb28dba941b39e5.png)

基本上，我们从一个源获得流数据，处理它，然后输出到某个地方。

这用 PyFlink 表示如下。

```
ds = env.add_source(kafka_consumer)
ds = ds.map(transform, output_type=output_type_info)
ds.add_sink(kafka_producer)
```

源和汇很好理解，但关键是可以用什么处理？

在官方文件中有一个所有可用操作的列表。
[https://night les . Apache . org/flink/flink-docs-release-1.15/docs/dev/datastream/operators/overview/](https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/dev/datastream/operators/overview/)

对于上面的例子，我们使用了`map`。

在 Flink 工件中还有一个操作的例子，代码放在`./examples/python/datastream/basic_operations.py`处。

# `map`和`flat_map`有什么区别？

操作列表中有两个类似的操作，`map`和`flat_map`，这两个操作有什么区别？

不同之处在于生成的输出数量。

在`map`的情况下，一个输入事件产生一个且只有一个输出事件；另一方面，`flat_map`可以生成零到多个输出事件。

让我们以实际代码为例。

```
def map_transform(i: int):
  return i * i

def flat_map_transform(i: int):
  for idx in range(i):
    yield idx

ds.map(map_transform, output_type=Types.INT())
ds.flat_map(flat_map_transform, output_type=Types.INT())
```

在这个例子中，`map`对所有输入的整数求平方并传递出去，一个输入对应一个输出。而`flat_map`输出一系列事件，输出事件的数量由输入事件决定。

如果输入为`0`，则`flat_map`的`yield`不会被触发，不会产生任何东西。

# 状态

状态是 Flink 最大的特点。

虽然我们有各种操作可用，但其中许多实际上是基于以前的事件产生结果的。我们如何保留以前的事件？这就是`State`的用武之地。

为了持久化数据，状态可以看作是一个内部存储器，状态的大小是每个节点内存的总和。

然而，状态可以保存在像`RocksDB`这样的持久存储中，以获得更大的可伸缩性。

```
from pyflink.datastream import StreamExecutionEnvironment, EmbeddedRocksDBStateBackendenv = StreamExecutionEnvironment.get_execution_environment()
env.set_state_backend(EmbeddedRocksDBStateBackend())
```

在 Flink 框架中使用状态，有两个关键点值得注意。

1.  状态只能在“键控数据流”中使用。
2.  状态基于操作，不能与其他人共享。

下面列出了所有可用的状态和参考。
[https://night les . Apache . org/flink/flink-docs-release-1.15/docs/dev/datastream/fault-tolerance/state/](https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/dev/datastream/fault-tolerance/state/)

其实`./examples/python/datastream/state_access.py`的一个例子也提供了很好的示范。

# 连接(共享状态)

如前一节所述，状态是基于操作的，不能共享，但有时我们确实需要组合两个不同的流状态，那么我们应该怎么做呢？

幸运的是，Flink 提供了`connect`,使我们能够共享同一作业中不同流的状态。

通过使用`connect`，我们可以组合不同的流，使用相同的操作，这样我们就可以共享相同的操作状态。

更具体地说，让我举一个实际的例子。有两条溪流。

*   流 1 提供了项目标识符和项目名称之间的映射。当项目名称改变时，一个事件`(item_id, item_name)`被发送到流中，所以我们只需要保存最新的状态。
*   流 2 是交易历史，包括售出的商品和订购的商品数量。

我们想要做的是，当输入任何购买时，我们必须对其进行汇总，并将最新的商品名称追加到其中。

这是经典的流式丰富模式，我在[我的上一篇文章](https://betterprogramming.pub/design-pattern-of-streaming-enrichment-17a9eb065eca)中详细解释了丰富设计模式。

这是完整的程序示例。

在`flat_map1`中处理了流 1，也就是说维护了项目编号和项目名称的映射，所以这个流不需要生成输出事件。

整个应用的核心在`flat_map2`里。我们从`self.cnt_state`中获取累计量，不仅添加新量，还将其更新回状态。然后，在输出过程中，我们从`self.state`中取相应的名字，最后输出丰富的事件。

# 结论

在最后一个例子中，我们演示了流的操作、状态和合并。

从这个例子我们很容易理解，只要我们正确的编写程序，Flink 可以做任何我们想做的事情。

我们将继续在流处理上做一些实验，如果有任何进一步的更新，我们将继续发布。