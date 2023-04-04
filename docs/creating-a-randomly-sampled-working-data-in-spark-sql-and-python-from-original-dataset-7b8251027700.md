# 从原始数据集在 Spark 和 Python 中创建随机采样的工作数据

> 原文：<https://blog.devgenius.io/creating-a-randomly-sampled-working-data-in-spark-sql-and-python-from-original-dataset-7b8251027700?source=collection_archive---------5----------------------->

了解如何从大型数据集创建具有统计显著性的样本，以加速迭代开发，消除 ML 训练中的偏差等。

![](img/1f72970550a995c1d955bc5ed742bf78.png)

[https://www.freepik.com/photos/food'](’<a)freepik 创作的美食照片—[www.freepik.com](http://www.freepik.com)/a>

# 为什么是 PySpark

使用 python 停放已经成为数据工程系统的一种非常流行的方法。它结合了所有东西的精华:Spark 用于更快的内存处理(当然，假设你有内存)；以及有用的 python 库的普遍存在和精通该语言的开发人员的可用性。将它与 SQL 结合起来，SQL 是数据处理的事实语言，如分析、清理、争论等等，你就有了一个像样的数据工程系统。

# 大数据集的挑战

在开发应用程序时，您经常需要使用组织中的真实数据集；不是虚构的或者不相关的，你在学习时会用到的。但是有一个小问题:真实的数据集通常非常大，当你编写程序时，你想要一个非常小的子集来加快开发过程。也许您正在一个内存更少的更小的服务器、一个云容器，甚至是内存非常有限的笔记本电脑上开发它。一个完整的数据集可能会导致内存不足的错误，尤其是在执行像 *collect* ()这样的操作时，或者至少会减慢您的迭代代码开发。

你能做什么？您可以使用简单的 Unix 工具(如`head`)获取实际数据文件的一部分；但这将使你的数据可疑地失真。文件的前 *n* 行可能属于一个客户；因此，当您对文件进行操作以执行识别分布模式或训练 ML 模型等任务时，它将导致糟糕的开发、无效的模型并掩盖任何潜在的错误。您需要该文件的一个较小的子集；但是必须从原始文件中随机选择，即必须是*统计上*显著的样本。在本文中，您将学习如何从大文件中创建数据样本，以表示实际数据的特定百分比；而是随机选择的。您还将学习如何根据实际分布来影响该百分比，以进一步消除偏差。

# 建立

你将需要一个实际的数据文件。出于本文的目的，让我们获得一个公开可用的真实数据集。它是美国所有州所有县的 COVID 和数据。你可以从[这里](https://www.openicpsr.org/openicpsr/project/128303/version/V1/view?path=/openicpsr/128303/fcr:versions/V1/Live-Data/us-counties.txt&type=file)下载文件。这是数据集的引用:

> 纽约时报。冠状病毒(新冠肺炎)在美国的数据:现场数据:us counties.txt. Ann Arbor，MI:大学间政治和社会研究联合会[经销商]，2020–12–08。[https://doi.org/10.3886/E128303V1-105700](https://doi.org/10.3886/E128303V1-105700)

下载文件并将其保存在您要处理的文件夹中。在这一点上，我假设你有；

*   正在运行的 Spark 集群(甚至是笔记本电脑上的本地 Spark 集群)。我计划稍后再写一篇关于如何安装和使用本地 Spark 安装的文章。
*   您已经安装了 PySpark 包。

要确保安装良好，请在命令行中输入以下内容:

```
**pyspark**
```

您应该会看到类似这样的屏幕(当然，您的 Spark 和 Python 版本可能会有所不同)

```
Python 3.6.4 (v3.6.4:d48eceb, Dec 19 2017, 06:04:45) [MSC v.1900 32 bit (Intel)] on win32Type “help”, “copyright”, “credits” or “license” for more information.Using Spark’s default log4j profile: org/apache/spark/log4j-defaults.propertiesSetting default log level to “WARN”.To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 2.4.5
      /_/Using Python version 3.6.4 (v3.6.4:d48eceb, Dec 19 2017 06:04:45)
SparkSession available as ‘spark’.
>>>
```

屏幕将等待熟悉的 python 提示符(" > > > ")。正如它在屏幕上显示的那样，已经有了我们需要的一组变量。

**sc** :这是 Spark 上下文。我们不需要使用这个

**spark** :这是 Spark SQL 会话。这将被大量使用。如果在上面的输出中没有看到这一点，可以通过执行以下命令在 PySpark 实例中创建它

```
**from pyspark.sql import *
spark = SparkSession.builder.appName(‘Arup’).getOrCreate()**
```

就是这样。让我们开始讨论今天目标的实质内容。

# 读取数据

首先，我们需要从下载的 CSV 文件中创建一个数据帧。检查文件，我们看到它有一个头，这使我们很容易映射到数据帧的列。它负责列的名称；但是数据类型呢？我们可以提供给他们。但是我们也可以使用一个称为模式推理的巧妙技巧，Spark 将读取几行内容，并尝试从实际数据中猜测数据类型。下面是我们将文件读入名为`df`的数据帧的代码。我们指定文件有一个标题(它应该提供列名),并且应该推断出模式。

```
>>> **df = spark.read.options(header=True, inferSchema=True).csv(“us-counties.txt”)**
```

让我们确认我们得到了所有的记录:

```
>>> **df.count()**
758243
```

它是否导入了所有列的名称并推断出了数据类型？我们可以检查数据帧的模式。

```
>>> **df.printSchema()**root
| — date: timestamp (nullable = true)
| — county: string (nullable = true)
| — state: string (nullable = true)
| — fips: integer (nullable = true)
| — cases: integer (nullable = true)
| — deaths: integer (nullable = true)
```

目前看来不错。让我们检查一个记录:

```
>>> **df.first()**Row(date=datetime.datetime(2020, 1, 21, 0, 0), county=’Snohomish’, state=’Washington’, fips=53061, cases=1, deaths=0)
```

在导入过程中，一切看起来都很好。

# 固定抽样

N 如何从这个数据帧创建一个样本。Spark 提供了一个名为`sample()`的函数，它接受一个参数——要采样的总数据的百分比。让我们使用它。现在，让我们使用 0.001%，或 0.00001 作为采样比率。此外，由于我们只是在试验，我们不会保存到另一个数据帧；我们将检查结果。

```
>>> **df.sample(.00001).show()**+-------------------+----------+--------------+-----+-----+------+
|               date|    county|         state| fips|cases|deaths|
+-------------------+----------+--------------+-----+-----+------+
|2020-04-13 00:00:00|       Bay|      Michigan|26017|   59|     2|
|2020-04-27 00:00:00|   Lubbock|         Texas|48303|  504|    43|
|2020-05-12 00:00:00|    Kimble|         Texas|48267|    1|     0|
|2020-05-24 00:00:00|    Thomas|      Nebraska|31171|    1|     0|
|2020-06-21 00:00:00|     Cedar|      Nebraska|31027|    9|     0|
|2020-07-02 00:00:00|Los Alamos|    New Mexico|35028|    8|     0|
|2020-07-13 00:00:00|  Montrose|      Colorado| 8085|  218|    12|
|2020-09-13 00:00:00|   Mathews|      Virginia|51115|   23|     0|
|2020-11-02 00:00:00|     Surry|North Carolina|37171| 2009|    33|
+-------------------+----------+--------------+-----+-----+------+
```

我们得到了 9 条记录，大约是文件中 758243 条记录的 0.001%。让我们再运行一次这个命令:

```
>>> **df.sample(.00001).show()**+-------------------+-------------------+-------------+-----+-----+------+
|               date|             county|        state| fips|cases|deaths|
+-------------------+-------------------+-------------+-----+-----+------+|2020-03-30 00:00:00|              Vilas|    Wisconsin|55125|    3|     0|
|2020-04-20 00:00:00|             Coffee|      Alabama| 1031|   64|     0|
|2020-05-16 00:00:00|               Clay|West Virginia|54015|    2|     0|
|2020-06-12 00:00:00|          Christian|     Missouri|29043|   29|     0|
|2020-07-22 00:00:00|               Wood|West Virginia|54107|  216|     3|
|2020-07-28 00:00:00|            El Paso|        Texas|48141|13552|   239|
|2020-08-16 00:00:00|             Zapata|        Texas|48505|  203|     3|
|2020-09-11 00:00:00|            Chester| Pennsylvania|42029| 6187|   362|
|2020-09-30 00:00:00|        Roger Mills|     Oklahoma|40129|   59|     1|
|2020-10-16 00:00:00|           McLennan|        Texas|48309| 9290|   134|
|2020-11-02 00:00:00|Virginia Beach city|     Virginia|51810| 8248|   107|
+-------------------+-------------------+-------------+-----+-----+------+
```

我们得到了 11 个记录，也有不同的记录，这是意料之中的。抽样是随机的；因此每次提取都会选择不同的记录。让我们再试一次:

```
>>> **df.sample(.00001).show()**+ — — — — — — — — — -+ — — — — -+ — — — — — — + — — -+ — — -+ — — — +
| date              | county  | state  | fips|cases|deaths|
+ — — — — — — — — — -+ — — — — -+ — — — — — — + — — -+ — — -+ — |2020–07–02 00:00:00| Paulding| Georgia       |13223|  638|  16|
|2020–07–17 00:00:00| Doña Ana| New Mexico    |35013| 1656|  12|
|2020–08–22 00:00:00|Mendocino| California    | 6045|  624|  16|
|2020–09–01 00:00:00| Brantley| Georgia       |13025|  310|   8|
|2020–10–04 00:00:00|Lancaster| Pennsylvania  |42071| 8195| 459|
|2020–11–21 00:00:00| Grant   | Kentucky      |21081|  540|   6|
+ — — — — — — — — — -+ — — — — -+ — — — — — — + ----+ —---+ — — — +
```

我们只拿到了 6 张唱片，而且与前两次不同。每次数字都不同的原因是因为采样百分比是近似值，而不是精确值。所以这些数字大约是原始记录数的 0.001%；但不完全一样。

# 播种

T 他的每次提取不同记录和不同数量的特性(当然作为真正随机性的一个属性)对于许多开发案例和您可能需要的东西来说可能是很好的。然而，在其他情况下，你会想要一个样本；但是希望该样本对于每次调用都是可预测的，即该样本在第一次应该是随机的，但是不应该随着后续调用而改变。你如何实现这一点？

为了再现相同的输出，幸运的是该函数带有一个可选参数 seed。这是你如何使用它。在这个例子中，我使用 9876543210 作为种子。

```
>>> **df.sample(.00001,9876543210).show()**+ — — — — — — — — — -+ — — — — — + — — — — — -+ — — -+ — — -+ — — — +
| date| county| state| fips|cases|deaths|
+ — — — — — — — — — -+ — — — — — + — — — — — -+ — — -+ — — -+ — — — +
|2020–07–24 00:00:00| Delaware| Indiana|18035| 541| 53|
|2020–08–04 00:00:00| Blaine| Idaho|16013| 571| 6|
|2020–08–10 00:00:00| Auglaize| Ohio|39011| 256| 6|
|2020–09–14 00:00:00|Deer Lodge| Montana|30023| 87| 0|
|2020–09–28 00:00:00| Campbell| Virginia|51031| 453| 4|
|2020–10–19 00:00:00| Clay|Mississippi|28025| 672| 21|
+ — — — — — — — — — -+ — — — — — + — — — — — -+ — — -+ — — -+ — — — +
```

下次运行它:

```
>>> **df.sample(.00001,9876543210).show()**+ — — — — — — — — — -+ — — — — — + — — — — — -+ — — -+ — — -+ — — — +
| date| county| state| fips|cases|deaths|
+ — — — — — — — — — -+ — — — — — + — — — — — -+ — — -+ — — -+ — — — +
|2020–07–24 00:00:00| Delaware| Indiana|18035| 541| 53|
|2020–08–04 00:00:00| Blaine| Idaho|16013| 571| 6|
|2020–08–10 00:00:00| Auglaize| Ohio|39011| 256| 6|
|2020–09–14 00:00:00|Deer Lodge| Montana|30023| 87| 0|
|2020–09–28 00:00:00| Campbell| Virginia|51031| 453| 4|
|2020–10–19 00:00:00| Clay|Mississippi|28025| 672| 21|
+ — — — — — — — — — -+ — — — — — + — — — — — -+ — — -+ — — -+ — — — +
```

每次都是同样的样本。如果你想要不同的样本呢？简单地使用不同的种子，或者完全省略种子。

```
>>> **df.sample(.00001,8765432109).show()**+ — — — — — — — — — -+ — — — — + — — — — — — + — — -+ — — -+ — — — +
| date              | county | state      | fips|cases|deaths|
+ — — — — — — — — — -+ — — — — + — — — — — — + — — -+ — — -+ — — — +
|2020–03–27 00:00:00|Franklin| Washington |53021| 12| 0|
|2020–05–08 00:00:00| Pasco  | Florida    |12101| 291| 9|
|2020–05–10 00:00:00|Mariposa| California | 6043| 15| 0|
|2020–06–28 00:00:00| Gregg  | Texas      |48183| 340| 14|
|2020–09–02 00:00:00| Benson |North Dakota|38005| 239| 4|
|2020–10–24 00:00:00| Steuben| New York   |36101| 951| 69|
+ — — — — — — — — — -+ — — — — + — — — — — — + — — -+ — — -+ — — — +
```

# 允许重复吗？

采样时，当函数遇到之前已经选取的值时，它会抑制重复的值。你可能无法接受。如果数据分布是单个值，例如“纽约”碰巧出现了很多次，那么它很可能会出现很多次。但是，由于 sample()取消了它之前选取的值，因此在采样过程中会人为地取消选取该值。该函数有另一个参数来实现这一点。

为了允许可能的重复值，您可以将第一个参数*替换为默认为 False 的*。通过将设置为 true，可以获得可能的重复值。这里有一个例子。我用 1 作为种子，这样我每次都能得到相同的样本。

```
>>> **df.select(“state”).sample(True, .00001, seed=1).show()**+ — — — — — +
| state     |
+ — — — — — +
| Wisconsin |
| Vermont   |
| Texas     |
|Washington |
| Georgia   |
|New Mexico |
| Tennessee |
| Louisiana |
| Hawaii    |
| Georgia   |
| Wisconsin |
| Wyoming   |
+ — — — — — +
```

您可以看到输出中有一些条目，例如“Wisconsin”重复出现。该函数仅仅表示它在采样过程中选取的内容；它没有拒绝之前摘的。现在让我们重新发出同样的命令，将参数设置为 False，或者完全省略参数，因为 False 是默认值。

```
>>> **df.select(“state”).sample(False, .00001, seed=1).show()**+ — — — — — +
| state     |
+ — — — — — +
| Illinois  |
| Georgia   |
|Washington |
| Virginia  |
| Minnesota |
+ — — — — — +
```

如你所见，这些条目都是独一无二的；不重复。

# 减少采样中的偏斜

现在你明白了抽样是如何工作的，你可能意识到我使用的抽样百分比(0.001%)将适用于所有情况。这意味着出现频率较高的值将被更频繁地选取。如果那是你想要的，你不需要再进一步；但是有时您可能希望减少经常重复的数据元素的偏斜效应。请允许我用一个例子来解释。

首先，让我们获得每个州的记录数:

```
>>> **from pyspark.sql.functions import desc**>>> **df.groupBy(“state”).count().alias(“cnt”).sort(desc(“cnt.count”)).show()**+ — — — — — — — — — — + — — -+
| state|count|
+ — — — — — — — — — — + — — -+
| Texas|57096|
| Georgia|38969|
| Virginia|31699|
| Kentucky|28132|
| Missouri|26770|
| Illinois|24258|
| North Carolina|24108|*… output truncated for brevity …*| Massachusetts| 3859|
| Arizona| 3813|
| Vermont| 3703|
| Nevada| 3678|
| New Hampshire| 2705|
| Connecticut| 2258|
| Rhode Island| 1482|
| Hawaii| 1050|
| Delaware| 983|
| Virgin Islands| 824|
|Northern Mariana …| 410|
|District of Columbia| 261|
| Guam| 253|
+ — — — — — — — — — — + — — -+
```

如您所见，一些州有大量记录，例如德克萨斯州，而关岛有少量病例。如果你以固定的 0.001%的比率抽样，关岛出现的机会很小；事实上，它甚至可能不会出现。同样的，高发州像德州，佐治亚等。会出现更多。这可能会导致 ML 模型的测试结果或训练结果不正确。

为了解决这个问题，您可能希望创建一个有条纹的抽样率，而不是对所有记录应用一个单一的抽样百分比，您希望根据州对不同的记录应用不同的抽样百分比。为了简单起见，让我们这样计划:

*   对于拥有 16，000 条或更多记录的州，税率为 0.001%
*   对于记录少于 16，000 条的州，税率为 0.01%。

幸运的是，Spark 提供了另一个函数 sampleBy()允许我们这样做。这个函数有两个参数:

*   *列名*:将被检查得出抽样百分比的列。在这种情况下，它将是“状态”
*   *分数*:dict 格式的那个列值将应用什么分数。例如，如果设置为:

```
{
  ‘Texas’ : 0.00001,
  ‘Guam’ : 0.0001,
   … and so on …
}
```

然后，将根据“状态”列中的值应用采样率。

还有第三个参数——*seed*——就像前面的 *sample* ()函数一样。它是可选的，就像那种情况一样。

但是，不可能为 fractions 参数手动构造 dict 对象。手动完成的工作量相当大。此外，分布可能会改变，我们应该自动化它以与实际的数据模式保持一致。我们是这样做的。首先，让我们将计数放入一个新的数据帧:

```
**df1 = df.groupBy(“state”).count().alias(“cnt”).sort(desc(“cnt.count”))**
```

我们将使用一个名为 lit()的函数向数据帧添加文字。同时，让我们也导入一些我们需要的其他函数。

```
**from pyspark.sql.functions import lit, when, col**
```

使用 lit()我们将把采样率加到相应的状态上。我们将把它存储在另一个数据帧中。采样比率存储在名为“stratum”的列中。

```
**df2 = df1.withColumn("stratum",
when (col(“count”) > (16000), lit(0.00001))
     .otherwise(lit(0.0001))
)**
```

现在，我们将创建一个名为 fractions 的 dict 对象来创建我们需要的状态到采样率的映射:

```
fractions = df2.select(“state”,”stratum”).rdd.collectAsMap()
```

让我们来看看 dict 变量:

```
>>> **print(fractions)**{‘Texas’: 1e-05, ‘Georgia’: 1e-05, ‘Virginia’: 1e-05, ‘Kentucky’: 1e-05, ‘Missouri’: 1e-05, ‘Illinois’: 1e-05, ‘North Carolina’: 1e-05, ‘Iowa’: 1e-05, ‘Tennessee’: 1e-05, ‘Kansas’: 1e-05, ‘Indiana’: 1e-05, ‘Ohio’: 1e-05, ‘Minnesota’: 1e-05, ‘Michigan’: 1e-05, ‘Mississippi’: 1e-05, ‘Nebraska’: 1e-05, ‘Arkansas’: 1e-05, ‘Oklahoma’: 1e-05, ‘Wisconsin’: 1e-05, ‘Florida’: 1e-05, ‘Pennsylvania’: 1e-05, ‘Alabama’: 1e-05, ‘Louisiana’: 0.0001, ‘Puerto Rico’: 0.0001, ‘Colorado’: 0.0001, ‘New York’: 0.0001, ‘California’: 0.0001, ‘South Dakota’: 0.0001, ‘West Virginia’: 0.0001, ‘North Dakota’: 0.0001, ‘South Carolina’: 0.0001, ‘Montana’: 0.0001, ‘Washington’: 0.0001, ‘Idaho’: 0.0001, ‘Oregon’: 0.0001, ‘New Mexico’: 0.0001, ‘Utah’: 0.0001, ‘Maryland’: 0.0001, ‘New Jersey’: 0.0001, ‘Alaska’: 0.0001, ‘Wyoming’: 0.0001, ‘Maine’: 0.0001, ‘Massachusetts’: 0.0001, ‘Arizona’: 0.0001, ‘Vermont’: 0.0001, ‘Nevada’: 0.0001, ‘New Hampshire’: 0.0001, ‘Connecticut’: 0.0001, ‘Rhode Island’: 0.0001, ‘Hawaii’: 0.0001, ‘Delaware’: 0.0001, ‘Virgin Islands’: 0.0001, ‘Northern Mariana Islands’: 0.0001, ‘District of Columbia’: 0.0001, ‘Guam’: 0.0001}
```

这正是我们需要传递给 *sampleBy* ()函数的内容。以上可能不是非常可读。可以用下面的来轻松阅读。

```
>>> **for i in fractions:**
    … **print(i, fractions[i])**
    …Washington 0.0001
Illinois 1e-05
California 1e-05
Arizona 0.0001
Massachusetts 0.0001
*… output truncated …*
```

现在，让我们对数据集进行采样:

```
**sampleDF = df.sampleBy("state",fractions,1).show(1000)
sampleDF.show(1000)**+-------------------+--------------------+-------------+-----+-----+------+
|               date|              county|        state| fips|cases|deaths|
+-------------------+--------------------+-------------+-----+-----+------+
|2020-03-24 00:00:00|                Yuma|      Arizona| 4027|    2|     0|
|2020-03-25 00:00:00|             Webster|    Louisiana|22119|    5|     1|
|2020-04-11 00:00:00|             Orleans|    Louisiana|22071| 5535|   232|
|2020-05-09 00:00:00|         Musselshell|      Montana|30065|    1|     0|
|2020-05-10 00:00:00|              Vernon|    Louisiana|22115|   18|     2|
|2020-05-20 00:00:00|               Glenn|   California| 6021|   12|     0|
|2020-05-23 00:00:00|         Carson City|       Nevada|32510|   83|     4|
|2020-06-01 00:00:00|Dillingham Census...|       Alaska| 2070|    1|     0|*… output truncated …*
```

仅此而已；您的*工作*数据集是 **sampleDF** ，它已经根据实际数据分布使用不同的采样率进行了仔细采样，这将减少偏斜效应。这只是一个简单的例子，说明如何应用两个比率。您可以修改创建 df2 数据帧的代码，根据需要添加任意多的比率。最终目标是创建字典对象，并将其传递给 *sampleBy* ()函数。

# 外卖食品

下面是这篇文章的简要概述。

1.  Spark 提供了一个名为 *sample* ()的函数，它从原始文件中抽取一个随机的数据样本。所有记录的采样率都是固定的。由于这是均匀随机的、频繁出现的值，因此会更频繁地出现在样本中，从而扭曲数据。
2.  Spark 提供了另一个名为 *sampleBy* ()的函数，该函数也提取随机样本；但是采样率可以依赖于文件的内容。这使您可以减少结果样本中常用值的出现，从而减少偏差。

# 进一步阅读

你可以在 Apache Spark 官方文档页面[https://Spark . Apache . org/docs/latest/API/python/reference/API/py Spark . SQL . data frame statfunctions . sample by . html](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrameStatFunctions.sampleBy.html)阅读更多关于 *sampleBy* ()函数的信息。然而，我必须警告，文档非常稀少。但是父文档页面显示了本文中显示的许多命令的扩展语法，这可能会有所帮助。