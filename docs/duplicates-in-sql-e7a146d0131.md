# SQL 中的重复项

> 原文：<https://blog.devgenius.io/duplicates-in-sql-e7a146d0131?source=collection_archive---------4----------------------->

![](img/6bba6020c54cb32feeb009aaf7f46dc1.png)

照片由[无修订](https://unsplash.com/@norevisions?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[上展开](https://unsplash.com/s/photos/same?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 SQL 中处理重复项时，您需要知道的就是这些。

## 识别重复条目

第一步应该是定义哪个列或列的组合形成唯一的行。一旦我们有了决定行唯一性的列集，我们就可以使用以下两种策略来查找哪些列组合有重复条目。

## 1.分组依据

我们可以使用 GROUP By 来找出分组列的组合在表中出现的次数。如果计数大于 1，则表示有两行所选列组合的值相同。因此给出了重复的值。

```
SELECT col1, col2
FROM table_name
WHERE (col1, col2) IN (
    SELECT col1, col2 FROM table_name
    GROUP BY col1, col2 
    HAVING COUNT(*) > 1
    )
```

## 2.带窗口的行编号

在 SQL 中，`PARTITION BY`子句将结果集划分为多个分区，其中给定分区中的每一行对所选列都具有相同的值。如果列值集在表中是唯一的，则具有该列集的分区将具有单行。相反，具有多行的分区表示存在重复值。我们使用`ROW_NUMBER()`为结果集分区内的每一行分配一个连续的整数。大于 1 的序列(出现/出现)意味着该值出现多次。

```
WITH dedup AS (
 SELECT col1, col2,
  ROW_NUMBER() OVER (PARTITION BY col1, col2 ORDER BY col3 ASC) AS occurrence
 FROM
  table_name
 )
SELECT
 col1, col2
FROM
 dedup
WHERE
 occurrence > 1
```

## 选择重复记录

现在，我们想从数据集中选择唯一的记录。我们有很多方法去做。我列出了其中的 5 个。

## 1.明显的

SQL 中的`DISTINCT`函数返回具有唯一值的结果。

```
SELECT DISTINCT col1, col2
FROM table_name
```

## 2.联盟

在 SQL 中，`UNION`结合了两个`SELECT`语句的结果。同时，它会删除结果数据集中的多次出现的行，并且只保留每一行的一次出现。

```
SELECT col1, col2
FROM table_nameUNIONSELECT col1, col2
FROM table_name
```

## 3.横断

在 SQL 中，`INTERSECT`用于获取两个`SELECT`查询的公共行。与`UNION`类似，`INTERSECT`也删除了结果数据集中的多次出现的行，并且每行只保留一次出现。

```
SELECT col1, col2
FROM table_nameINTERSECTSELECT col1, col2
FROM table_name
```

## 4.ROW_NUMBER

如上所述，大于 1 的序列(出现/出现)意味着该值出现不止一次。值 1 表示列值集的第一次出现。因此，我们对`occurrence` 值设置为 1 的行进行过滤。这将产生唯一的行。

```
WITH dedup AS (
 SELECT col1, col2,
  **ROW_NUMBER() OVER (PARTITION BY col1, col2 ORDER BY col3 ASC) AS occurrence**
 FROM
  table_name
 )
SELECT
 col1, col2
FROM
 dedup
WHERE
 **occurrence** = 1
```

## 5.分组依据

在 SQL 中，`GROUP BY`为`GROUP BY`列的每个唯一组合返回一行。因此，如果我们简单地选择列并按相同的列对它们进行分组，我们将得到唯一的行。

```
SELECT col1, col2
FROM table_name
GROUP BY col1, col2
```

## 删除重复记录并保留一个

现在我们想从数据集中删除重复的记录。这可以通过使用 ROW_NUMBER 和 PARTITION BY 来实现。要决定保留哪些行(最新/最早或基于其他列),我们可以在窗口中使用 ORDER BY。

```
WITH dedup AS (
 SELECT col1, col2,
  **ROW_NUMBER() OVER (PARTITION BY col1, col2 ORDER BY col3 ASC) AS occurrence**
 FROM
  table_name
 )
delete from dedup where **occurrence >** 1 -- deletes the earliest (based on col3)
```

如果我们希望保留最新的，我们可以将订单按值切换到`DESC`。同样，如果我们想要保留两个副本，那么我们可以将 where 子句改为`occurrence > 2`。

SQL 快乐！！！