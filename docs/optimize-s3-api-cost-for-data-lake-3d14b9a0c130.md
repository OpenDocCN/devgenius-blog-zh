# 优化数据湖的 S3 API 成本

> 原文：<https://blog.devgenius.io/optimize-s3-api-cost-for-data-lake-3d14b9a0c130?source=collection_archive---------3----------------------->

在之前的[文章](https://medium.com/@faisal-22508/analysis-of-s3-api-cost-for-data-lake-3b0fc4b7c19c)中，我们讨论了如何识别导致高 S3 API 成本的 S3 数据集。一旦我们有了表的列表，下一步就是看看我们是否可以降低与这些表相关的成本。

## 如何计算 S3 API 成本

在讨论 API 成本时，要记住的最重要的事情是 *API 调用依赖于添加或检索的对象数量，而与对象大小无关。*这意味着大量的小对象会增加 Get 和 Put API 调用的成本。

为了降低成本，我们需要减少使用 Athena 或任何其他查询引擎从数据湖中读取表时要扫描的对象数量。可以通过两种方式完成:*分区*和*分桶。*我们将讨论这两个问题，但是***是对我很有效的技巧。***

## *分割*

*分区是一个众所周知的话题——对这个话题的详细讨论超出了本文的范围。您可以在此阅读如何为数据湖对 S3 数据进行分区— [AWS 文档](https://docs.aws.amazon.com/athena/latest/ug/partitions.html)*

*要对现有的未分区表进行分区，可以使用 Athena 的 *CTAS* 命令— [AWS doc](https://docs.aws.amazon.com/athena/latest/ug/ctas-examples.html#ctas-example-partitioned)*

*如果表有超过 100 个分区，您可以使用*INSERT INTO—*[AWS doc](https://docs.aws.amazon.com/athena/latest/ug/ctas-insert-into.html)*

## *确认查询中使用了分区*

*如果分区表的 API 开销很高，首先要做的是确保查询中实际使用了分区:*

*   *使用 *SHOW PARTITIONS table_name 确认分区元数据已更新。它显示所有列出的分区**
*   *确保扫描数据时使用分区键。这一点非常重要，因为在很多边缘情况下，分区键会被忽略。例如，如果分区名在**子查询**的`WHERE`子句中，Athena 当前不会过滤分区。Athena 中的 EXPLAIN 命令可用于确认分区键的使用*

```
*-- Table is partitioned by (year,month,day,hour)
explain select device_id from ticket 
where year = '2022' and month ='11' and day = '6' and hour = '13'-- Result
Query Plan
Fragment 0 [SOURCE]
    Output layout: [device_id]
    Output partitioning: SINGLE []
    Output[columnNames = [device_id]]
    │   Layout: [device_id:varchar]
    │   Estimates: 
    └─ TableScan[table = awsdatacatalog:<athena_db_name>:ticket]
           Layout: [device_id:varchar]
           Estimates: 
           device_id := device_id:string:REGULAR
           year:string:PARTITION_KEY
               :: [[2022]]
           day:string:PARTITION_KEY
               :: [[6]]
           month:string:PARTITION_KEY
               :: [[11]]
           hour:string:PARTITION_KEY
               :: [[13]]*
```

*命令输出将清楚地显示查询执行中将使用哪些分区键和值。这可以用于 Athena 上的任何 SQL，如果没有如上所示的分区键，或者如果键具有所有可能的值，这意味着没有使用键，将发生全表扫描。*

*如果在执行查询时没有使用分区键，如果可能的话尝试改变 SQL，或者也许*分桶*是最好的解决方案，这是下一个主题。*

## *存储基础知识*

*分桶是一种将基于特定列的数据分组到单个分区中的技术。这些列被称为*桶键*。创建表格时，我们可以指定表格在 S3 路径中应该包含的文件数量。随着文件数量的急剧减少，S3 API 的成本也以同样的比例减少。这是解决巨额 API 成本的最有效方法。*

*Athena 支持 CTAS 命令来创建分桶表。您可以使用基数高的列作为*桶键**

```
*CREATE TABLE merged_tables.ticket
WITH (
format = 'PARQUET',
external_location = 's3://de-experements/databases/merged_tables/ticket',
bucketed_by = ARRAY['device_id'],
bucket_count = 10)
AS SELECT *
FROM raw_tables.ticket;
-- target table will have exactly 10 files in S3\. If source table has 10K files and did full table scan, we will see API count reduction of ~1000X for this data set*
```

## *确认分桶表更划算*

*现在我们知道了如何使用 SQL 创建分桶表。确保分桶表更具成本效益的最佳方法是在 SQL 的回填中使用分桶表。*

*假设您从分析师那里获得了新的 SQL。他们希望回填最近三年的目标表，这意味着您必须运行 SQL 数百次(如果 SQL 是针对一天的，并且要追加结果)*

*   *为每个源表创建分桶表。对分时段表位置使用单独的 S3 时段。*
*   *将 SQL 指向分桶表，并从调度器运行回填 SQL(我们使用气流)*
*   *回填完成后，检查 S3 桶中用于分桶表的 S3 API 的成本是多少。*

*如果没有回填，您可以创建分桶表，并在这些表上重复运行一些日常 SQL，比较 API 成本。*

## *如何存储现有表*

*在大多数情况下，数据作为单个文件从批处理或流管道写入 S3，不可能在每次写入 S3 时重新创建分桶表。*

*这里有一个简单的使用策略。这里我们有一个`*raw_table*`和一个对应的`*bucketed_table*` *(从`*raw_table*` *)* 创建的**

*   *我们将保持`*raw_table*`**完好无损。写至`raw_table`的 S3 位置将照常发生。***
*   **`*bucketed_table*` 每周(或者你决定的任何频率)从 *raw_table* 中创建。该表创建后，任何写入`*raw_table’s*` S3 位置的文件也会被复制到`*bucketed_table’s*` S3 位置*。***
*   **因此`*bucketed_table*` *将在最近 N 天内拥有分桶文件和附加文件。***
*   **下周，我们将删除 `*bucketed_table*` ，清理 S3 位置，并从`*raw_table*`T7 重新创建它。— N 现在*分桶 _ 表*只有分桶文件。**
*   **如果`*raw_table*` 被分区，则表中会有额外的列可用。因此，当在 *bucketed_table* 中写入单个文件时，请确保将这些列添加到文件中**

**让所有的 SQL 都在`*bucketed_table*` 和上运行，你会看到 S3 API 成本的大幅降低，而且大部分查询都会快得多。**

## **结论**

**表格分桶是降低 S3 API 成本的一项非常有用的技术，无需太多努力即可在生产管道中实施。**