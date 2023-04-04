# 我如何将 Azure price 迁移到 Clickhouse 并处理副本，第 2 部分

> 原文：<https://blog.devgenius.io/how-i-migrated-azureprice-to-clickhouse-and-handled-duplicates-part-2-66102da98a10?source=collection_archive---------10----------------------->

在之前的[文章](https://medium.com/@Gaploid/how-i-migrate-to-clickhouse-and-speedup-my-backend-7x-and-decrease-cost-by-6x-part-1-2553251a9059)中，我描述了在**转移到 ClickHouse** 后我实现的迁移和好处。在第 2 部分的文章中，我想讲述在采用新的数据库时遇到的挑战，以及我是如何在不损失性能的情况下解决这些挑战的。

下面是建立在 Azure 之上的 azureprice.net[后端架构。](https://azureprice.net)

每天，Azure 的主要功能都由一个定时器触发。它生成了数千个任务来接收来自不同来源的数据，包括微软 Azure APIs 和外部服务，如外汇汇率。

所有这些任务都被插入到 Azure 队列服务中，以并行执行下一步，并增加流程的可靠性。如果其中一个任务失败，稍后将再次重新排队。每个函数合并来自不同来源的数据，并向 ClickHouse 插入一小批(几百行)数据。

该管道每天处理大约 450 万行数据，上传数据的成本不到 1 美元。

![](img/05c8e8ce9c3b29a0aae7ddd1159d4461.png)

azureprice 摄取管道

## 复制和点击房屋细节

正如您在上面演示的过程中所看到的，可能会有许多重复项，因为在插入数据时，由于外部限制和重新排队，一些函数可能会失败。

当我使用 CosmosDB 存储数据时，我通过主键对数据库进行了 **upserts** ，重复不是问题，因为我有一个非常简单的主键，由 VM 名称、区域和一些其他字段组成。数据库做了所有删除和插入重复项的脏活。

不幸的是，ClickHouse 没有我们在其他数据库中使用的主键。ClickHouse 中的主键更像是一个索引，这是一个有点令人困惑的名称。

官方文档中有更多关于这个的细节。更复杂的是，它也没有**上插**。

因此，Clickhouse 可以接受具有相同主键值的行，这就是主要挑战所在。

我有一个管道，每天可以被触发多次，如果其中一个任务失败并重新启动，还可以插入副本。我会得到这样的东西:

![](img/59c73a9baa7cc3ea5e34905c38c413d9.png)

我有一堆描述我的数据的字段和一个名为 _upload 的特殊列。该列是 DDMMYYYY 格式的分区键，它帮助我限制查询扫描的范围，并只对部分数据进行优化。

不同的文章描述了用去重复来处理这种情况的方法:

*   [https://double . cloud/blog/posts/2022/11/de duplication-in-click house-a-practical-approach](https://double.cloud/blog/posts/2022/11/deduplication-in-clickhouse-a-practical-approach)
*   [https://www . tiny bird . co/docs/guides/de duplication-strategies . html](https://www.tinybird.co/docs/guides/deduplication-strategies.html)

不幸的是，默认推荐的**替换合并树**对我来说不是一个选项，因为替换或折叠副本最终会不可预测地发生。我需要几乎没有重复的即时数据。在查询中只使用 **Distinct** 操作符可能会影响性能，特别是因为计划只存储一年的数据历史，这大约是 360 x 4.5M = **1.6B 行。在这种情况下，每次上传数据，性能都会开始下降。**

## **摄取期间处理副本**

**首先**，我需要优化最频繁的查询，以获得 azureprice.net 第一屏的数据。这是最后一天的定价(当前价格)的虚拟机列表，没有重复。

我为每天的表添加了一个分区，这将有助于我将选择查询的扫描过程缩小到一个特定的分区(1 天的数据)，而不会对未来产生任何影响。

```
CREATE TABLE default.price
(
  `name` LowCardinality(String),
  `CPUdesc` LowCardinality(String),
  `CpuArchitecture` LowCardinality(String),
  …
  `bestPriceRegion` String,
  `bestSecondPriceRegion` String,
  `modifiedDate` DateTime,
  `_upload` Int32
)
ENGINE = MergeTree
PARTITION BY _upload
ORDER BY (regionId,  Currency, name, tier, paymentType)
```

**其次**，分区将有助于跨天复制，但如果其中一个任务失败，并在同一分区内为该相似天创建另一个相似行，它将无济于事。

为此，我创建了一个调度任务，使用**最终重复数据删除**操作符对最后一个分区执行 [**优化**](https://clickhouse.com/docs/en/sql-reference/statements/optimize/) 。该运算符强制折叠具有相同主键的行。为此，我首先得到最后一个分区:

```
SELECT 
  DISTINCT "partition" 
FROM 
  system.parts 
WHERE 
  table ='price' and database=currentDatabase() 
ORDER BY partition_id desc LIMIT
```

使用它来仅在该分区上执行**优化**会显著减少执行时间，因为它将仅适用于最后一次每日上传，并且将始终适用于 4.5M 的行。

```
OPTIMIZE table price PARTITION ID '%last_part%' FINAL DEDUPLICATE BY name, Currency, regionId, tier, paymentType, _upload
```

**作为最后一步**，我运行常规**优化**来改进文件结构并强制合并部分:

```
OPTIMIZE table price PARTITION ID '%last_part%' FINAL
```

## **在选择期间处理重复(第二道防线)**

上面的过程看起来不错，但是如果用户在大约一个小时的摄取过程中访问了一个网站呢？他只能看到部分数据，可能还有重复的数据，这在这里是不可行的。

为了处理这种情况，我们只需要在数据已经准备好并消除重复的情况下工作。为了实现这一点，我在从当前日期起的第 **-1 天**获取数据，或者更准确地说，在数据优化、完全上传和消除重复后的第 **-1 个分区**返回。以下是一个选择查询的示例:

```
SELECT 
  * 
FROM 
  price as c 
WHERE 
  c.Currency = {Currency : String} 
  AND c.regionId = {Region : String} 
  AND c.tier = {Tier : String} 
  AND c.paymentType = {Type : String} 
  AND _partition_value.1 = (
    SELECT 
      DISTINCT "partition" 
    FROM 
      system.parts 
    WHERE table ='price' and database=currentDatabase() 
    ORDER BY partition_id desc LIMIT 1 OFFSET 1
```

具体来说，下面的部分使用**偏移**和**限制**运算符从系统点击室表中选择前一个分区，并在 **WHERE** 子句中使用。

```
SELECT 
  DISTINCT "partition"
FROM   
  system.parts
WHERE  
  table = 'price' AND DATABASE = Currentdatabase()
ORDER  BY partition_id DESC LIMIT  1 offset 1
```

这个小东西帮助我总是只处理一个分区的数据，消除了全表扫描，避免了摄取过程中部分数据的结果。

我在我的复合主键上添加了 **DISTINCT** 来完成并确保在任何情况下都不会出现重复。

**DISTINCT** 将始终在可预测的时间工作，因为它将始终只在一个分区内工作，并且不会影响查询性能。

```
SELECT 
  DISTINCT ON (NAME, regionid,...) *
FROM   
  'price' AS c
```

## **总结**

速度永远是某种东西的交易；在 ClickHouse 中，这是支持数据一致性的能力。从好的方面来看，通过增加逻辑和调整数据模式，这些事情是可以管理的。最终，它没有影响我的速度。我从应用程序到 Clickhouse **的所有查询都有大约 10ms** ，并且没有降级或与数据量或数据增长相关。