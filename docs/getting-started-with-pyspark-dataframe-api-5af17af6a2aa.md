# PySpark DataFrame API 入门

> 原文：<https://blog.devgenius.io/getting-started-with-pyspark-dataframe-api-5af17af6a2aa?source=collection_archive---------5----------------------->

**PySpark、Python、Jupyter 笔记本**

![](img/b6a4d6d4b74002188e6c0ace9e4c8b5b.png)

火花数据帧 API

我们已经介绍过 PySpark 设置。现在是时候探索它的特性了，它将帮助我们进行数据工程！编写我们自己的程序来执行数据转换任务可能相当具有挑战性。但是，Spark 有两个特性可以让您利用现有的技能来执行与数据相关的任务。我们已经谈到了这两个特性:DataFrame API 和 Spark SQL。我们可以利用这两者之一来清理、转换和塑造我们的数据。我们将讨论各种数据框架 API 操作，它们将帮助我们进行数据转换。上一篇文章的链接是[这里](/setup-pyspark-locally-build-your-first-etl-pipeline-with-pyspark-91c3060c6133)。

完整的代码可在 [GitHub](https://github.com/hnawaz007/pythondataanalysis/tree/main/PySpark) 上获得。视频教程可以在 YouTube 上找到。

PySpark DataFrame API 使我们能够:

*   从各种来源读取数据，如平面文件、拼花、数据库和 API。
*   使用数据框架轻松执行数据转换
*   执行选择列、筛选、连接、聚合功能

**大数据背景下的火花**

让我们解释一下为什么 PySpark 在当前的数据领域很重要。我们都知道并且喜欢 Pandas 在数据操作方面的灵活性和多功能性。然而，由于数据的庞大，我们会遇到内存问题。然而，这不是熊猫的问题，而是运行它的机器的限制。如果我们的数据量大于机器的 ram，我们可以最大化单台机器上的 ram。这将引发内存错误。为了解决这个问题，我们可以使用块大小来批量处理数据，或者在将数据加载到 Pandas 之前简单地过滤数据。这是我在熊猫系列节目中经常被问到的问题之一。现在我们在 Spark 上有了 Pandas API，这是一个为处理大量数据而设计的分布式引擎。它提供了超越单台机器的可扩展性。Spark 上的 pandas API 可以很好地扩展到大型节点集群。给你一些背景资料，Databricks 有一个案例研究。Spark 集群能够在几秒钟内处理和执行 15TB 拼花数据集上的各种数据相关任务。

**数据框架 API 操作**

让我们打开笔记本，从 PySpark DataFrame API 开始。像往常一样，我们将在顶部导入所需的库。我们将 Java_home 变量设置为 Java 安装目录。这是 spark 所需要的。接下来，我们定义 spark 配置细节并启动 Spark 会话。这在[上一节](/setup-pyspark-locally-build-your-first-etl-pipeline-with-pyspark-91c3060c6133)中有详细介绍。

让我们从 SQL 数据库中读取数据。我们定义了数据库细节和凭证。使用这些信息，我们构建一个 jdbc url。

```
database **=** "AdventureWorksDW2019"
table **=** "DimProduct"
password **=** os**.**environ['PGPASS']
user **=** os**.**environ['PGUID']
schema  **=** "dbo"
jdbc_url **=** f'jdbc:sqlserver://localhost:1433;database={database};encrypt=true;trustServerCertificate=true;'
```

我们调用 spark read 函数来导入数据。我们指定数据源是 jdbc 以及额外的选项，如 url、表、凭证和驱动程序的名称。这会将数据加载到数据帧中。现在我们有了一个数据框架，我们将能够运行数据框架 API 操作，以及针对它的 SQL 查询。

```
df **=** spark**.**read \
    **.**format("jdbc") \
    **.**option("url", jdbc_url) \
    **.**option("dbtable", f"{schema}.{table}") \
    **.**option("user", user) \
    **.**option("password", password) \
    **.**option("driver", "com.microsoft.sqlserver.jdbc.SQLServerDriver") \
    **.**load()
```

我们可以通过调用 show()方法来查看数据帧。默认情况下显示前 20 行。为了更好的可读性，我们也可以预览为熊猫数据帧。

```
df**.**show()
# Convert to Pandas for better readability
df**.**limit(10)**.**toPandas()
```

Dataframe API 允许我们轻松地执行各种操作。让我们来看几个例子。我们可以用 ColumnRename 函数重命名列*。它接受旧的和新的列名。我们可以用 count 函数检查数据帧包含多少行。我们可以通过选择从数据框架中选择列的子集。*

```
*# Rename column*
df **=** df**.**withColumnRenamed("EnglishProductName","ProductName")
df**.**show()
*# Row count*
df**.**count(
*# Select subset of columns*
df **=** df**.**select("ProductKey", "ProductName", "Color")
df**.**show())
```

我们可以使用排序函数对数据集进行排序，并提供我们想要排序的列。默认情况下，排序顺序是升序。为了颠倒排序顺序，我们需要将函数从 spark sql 作为 f 导入。然后我们可以调用排序，并向函数提供关键字 desc 和列名。这将我们的数据集按降序排序。

```
# Ascending Sort
df**.**sort("ProductName")**.**show()
# Descending Sort
**from** pyspark.sql **import** functions **as** F
df**.**sort(F**.**desc("ProductName"))**.**show()
```

接下来我们来看看过滤器。我们导入的数据将包含可能与分析无关的附加信息。我们可以用过滤器过滤掉不必要的数据。我们调用 select 函数并向它提供我们需要的列。在这一点上，我们可以链接过滤器。在过滤函数中，我们提供列名和值。在这种情况下，我们希望查看 ProductKey 等于 22 的记录。让我们用 show 预览一下数据。如果我们的列不包含任何空格，我们也可以编写不带引号的列。过滤功能也支持通配符操作。我们可以选择名称中包含头盔的产品。这将返回所有带有头盔字的产品。

```
# Filter records with filter
df**.**select("ProductKey", "ProductName")**.**filter("ProductKey = 22")**.**show()
# We can also select columns without Quotes
df**.**select(df**.**ProductKey, df**.**ProductName)**.**filter("ProductKey = 22")**.**show()
# Filter records with wildcard
df**.**select("ProductKey", "ProductName")**.**filter("ProductName like '%helmet%'")**.**show()
```

我们可以提供多种过滤条件。每个条件必须放在括号中，我们可以用& OR 子句分隔这些条件。这里我们指定我们想要包含单词头盔的产品的记录，并且我们只想要黑色的产品。

```
# Filter with multiple conditions
df**.**filter((df**.**ProductName**.**like('%helmet%')) **&** (df**.**Color**==**'Black'))**.**show()
```

如果您更喜欢 SQL 并希望在 SQL 中执行过滤操作，那么只需创建一个基于数据帧的视图。一旦我们有了一个视图，我们就可以使用 Spark SQL 编写 SQL。我们可以在 where 子句中提供我们的过滤条件，并过滤出相关的数据。

```
*# Create a view*
df**.**createOrReplaceTempView("Product")
spark**.**sql("select count(1) from Product")**.**show()
# Filter data using SQL
spark**.**sql("select ProductKey, ProductName from Product where ProductKey = 22")**.**show()
# Filter data using wildcard
spark**.**sql("select ProductKey, ProductName from Product where ProductName like '%helmet%'")**.**show()
```

接下来，我们将导入一个新的数据集。这包含我们产品的交易。用数据库术语来说，这是一个事实表。这有更多的行，因为我们产品的所有销售交易都记录在这里。我们将从该数据帧创建一个视图，并将缓存该视图以获得更好的性能。

```
*#Let's get the fact table with product sales transactions*
sales **=** spark**.**read \
    **.**format("jdbc") \
    **.**option("url", jdbc_url) \
    **.**option("dbtable", f"dbo.FactInternetSales") \
    **.**option("user", user) \
    **.**option("password", password) \
    **.**option("driver", "com.microsoft.sqlserver.jdbc.SQLServerDriver") \
    **.**load()
# Create a view based on DataFrame
sales**.**createOrReplaceTempView("sales")
sales**.**cache()
```

为了得到我们产品的销售数字，我们需要把这两个视图结合起来。同样，我们可以使用 dataframe API 或 SQL 来执行此操作。我们可以在 Spark SQL 中发出以下 SQL 查询。在这里，我们选择产品名称和销售额。我们在 ProductKey 列中加入了产品和销售视图。我们正在筛选包含关键字头盔的产品记录。因为我们对销售额使用了聚合函数 SUM，所以我们需要按产品名称对其进行分组。我们根据销售额对查询进行降序排序。这将显示产品及其各自的销售数字。

```
spark**.**sql("""
SELECT 
    p.ProductName,
    SUM(s.SalesAmount) AS SalesAmount
FROM  Product p
    Inner join sales s on p.ProductKey = s.ProductKey
where ProductName like '%helmet%'
Group by 
    p.ProductName
order by 
    SUM(s.SalesAmount) desc"""
)**.**show()
```

我们可以用 dataframe 执行连接操作。我们在 ProductKey 列上连接 sales 和 df 数据帧，并且我们只希望这两个数据帧中的匹配行带有关键字 Inner。

```
salesjoined **=** sales**.**join(df, ['ProductKey'],how**=**'inner')
salesjoined**.**limit(10)**.**toPandas()
```

类似地，我们可以使用 group by 函数对连接的数据帧执行聚合。我们按照产品和颜色对数据集进行分组。聚合函数是 sum，我们将它应用于销售额。假设我们想要查看每种产品和颜色的最大销售额。我们最终将得到两个名称为销售额列。我们可以使用 Alias 函数来重命名列。

```
salesjoined**.**groupBy(["ProductName","Color"])**.**agg(
    F**.**sum("SalesAmount")**.**alias("TotalSalesAmounted"),\
    F**.**max("SalesAmount")**.**alias("MaxSalesAmount")\
    )**.**show()
```

大多数情况下，我们会根据数据集中现有的列来派生新的列进行分析。我们可以通过各种方式实现这一目标。首先，我们用 Column 函数计算净销售额列*。我们提供新的列名，然后将这些列提供给 F 点 col 函数。我们导入上面 F 的火花函数。我们从销售额中减去税额，得到净销售额列。*

```
saleswithNet **=** salesjoined**.**withColumn("NetSales", F**.**col("SalesAmount") **-** F**.**col("TaxAmt"))
saleswithNet**.**limit(10)**.**toPandas()
```

我们还基于条件创建了一个列。我们可以像 construct 一样使用 SQL Case 语句。我们用 Column 函数调用*,并提供新的列名。当列值等于值时，我们为它提供一个值。在本例中，我们正在创建区域列。当销售区域关键字为 7 时，区域应为欧洲。我们指定多个 when 条件。如果该值不满足指定的条件，则属于“否则”。我们为美国设定了另外一个标准。让我们预览一下数据帧，在最后我们看到了区域列。我们可以检查该列中的不同值来验证我们的逻辑。*

```
*#Create a new column based on a condition*
saleswithNet **=** saleswithNet**.**withColumn(
    'Region',
    F**.**when((F**.**col("SalesTerritoryKey") **==** 7), "Europe")\
    **.**when((F**.**col("SalesTerritoryKey") **==** 8) , "Europe")\
    **.**when((F**.**col("SalesTerritoryKey") **==** 9) , "Pacific")\
    **.**when((F**.**col("SalesTerritoryKey") **==** 10) , "Europe")\
    **.**otherwise("Americas")
)
saleswithNet**.**limit(10)**.**toPandas()
*#Check distinct values*
saleswithNet**.**select('Region')**.**distinct()**.**collect()
```

最后，我们将从列中删除空值。CarrieTrackingNumber 列为空。我们使用带有列的*函数和类似的结构。我们检查该列是否指定了一个值，否则，将其设置为一个现有值。这将用我们提供的值替换空值或无值。*

```
*#Replace null with 0*
**from** pyspark.sql.functions **import** when, lit
saleswithNet **=** saleswithNet. WithColumn('CarrierTrackingNumber', when(saleswithNet**.**CarrierTrackingNumber**.**isNull(), 
lit('0'))**.**otherwise(saleswithNet**.**CarrierTrackingNumber))
saleswithNet**.**limit(10)**.**toPandas()
```

在某些情况下，我们可能希望删除一列。我们可以使用 drop 函数。我们向它提供想要删除的列。这将从我们的数据框架中删除该列。

```
*# Drop a columns*
saleswithNet**=**saleswithNet**.**drop("CustomerPONumber")
saleswithNet**.**limit(10)**.**toPandas()
```

这些是可用于数据转换的一些常见的 Dataframe API 操作。如果你想进一步探索这些，那么查看一下 [PySpark 文档](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.DataFrame.html)。

**结论**

*   我们描述 PySpark DataFrame 是什么，以及如何使用这个 API 的各种特性。
*   我们展示了使用 PySpark DataFrame API 导入和操作数据是多么容易。
*   我们从 SQL 数据库中读取数据，并使用 PySpark 对其进行操作。
*   完整的代码可以在[这里](https://github.com/hnawaz007/pythondataanalysis/blob/main/PySpark/PySpark%20Session%202.ipynb)找到