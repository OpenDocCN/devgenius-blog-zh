# PySpark —设置三角洲湖

> 原文：<https://blog.devgenius.io/pyspark-setup-delta-lake-971e2e37330d?source=collection_archive---------3----------------------->

三角洲湖是目前围绕大数据领域的热门话题之一。它在数据湖的可靠性方面带来了很多改进。但是为什么它会获得如此多的关注呢？

![](img/403d163379a214973c07b5e59c5c3ab7.png)

代表图像(鸣谢:delta.io)

根据 Delta IO 文档—“*Delta Lake 是一个开源项目，支持在数据湖之上构建湖屋架构。Delta Lake 提供 ACID 事务、可扩展的元数据处理，并在现有数据湖(如 S3、ADLS、GCS 和 HDFS* )的基础上统一流式和批量数据处理。

有这么多与三角洲湖相关的重要特征，查看下面的图片

![](img/bc28d27f1832c24c83ce8a1a1bd25f60.png)

主要功能(学分:delta.io)

今天，我们将使用 PySpark 设置 delta lake，并为我们的 metastore 启用相同的功能。

为了让 PySpark 支持 Delta Lake，我们需要将 delta-core jar 导入 SparkSession 并正确设置配置。

```
**# Create Spark Session with Delta JARS and conf**

from pyspark.sql import SparkSession

spark = SparkSession \
    .builder \
    .appName("Delta with PySpark") \
    .config('spark.jars.packages', 'io.delta:delta-core_2.12:2.1.1') \
    .config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension") \
    .config(
        "spark.sql.catalog.spark_catalog",
        "org.apache.spark.sql.delta.catalog.DeltaCatalog",
    ) \
    .config("spark.sql.warehouse.dir", "spark-warehouse") \
    .master("local[*]") \
    .enableHiveSupport() \
    .getOrCreate()

spark
```

![](img/c4d8963748fe14156b3de9ad5a2a2b88.png)

生成 SparkSession

> 确保启用持久化 metastore。查看我以前的博客，在同一个——[https://urlit . me/blog/py spark-implementing-persisting-metastore/](https://urlit.me/blog/pyspark-implementing-persisting-metastore/)

现在，我们准备与三角洲湖合作。为了创建我们的第一个增量表，让我们读取销售拼花数据。

```
**# Lets read our Sales dataset**

df_sales = spark.read.parquet("dataset/sales.parquet/*parquet")
df_sales.printSchema()
df_sales.show(10, False)
```

![](img/841d2fd4ef3e5381f34f4b11e0d894ff.png)

销售数据集

现在，我们将这些数据转换并写成增量表。

```
**# Lets create a sales managed delta table**
from pyspark.sql.functions import to_timestamp, expr

df_formatted = (
    df_sales
    .withColumn("transacted_at", to_timestamp("transacted_at"))
    .withColumn("amount", expr("CAST(amount as decimal(14,2))"))
               )

df_formatted.write \
    .format("delta") \
    .mode("overwrite") \
    .option("mergeSchema", True) \
    .saveAsTable("sales_delta_managed")
```

![](img/72fd63872131c06719a30b600d0f60d9.png)

销售增量表

由于我们没有指定任何外部位置，默认情况下，它将是一个内部管理的增量表。

![](img/bf35e8023d14c2a0dbda6ce39c3b9fef.png)

表格定义

![](img/fd5ee8d9bba88e8ec6a37aedb401019d.png)

列表数据

因为，增量表支持版本控制

```
**# Lets check the current version of the table**

from delta import DeltaTable

dt = DeltaTable.forName(spark, "sales_delta_managed")
dt.history().select("version", "timestamp").show(truncate=False)
```

![](img/d7960b21be03a44425da7a7d21c0130a.png)

增量表版本控制

让我们更新一个记录来查看版本控制中的变化

![](img/55027581f13bb9f7fcab015675cf4aad.png)

ACID 事务支持

记录更新没有任何麻烦(*三角洲湖支持 DML 操作*)

![](img/ccec2aeb78cb6ae03a5f698db5dcee71.png)

记录已更新

版本控制的变化

![](img/9a2f5bb7d29b35a189aa4c58fd51d398.png)

DML 操作后版本更新

让我们验证给定的表位置是否是增量表

```
**# Verify if a given table is Delta**

print(DeltaTable.isDeltaTable(spark, "spark-warehouse/sales_managed/"))
print(DeltaTable.isDeltaTable(spark, "spark-warehouse/sales_delta_managed/"))
```

![](img/319db323974cf811e3a200af3e3e8467.png)

验证增量表

**奖励:将拼花数据转换为 delta 的快捷方式**

```
**# Shortcut to create a Parquet location to delta table
# We will convert the sales_managed table to delta**

DeltaTable.convertToDelta(spark, "parquet.`spark-warehouse/sales_managed`")
```

验证位置是否转换为增量

![](img/2d2a3f993533a83603cc29a8a3d21517.png)

确认

但是，如果我们检查 Catalog 中的元数据，它仍然是一个 hive 表。

![](img/28658648159462a3a4435b4af5436db1.png)

所以，也要转换元数据

```
%%sparksql

CONVERT TO DELTA default.sales_managed;
```

![](img/21c3d038cab0adf09b890eaedabb8c4e.png)

转换为增量的表格

我们将在接下来的博客中涉及更多的三角洲湖概念。

在 Github 上查看 iPython 笔记本—[https://Github . com/subhamkharwal/ease-with-Apache-spark/blob/master/27 _ delta _ with _ py spark . ipynb](https://github.com/subhamkharwal/ease-with-apache-spark/blob/master/27_delta_with_pyspark.ipynb)

查看我的个人博客—[https://urlit.me/blog/](https://urlit.me/blog/)

查看 PySpark Medium 系列—[https://subhamkharwal . Medium . com/learn bigdata 101-spark-Series-940160 ff4d 30](https://subhamkharwal.medium.com/learnbigdata101-spark-series-940160ff4d30)