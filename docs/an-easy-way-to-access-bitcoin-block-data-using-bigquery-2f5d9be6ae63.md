# 使用 BigQuery 访问比特币块数据的简单方法

> 原文：<https://blog.devgenius.io/an-easy-way-to-access-bitcoin-block-data-using-bigquery-2f5d9be6ae63?source=collection_archive---------7----------------------->

比特币区块链包括一个不断增长的记录列表，称为包含交易的块。块按反向顺序链接，每个块都引用链中的前一个块。在采矿发生或比特币交易发生后，会创建新的区块。

区块链的第一个区块叫做创世区块，创建于 2009 年 1 月 3 日。它是所有块都可以追溯到的共同祖先。数据块存储在节点上。所有节点都相互连接，并且每个节点从一开始就有一个完整的区块链副本。随着新块的确认和添加，区块链的本地副本被更新。

在本文中，我们将使用 Google BigQuery 查询比特币区块链的块高、挖掘的块、总块大小和平均块间隔。查询结果可以在[比特币仪表盘](https://trigolabs.io/bitcoin/)上查看。确保选择“选择类别”作为网络，并选择“网络”作为区块高度、开采的区块、区块大小或区块间隔。

![](img/e3cb00b0dc10bf2a3d03648b2db406f9.png)

# 开始前的假设

*   你需要有一个谷歌云帐户。遵循[big query](https://medium.com/dev-genius/getting-started-with-google-bigquery-5e076e77d43c)入门中的步骤，熟悉该平台。
*   此外，您可以使用 BigQuery 平台[在这里](https://medium.com/@vivbellavita/explore-cryptocurrency-public-datasets-with-bigquery-1d9562691580)探索加密货币公共数据集。

# 区块高度

区块高度是指区块链已开采的区块数量。

该查询创建了一个表`miner_block_height`,以检索每天开采的最高区块的日期和区块高度

*   `MAX(number)` —返回每天的最大块数。

最终输出将显示块高度，按日期升序排序。

```
WITH miner_block_height AS (
    SELECT DATE(timestamp) AS date,  MAX(number) AS block_height 
    FROM `bigquery-public-data.crypto_bitcoin.blocks`
    WHERE DATE(timestamp) <= DATE_SUB(CURRENT_DATE(), INTERVAL 1 DAY) 
    GROUP BY date
)

SELECT * FROM miner_block_height
ORDER BY date
```

# 开采的区块

开采的区块数是指一天内区块链中开采和包括的区块数。

该查询创建了一个表`miner_blocks`,用于提取每天开采的区块的日期和数量

*   `COUNT(DISTNCT number)` —统计每天不同的块数

最终输出将显示每天开采的区块数量，按日期升序排序。

```
WITH miner_blocks AS (
    SELECT DATE(timestamp) AS date,  COUNT (DISTINCT number) AS blocks_mined 
    FROM `bigquery-public-data.crypto_bitcoin.blocks`
    WHERE DATE(timestamp) <= DATE_SUB(CURRENT_DATE(), INTERVAL 1 DAY)
    GROUP BY date
)

SELECT * FROM miner_blocks
ORDER BY date
```

# 块大小

块大小是指一天内创建的所有块的总大小(以字节为单位)。

这个查询创建了一个表`miner_block_size`，它使用`SUM`函数汇总了每个日期所有块的大小。

最终输出将显示每天的总块大小，按日期升序排序。

```
WITH miner_block_size AS (
    SELECT DATE(timestamp) AS date, SUM(size)
    FROM `bigquery-public-data.crypto_bitcoin.blocks`
    WHERE DATE(timestamp) <= DATE_SUB(CURRENT_DATE(), INTERVAL 1 DAY) 
    GROUP BY date
)

SELECT * FROM miner_block_size
ORDER BY date
```

# 闭塞区间

区块间隔是指开采区块之间的平均时间(秒)。

首先，我们创建一个公共表表达式(CTE) `block_rows`，它根据字段`timestamp`对块进行排序，并根据顺序为每个块分配一个`row_num`。

接下来，我们创建一个 CTE `miner_block_interval`，基于`row_num`字段将`block_rows`表与其自身连接，并计算每个块的`timestamp`字段与前一个块的`timestamp`字段之间的时间差。这个时间差就是开采区块之间的时间间隔。结果按`timestamp`字段以升序排序。

第三个 CTE `block_interval_by_date`使用`AVG(delta_block_time)`函数计算每天的平均块间隔。

最终输出将显示每天的平均块间隔，按日期升序排序。

完整的查询如下所示。

```
WITH block_rows AS (
  SELECT *, ROW_NUMBER() OVER (ORDER BY timestamp) AS row_num
  FROM `bigquery-public-data.crypto_bitcoin.blocks` 
), 

miner_block_interval AS (
    SELECT row_a.timestamp AS block_time, row_a.number AS block_number,
    TIMESTAMP_DIFF(row_a.timestamp,row_b.timestamp, SECOND) AS delta_block_time
    FROM block_rows row_b
    JOIN block_rows row_a
    ON row_b.row_num = row_a.row_num-1
    ORDER BY row_a.timestamp
),

block_interval_by_date AS (
  SELECT DATE(block_time) AS date, AVG(delta_block_time) AS mean_block_interval
  FROM miner_block_interval
  WHERE block_number > 1
  GROUP BY date
)

SELECT * FROM block_interval_by_date
ORDER BY date
```

# 结论

我们已经看到，使用 Google BigQuery 来检索比特币区块链上的块数据相对容易。通过利用谷歌云强大的数据仓库和查询工具，用户可以对比特币网络进行有价值的洞察，即块高度、挖掘的块、总块大小和平均块间隔，如此处所示。总体而言，使用 BigQuery 轻松访问比特币块数据的能力是一项重大发展，有可能推动加密货币领域的创新和增长。

**也读作:**

*   [在 BigQuery 中查询比特币区块链活跃地址](https://medium.com/dev-genius/query-bitcoin-blockchain-for-active-addresses-in-bigquery-be440f362dd9)
*   [使用 BigQuery 查询比特币余额指南](https://medium.com/coinmonks/guide-to-query-wallet-balances-of-bitcoin-in-bigquery-a4b52ec2466a)
*   [持有比特币的储备证明——如何在 BigQuery 中计算](https://medium.com/coinmonks/proof-of-reserves-for-bitcoin-holdings-how-to-compute-in-bigquery-6f6c5bc8648d)

***感谢阅读！***

如果你喜欢这篇文章，并想了解更多，请关注我。我定期发布与链上分析、机器学习和 BigQuery 相关的主题。我尽量让我的文章简单而精确，尽可能提供代码、例子和模拟。