# 了解解释分析查询计划(第 2 部分，共 2 部分)

> 原文：<https://blog.devgenius.io/understanding-explain-analyze-query-plans-part-2-of-2-55a5d7d3404e?source=collection_archive---------9----------------------->

本文是一组 2 篇文章的第 2 部分，着重于理解使用`EXPLAIN`语句的查询计划器输出。如果你想阅读第 1 部分，你可以点击这里:[链接](https://medium.com/@schallwigmax/how-to-understand-explain-query-plans-part-1-of-2-b0ad173af333)。这篇文章在 YouTube 上也有视频格式[点击这里](https://youtu.be/IUJQVdYjHLY)。

在本文的第 1 部分中，我们看到了如何解释`EXPLAIN`语句的结果，以获得查询规划者对它计划如何执行查询以及它认为每一步的开销的估计。

然而，仅仅获得估计可能并不能说明全部情况，因为有时查询规划者的估计并不完全符合现实。因此，执行查询来获得每一步实际花费了多长时间的信息实际上是很有帮助的。这就是`EXPLAIN ANALYZE`声明允许我们做的。

以前，当使用`EXPLAIN`时，我们看到的是查询计划，如，获得最终结果所需的步骤，以及基于查询计划器认为是最佳路径的估计成本。`EXPLAIN ANALYZE` 另一方面会为我们实际执行查询，我们得到的是实际的结果。

现在，因为`EXPLAIN ANALYZE`执行查询，通常最好将`EXPLAIN ANALYZE`语句包装在`BEGIN`和`ROLLBACK`语句之间，这样查询实际上不会影响底层数据。

现在让我们重新运行之前的查询，看看结果。

```
BEGIN;
EXPLAIN ANALYZE SELECT uel.*
FROM user_event_log uel
LEFT JOIN event e on e.id = uel.event_id
WHERE e.name = ‘like’;
ROLLBACK;
```

给出查询计划:

```
Hash Join (cost=25.95..79.75 rows=5 width=117) (actual time=0.044..0.860 rows=219 loops=1)
 Hash Cond: (uel.event_id = e.id)
 -> Seq Scan on user_event_log uel (cost=0.00..50.92 rows=1092 width=117) (actual time=0.012..0.258 rows=1092 loops=1)
 -> Hash (cost=25.88..25.88 rows=6 width=4) (actual time=0.017..0.018 rows=1 loops=1)
    Buckets: 1024 Batches: 1 Memory Usage: 9kB
    -> Seq Scan on event e (cost=0.00..25.88 rows=6 width=4) (actual time=0.011..0.013 rows=1 loops=1)
       Filter: (name = ‘like’::text)
       Rows Removed by Filter: 4
Planning Time: 0.240 ms
Execution Time: 0.930 ms
```

我们现在获得了一些以前没有的额外字段。最明显的是底部的两个汇总统计数据，它告诉您在查询的计划和执行上花费了多少时间。

然而，我们现在在节点上还有一组有趣的额外统计数据。我们可以看到，除了成本估计之外，我们还获得了关于实际时间、行和循环的统计数据。

现在，这些行是实际生成的行数，实际时间是该节点的实际启动和完整时间，以毫秒为单位。这两个值都是每个循环的平均值，loops 参数告诉我们这个节点执行的频率。

在我们的例子中，每个节点只执行一次，但是随着存储的数据量变大，如果查询计划决定使用一个大于 1 的循环值，例如用一个`Nested Loop Join`代替一个`hash Join`，那么很可能会看到内部的子节点。

我们看到的另一个有趣的统计数据是在我们的`event`表扫描中被过滤器删除的行数。我们可以看到，在我们的例子中，4 行在`Sequential Scan`期间被删除。

这是一个有趣的统计数据，您应该密切关注，因为如果这个数字非常大，那么这可能是一个很好的优化机会，可以创建一个索引。因为否则您将从磁盘读取大量数据，然后再次丢弃这些数据，如果您还记得第 1 部分，我们从磁盘获取的数据量越少，我们的性能就越好。

执行`EXPLAIN ANALYZE`非常有用，因为它可以让您将计划估计与实际性能进行比较。

# 表格统计

在某些情况下，您实际上会遇到性能问题，因为优化器用来计划查询的来自`pg_statistics`数据的统计数据可能已经过时。如果是这种情况，那么你将会看到你的计划结果与实际结果之间的显著差异。

当事件表上的`Sequential Scan`返回的估计行数是 6，而实际返回的行数是 1 时，我们可以看到这样的一个实例。虽然这在我们的例子中没有引起任何问题，因为我们的`event`表很小，但是如果你的表的大小是不可协商的，这实际上会是一个相当大的问题。

您肯定希望注意这样的情况:查询计划估计将返回 1 行，而实际返回的行数不是 1，反之亦然。这可能表明您的表统计信息已经过时，并且实际上可能导致制定次优计划，因为您的查询计划器的假设已经过时。

例如，如果您的表突然发生大量更改，而`autovacuum`还不能更新您的统计数据，就会发生这种情况。虽然这里的表上的一个`ANALYZE`语句会为您更新统计数据，但这只是一个临时的解决方案，您应该后退一步，想想您的表是如何被用来辨别您是否可以改进应用程序与它们交互的方式。

`ANALYZE user_event_log;`

# 缓冲

总的来说，性能的最大因素是我们需要筛选多少数据。

一般来说，我们筛选的数据越多，我们的查询就越慢。

我们可以结合使用`BUFFERS`选项和`ANALYZE`来获取关于有多少数据通过了数据库内存的信息，即使它没有包含在最终结果中。

```
BEGIN;
EXPLAIN (ANALYZE, BUFFERS) SELECT uel.*
FROM user_event_log uel
LEFT JOIN event e on e.id = uel.event_id
WHERE e.name = ‘like’;
ROLLBACK;
```

给予:

```
Hash Join (cost=25.95..79.75 rows=5 width=117) (actual time=0.140..1.185 rows=219 loops=1)
 Hash Cond: (uel.event_id = e.id)
 Buffers: shared hit=44
 -> Seq Scan on user_event_log uel (cost=0.00..50.92 rows=1092 width=117) (actual time=0.020..0.382 rows=1092 loops=1)
    Buffers: shared hit=40
 -> Hash (cost=25.88..25.88 rows=6 width=4) (actual time=0.051..0.051 rows=1 loops=1)
    Buckets: 1024 Batches: 1 Memory Usage: 9kB
    Buffers: shared hit=1
    -> Seq Scan on event e (cost=0.00..25.88 rows=6 width=4) (actual time=0.023..0.026 rows=1 loops=1)
       Filter: (name = ‘like’::text)
       Rows Removed by Filter: 4
       Buffers: shared hit=1
Planning Time: 2.101 ms
Execution Time: 1.396 ms
```

缓冲区表示读取的数据批次。每个缓冲区有 8Kb 的内存。在这种情况下，我们看到`shared hit`，这意味着我们的缓冲值已经缓存(即存储在 RAM 内存中)。但是，如果我们重新启动 Postgres 并重新运行查询，我们将会看到:

```
Hash Join (cost=25.95..79.75 rows=5 width=117) (actual time=0.578..3.011 rows=219 loops=1)
 Hash Cond: (uel.event_id = e.id)
 Buffers: shared hit=3 read=41
 -> Seq Scan on user_event_log uel (cost=0.00..50.92 rows=1092 width=117) (actual time=0.158..2.069 rows=1092 loops=1)
    Buffers: shared read=40
 -> Hash (cost=25.88..25.88 rows=6 width=4) (actual time=0.359..0.359 rows=1 loops=1)
    Buckets: 1024 Batches: 1 Memory Usage: 9kB
    Buffers: shared read=1
    -> Seq Scan on event e (cost=0.00..25.88 rows=6 width=4) (actual time=0.330..0.333 rows=1 loops=1)
       Filter: (name = ‘like’::text)
       Rows Removed by Filter: 4
       Buffers: shared read=1
Planning Time: 6.682 ms
Execution Time: 3.402 ms
```

`shared read`意味着这些值是从磁盘中读取的，这比从 RAM 内存中读取要慢得多。

在这个例子中，我们使用了 1 个缓冲区来扫描我们的`event`表，使用了 40 个缓冲区来扫描我们的`user_event_log`表。缓冲区统计数据不是循环数的平均值，而是代表总数。

但是要注意，构建试图缓存所有内容的查询可能是非常不可预测的，因为一个类似但不相关的查询可能已经被我们应用程序的另一个用户调用过，其中一些结果已经被缓存了。而在其他情况下，这些数据可能有一段时间没有被读取，因此需要从磁盘中完全检索。

然而，我们可以记下实际使用了多少缓冲区。我们应该特别关注缓冲区数量较多的情况，因为这意味着我们要处理大量数据。这可能是通过在“由过滤器移除的行”字段中看到一个较高的数字来实现的，这意味着我们正在读取和丢弃大量数据。这里也是我们可以考虑优化的地方，看看我们是否可以重写查询，或者考虑其他方面，如添加索引。

显然，如果我们返回的行数也很高，那么我们只是在读取大量数据。然而，在这些情况下，我们应该问自己，我们真的需要每次都检索所有这些数据吗？还是我们不必要地获取了实际上并未被使用的数据？

# 摘要

`EXPLAIN ANALYZE`语句和`BUFFERS`关键字一起，可以为我们提供非常有见地的信息，让我们知道查询的哪些部分实际上是瓶颈，这样我们就可以在考虑优化查询时关注适当的地方。

然而，应该总是在更大的上下文中考虑它们，并且不是所有繁重的查询都可以被优化。如果您从一个没有`LIMIT`或`WHERE`子句的表中执行`SELECT *`，那么您就无法优化性能。

尽管在这一点上你也应该问自己，我真的需要在这里这样做吗？不能在某个地方加个`LIMIT`和`OFFSET`吗？我不能在某个地方添加一个`WHERE`子句，或者减少我的数据吗？例如，在另一个数据集上添加一个`INNER JOIN`来减少我需要检索的数据？

请记住，`EXPLAIN ANALYZE`是一个可以帮助我们更好地理解问题的工具，但是我们总是必须在我们试图完成的背景下考虑一切。