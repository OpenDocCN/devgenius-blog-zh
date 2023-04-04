# Cassandra 主键与分区键和聚集键

> 原文：<https://blog.devgenius.io/cassandra-primary-vs-partitioning-vs-clustering-keys-3b3fa0e317f4?source=collection_archive---------2----------------------->

![](img/eddbb907be3fdfcfcb1b02c66ce42f7a.png)

照片由 [Sidorova Alice](https://unsplash.com/@sesambrotchen?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

卡珊德拉有多种类型的钥匙。即:

1.  主关键字
2.  分区键
3.  聚类键

让我们逐一查看一下，以便更好地理解它们。

# 主关键字

每个表都需要一个主键。主键可以是一个字段，也可以是多个字段的组合。每个记录的主键必须是唯一的。否则，如果您试图添加主键已经存在的记录，Cassandra 将执行 upsert。

让我们来看一个现实生活中的卡珊德拉表的例子:

```
CREATE TABLE player (
   name text,
   club text,
   league text,
   nationality text,
	 kit_number text,
   position text,
	 goals int,
   assists int,
   appearences int,
   PRIMARY KEY (club, league, name, kit_number, position, goals)
)
```

当一个表有多个字段作为主键时，我们称之为**复合主键**。该表也可以有一个字段作为其主键。

# 分区键

对于单个字段主键，**分区键**是同一个字段。

对于复合主键，默认情况下，**分区键**是主键的第一个字段。所以对于上面的例子，表的分区键是`club`。

假设您想要定义一个由多个字段组成的分区键。你是怎么做到的？

很简单，只要把你想成为分区键一部分的字段放在括号内。让我们来看看这是如何工作的。

继续我们当前的示例表，假设您希望将`name`和`club`的组合作为分区键。这是您所做的唯一更改:

```
PRIMARY KEY ((name, club), league, kit_number, position, goals)
```

现在我们知道了如何定义不同的分区键，让我们来讨论一下分区键到底是什么。

Cassandra 是一个由多个节点组成的分布式数据库。数据存储在分区中。分区键将数据分组到同一个分区中。一台机器可以有多个分区。您希望*相似的*数据留在同一个分区中，以便更快地读取。

让我们来看一下我们最初的带有`club`分区键的例子。这意味着，来自同一个俱乐部的球员将在同一个分区。Cassandra 将使用一致的散列法，以便对于一个给定的俱乐部，所有球员记录总是在同一个分区中结束。

最后，让我们看一个使用复合分区键的例子，例如`(position,league)`。在这种情况下，同一联盟中同一位置的所有球员都将处于同一分区。

如您所见，**分区键**“分块”数据，以便 Cassandra 知道扫描哪个分区(进而哪个节点)来查找传入的查询。

# 聚类键

聚类键决定分区内数据的排序顺序。为了便于访问，下面是我们原始示例的另一个例子:

```
CREATE TABLE player (
   name text,
   club text,
   league text,
   nationality text,
	 kit_number text,
   position text,
	 goals int,
   assists int,
   appearances int,
   PRIMARY KEY (club, league, name, kit_number, position, goals)
)
```

除分区键外，主键中的每个字段都是**聚类键的一部分。**

在这种情况下，我们知道`club`是分区键。

所以`league name kit_number position goals`是**的聚类键。**您可以为每个聚类键定义排序顺序。假设您可以按照递减的 kit_number 和递增的目标对其进行排序。

排序顺序与主键中字段的顺序相同。在上面的例子中，数据是这样排列的:

*   来自同一个俱乐部的每个玩家最终都在同一个独特的分区中
*   在一个分区内，玩家按照他们所属的联赛排序
*   然后按他们的名字排序
*   其中，它们按 kit_number 排序
*   …根据主键中字段的顺序依次类推

**因此，当涉及到模式设计时，主键中字段的顺序非常重要。**

可以为不同字段的聚类关键字定义不同的排序顺序。例如

```
CREATE TABLE player (
   name text,
   club text,
   league text,
   nationality text,
	 kit_number text,
   position text,
	 goals int,
   assists int,
   appearances int,
   PRIMARY KEY (club, league, name, kit_number, position, goals)
)
WITH CLUSTERING ORDER BY (league ASC, name DESC, kit_number ASC, position DESC );
```

因此，在这个示例中，在一个分区内，数据将首先按联赛升序排序，然后按姓名降序排序，再按 kit_number 升序排序，然后按位置降序排序，最后按默认顺序(即升序)按进球排序。

如您所见，这两件事都很重要:

*   您在主键中放置字段的顺序
*   为每个字段定义排序顺序的方式(如果不定义，默认为升序)

# 最后的想法

定义 Cassandra 模式的方式非常重要。在设计模式之前，您应该对自己的读写模式有所了解。这将确保您选择正确的分区和聚集键来正确地组织磁盘中的数据。这样，你的读写速度会非常快。