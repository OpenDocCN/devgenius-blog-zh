# PySpark 中的 RDDs 编程

> 原文：<https://blog.devgenius.io/programming-with-rdds-in-pyspark-5889ff90da91?source=collection_archive---------10----------------------->

![](img/60bd032c92811dc24a7f3469aa3ad2bd.png)

Matthew Jungling 在 [Unsplash](https://unsplash.com/photos/xBhVNtslEuE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# PySpark 中的 RDDs 编程

![](img/86b9903db2b11e8f6fac5231404713bb.png)

当处理大数据时，这是当今时代数据科学家的一种需要，执行是在 Apache spark 上进行的。spark 最初构建于 Scala 之上，现在可以使用 PySpark 在 Python 之上运行。

在高层次上，每个 Spark 应用程序都由一个驱动程序组成，该程序运行用户的主要功能，并在集群上执行各种并行操作。Spark 提供的主要抽象是一个弹性分布式数据集(RDD ),这是一个跨集群节点划分的元素集合，可以并行操作。

这篇博客主要是使用 PySpark 对 RDD 进行一个温和的介绍。

Spark 中的 RDD(弹性分布式数据集)只是一个**不可变的分布式对象集合**。每个 RDD 被分成多个分区，这些分区可以在集群的不同节点上进行计算。rdd 是 PySpark 中的基本数据类型。

## 先决条件:

1.  大数据基础
2.  Pyspark 工作原理和基本原理
3.  Python 知识

## 配置平台

我们将使用 Google collaboratory 来运行 PySpark 命令，所以首先让我们配置 Colab。

现在让我们初始化一个 Spark 会话

太好了！

我们安装了 PySpark，并在 colab 中用一个命令启动了一个 Spark 会话。现在让我们创建 rdd。

# 在火花中创造 RDD

创建数据框有两种方式:

1.  通过加载外部数据集
2.  分发一组对象集合

让我们首先使用`parallelize()`方法从程序中已经存在的对象集合中创建 RDD。

`show()` —打印数据帧中的前 20 行

`collect()` —以数组形式返回 RDD 的所有元素

我们还可以使用`read.csv()`从 CSV 文件创建 RDD，首先创建一个数据帧，然后使用`.rdd`将其转换为 RDD

`first()`命令从 RDD 中检索第一条记录

使用`textFile()`从文本文件创建 RDD

使用`read.json()`从一个 JSON 文件创建一个 RDD，这会创建一个数据框，我们使用`.rdd`将它转换成 RDD

`take(n)`返回包含数据集前 n 个元素的数组

注意:rdd 也可以从以下位置创建

1.  通过从数据库中读取数据集

语法-

```
df = spark.read.jdbc(url=url, table=table_name,properties=properties)
```

1.  被一个 HDFS 人

语法-

```
from pyspark.sql import HiveContext
hc = HiveContext(sc)
tf1 = sc.textFile("hdfs://.../demo.CSV")
hc.sql("use intg_cme_w")
spf = hc.sql("SELECT * FROM spf LIMIT 100")
```

# 火花操作

有两种主要类型的火花操作:

1.  转型——在原有的基础上建设新的 RDD
2.  操作-对 RDD 执行计算并返回值

## 转换

PySpark RDD 转换是惰性评估，这意味着 Spark 会根据您在 RDD 上执行的所有操作创建一个图，并且只有在 RDD 上执行操作时才会开始执行图，简单地说，操作只有在被操作调用时才会执行。它用于从一个 RDD 转换/更新到另一个。在 RDD 上执行时，它会产生一个或多个新 RDD。

由于 RDD 本质上是不可变的，转换总是创建新的 RDD 而不更新现有的，因此，RDD 转换链创建了 RDD 谱系(也称为 RDD 算子图或 RDD 依赖图)。

## 行动

另一方面，动作基于 RDD 计算结果，并将其返回给驱动程序或保存到外部存储系统(例如，HDFS)。

[在这里查看所有转换及其描述的官方文档](https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations)

现在让我们采取一种实用的方法，一步一步地熟悉 RDDs 编程。

# 示例 1

我们将做以下事情-

1.  从 python 列表创建 RDD
2.  获取第一个元素
3.  获取前两个元素
4.  获取 RDD 中的分区数量

我们将使用`parallelize()`方法从列表中创建一个 RDD。

对于大数据，甚至是表格数据，一个表可能有 1000 多列。有时，分析人员想看看那些数据列是什么样子。该函数定义在 RDD 上，将返回 RDD 的第一个元素。

要从列表中获取多个元素，我们可以使用`take()`。

使用`getNumPartitions()`可以获取集合的分区数量。

# 示例 2

我们将做以下事情-

1.  从 python 列表创建 RDD
2.  将每个值增加 1.1 倍
3.  过滤掉大于特定值的值

[](https://carbon.now.sh/?bg=rgba%2528171%252C+184%252C+195%252C+1%2529&t=night-owl&wt=none&l=markdown&width=500&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=false&pv=10px&ph=9px&ln=false&fl=1&fm=Hack&fs=14.5px&lh=133%2525&si=false&es=1x&wm=false&code=%252523%252520create%252520an%252520RDD%252520from%252520a%252520python%252520list%25250A%252523%252520this%252520list%252520contains%252520prices%252520of%252520productX%252520in%252520year%2525202021%252520each%252520month%25250AtempData%252520%25253D%252520%25255B59%25252C57.2%25252C53.6%25252C55.4%25252C51.8%25252C53.6%25252C55.4%25252C57%25252C59.2%25252C56.8%25252C53.9%25252C55.5%25255D%25250Ardd%252520%25253D%252520sc.parallelize%2528tempData%2529%25250Ardd.collect%2528%2529%25250A%252520%252520%252520%252520%252520%25250A%25255B59%25252C%25252057.2%25252C%25252053.6%25252C%25252055.4%25252C%25252051.8%25252C%25252053.6%25252C%25252055.4%25252C%25252057%25252C%25252059.2%25252C%25252056.8%25252C%25252053.9%25252C%25252055.5%25255D) [## 碳

### Carbon 是创建和共享源代码美丽图像的最简单方式。

碳. now.sh](https://carbon.now.sh/?bg=rgba%2528171%252C+184%252C+195%252C+1%2529&t=night-owl&wt=none&l=markdown&width=500&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=false&pv=10px&ph=9px&ln=false&fl=1&fm=Hack&fs=14.5px&lh=133%2525&si=false&es=1x&wm=false&code=%252523%252520create%252520an%252520RDD%252520from%252520a%252520python%252520list%25250A%252523%252520this%252520list%252520contains%252520prices%252520of%252520productX%252520in%252520year%2525202021%252520each%252520month%25250AtempData%252520%25253D%252520%25255B59%25252C57.2%25252C53.6%25252C55.4%25252C51.8%25252C53.6%25252C55.4%25252C57%25252C59.2%25252C56.8%25252C53.9%25252C55.5%25255D%25250Ardd%252520%25253D%252520sc.parallelize%2528tempData%2529%25250Ardd.collect%2528%2529%25250A%252520%252520%252520%252520%252520%25250A%25255B59%25252C%25252057.2%25252C%25252053.6%25252C%25252055.4%25252C%25252051.8%25252C%25252053.6%25252C%25252055.4%25252C%25252057%25252C%25252059.2%25252C%25252056.8%25252C%25252053.9%25252C%25252055.5%25255D) 

我们最终有四个元素指示温度大于或等于 13。现在您已经了解了使用 PySpark 对数据进行基本分析的方法。

# 示例 3:基本数据操作

我们将做以下工作:

1.  从表格中列出清单
2.  并行化数据
3.  计算平均值
4.  过滤平均值
5.  查找最佳结果
6.  寻找底部结果
7.  基于一个值获取所有结果

每个学生每年每学期的平均成绩

第二年平均成绩最高的前三名学生

第二年平均成绩最低的前三名学生

第二年第二学期平均成绩超过 80%的所有学生

使用 map()函数通常很有帮助。在本例中，可以使用 map()计算每年每学期的平均成绩。获得前 k 个元素，例如前 k 个高性能债券，是一个一般的数据科学问题。PySpark takeOrdered()函数将从我们的 RDD 中获取顶部 k 或顶部底部元素。可以使用 filter()函数筛选第二年平均成绩超过 80%的学生。

takeOrdered()函数是一个动作。为了打印结果，我们没有使用 collect()函数来获取数据。记住，转换创建了另一个 RDD，所以我们需要 collect()函数来收集数据。但是一个动作将直接获取数据到驱动程序，而 collect()不是必需的。

# 参考文献—

1.  GitHub 查看代码—[https://GitHub . com/RaghuMadhavTiwari/py spark-practice/blob/main/Programming _ with _ RDDs _ in _ py spark . ipynb](https://github.com/RaghuMadhavTiwari/PySpark-practice/blob/main/Programming_with_RDDs_in_PySpark.ipynb)
2.  PySpark 食谱 _ 用 PySpark2 解决问题的方法

# 祝你好运！

如果你想进一步讨论这个问题，请通过 LinkedIn 联系我，地址: [Raghu Madhav Tiwari](https://www.linkedin.com/in/raghumadhavtiwari/) ！**在下面留下掌声和评论支持博客吧！关注更多。**

PS:与我进行一对一的会谈(此处为新生或模拟面试提供指导—[topmate.io/raghu_tiwari?utm_source=topmate&UTM _ medium = popup&UTM _ campaign = social profile](http://topmate.io/raghu_tiwari?utm_source=topmate&utm_medium=popup&utm_campaign=SocialProfile)