# 你会发现有用的 SQL Cheatsheet

> 原文：<https://blog.devgenius.io/sql-cheatsheet-you-will-find-useful-b5316f308525?source=collection_archive---------8----------------------->

## 使用此备忘单，让您的 SQL 之旅更加轻松

![](img/c5ea7821e0d68aaee5cf8c9e22570b3b.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Sunder Muthukumaran](https://unsplash.com/@sunder_2k25?utm_source=medium&utm_medium=referral) 拍摄

SQL 是标准且广泛使用的编程语言，用于管理关系数据库并对其中的数据执行各种操作。SQL 代表结构化查询语言。对于关系数据库，ANSI(美国国家标准协会)宣布 SQL 为标准语言。在这里，我试图用这个备忘单让你的 SQL 之旅变得更容易。

我们去看看。

## 从表中查询数据

*   从表中查询列 c1、c2 中的数据

```
SELECT c1, c2 FROM t;
```

*   查询表中的所有行和列

```
SELECT * FROM t;
```

*   用条件查询数据和筛选行

```
Select c1, c2 FROM t
WHERE condition;
```

*   从表中查询不同的行

```
SELECT DISTINCT c1 FROM t 
WHERE condition;
```

*   按升序或降序对结果集进行排序

```
SELECT c1, c2 FROM t
ORDER BY c1 ASC [DESC];
```

*   跳过行的*偏移*，返回下 n 行

```
SELECT c1, c2 FROM t
ORDER BY c1
LIMIT n OFFSET offset;
```

*   使用聚合函数对行进行分组

```
SELECT c1, aggregate(c2)
FROM t
GROUP BY c1;
```

*   使用 HAVING 子句筛选组

```
SELECT c1, aggregate(c2)
FROM t
GROUP BY c1
HAVING condition;
```

## 从多个表中查询

*   内部联接 t1 和 t2

```
SELECT c1, c2
FROM t1
INNER JOIN t2 ON condition;
```

*   左连接 t1 和 t1

```
SELECT c1, c2
FROM t1
LEFT JOIN t2 ON condition;
```

*   右连接 t1 和 t2

```
SELECT c1, c2
FROM t1
RIGHT JOIN t2 ON condition;
```

*   执行完全外部联接

```
SELECT c1, c2
FROM t1
FULL OUTER JOIN t2 ON condition;
```

*   生成表中行的笛卡尔乘积

```
SELECT c1, c2
FROM t1
CROSS JOIN t2;
```

*   执行交叉连接的另一种方式

```
SELECT c1, c2
FROM t1, t2;
```

*   使用 INNER JOIN 子句将 t1 连接到自身

```
SELECT c1, c2
FROM t1 A
INNER JOIN t2 B ON condition;
```

**使用 SQL 运算符**

*   合并两个查询中的行

```
SELECT c1, c2 FROM t1
UNION [ALL]
SELECT c1, c2 FROM t2;
```

*   返回两个查询的交集

```
SELECT c1, c2 FROM t1
INTERSECT
SELECT c1, c2 FROM t2;
```

*   从另一个结果集中减去一个结果集

```
SELECT c1, c2 FROM t1
MINUS
SELECT c1, c2 FROM t2;
```

*   使用模式匹配%查询行，_

```
SELECT c1, c2 FROM t1
WHERE c1 [NOT] LIKE pattern;
```

*   查询列表中的行

```
SELECT c1, c2 FROM t
WHERE c1 [NOT] IN value_list;
```

*   查询两个值之间的行

```
SELECT c1, c2 FROM t
WHERE c1 BETWEEN low AND high;
```

*   检查表中的值是否为空

```
SELECT c1, c2 FROM t
WHERE c1 IS [NOT] NULL;
```

## **管理表格**

*   创建一个包含三列的新表

```
CREATE TABLE t (
id INT PRIMARY KEY,
name VARCHAR NOT NULL,
price INT DEFAULT 0
);
```

*   从数据库中删除该表

```
DROP TABLE t ;
```

*   向表中添加新列

```
ALTER TABLE t ADD column;
```

*   从表中删除列 c

```
ALTER TABLE t DROP COLUMN c ;
```

*   添加约束

```
ALTER TABLE t ADD constraint;
```

*   取消限制

```
ALTER TABLE t DROP constraint;
```

*   将表从 t1 重命名为 t2

```
ALTER TABLE t1 RENAME TO c2 ;
```

*   将列 c1 重命名为 c2

```
ALTER TABLE t1 RENAME c1 TO c2 ;
```

*   删除表格中的所有数据

```
TRUNCATE TABLE t;
```

**使用 SQL 约束**

*   将 c1 和 c2 设置为主键

```
CREATE TABLE t(
c1 INT, c2 INT, c3 VARCHAR,
PRIMARY KEY (c1,c2)
);
```

*   将 c2 列设置为外键

```
CREATE TABLE t1(
c1 INT PRIMARY KEY,
c2 INT,
FOREIGN KEY (c2) REFERENCES t2(c2)
);
```

*   使 c1 和 c2 中的值唯一

```
CREATE TABLE t(
c1 INT, c1 INT,
UNIQUE(c2,c3)
);
```

*   确保 c1 > 0 且 c1 中的值> = c2

```
CREATE TABLE t(
c1 INT, c2 INT,
CHECK(c1> 0 AND c1 >= c2)
);
```

*   设置 c2 列中的值不为空

```
c1 INT PRIMARY KEY,
c2 VARCHAR NOT NULL
);
```

**修改数据**

*   在表格中插入一行

```
INSERT INTO t(column_list)
VALUES(value_list);
```

*   在表格中插入多行

```
INSERT INTO t(column_list)
VALUES (value_list),
(value_list), ....;
```

*   将 t2 中的行插入 t1

```
INSERT INTO t1(column_list)
SELECT column_list
FROM t2;
```

*   为所有行更新列 c1 中的新值

```
UPDATE t
SET c1 = new_value;
```

*   更新 c1、c2 列中符合条件的值

```
UPDATE t
SET c1 = new_value,
c2 = new_value
WHERE condition;
```

*   删除表格中的所有数据

```
DELETE FROM t;
```

*   删除表中行的子集

```
DELETE FROM t
WHERE condition;
```

**管理视图**

*   创建一个包含 c1 和 c2 的新视图

```
CREATE VIEW v(c1,c2)
AS
SELECT c1, c2
FROM t;
```

*   使用检查选项创建新视图

```
CREATE VIEW v(c1,c2)
AS
SELECT c1, c2
FROM t;
WITH [CASCADED | LOCAL] CHECK OPTION;
```

*   创建递归视图

```
CREATE RECURSIVE VIEW v
AS
select-statement -- anchor part
UNION [ALL]
select-statement; -- recursive part
```

*   创建临时视图

```
CREATE TEMPORARY VIEW v
AS
SELECT c1, c2
FROM t;
```

*   删除视图

```
DROP VIEW view_name
```

**管理指标**

*   在表 t 的 c1 和 c2 上创建一个索引

```
CREATE INDEX idx_name
ON t(c1,c2);
```

*   在表 t 的 c3、c4 上创建唯一索引

```
CREATE UNIQUE INDEX idx_name
ON t(c3,c4);
```

*   删除索引

```
DROP INDEX idx_name;
```

**SQL 聚合函数**

*   AVG 返回列表的平均值
*   COUNT 返回列表中元素的数量
*   SUM 返回列表的总和
*   MAX 返回列表中的最大值
*   MIN 返回列表中的最小值

**管理触发器**

*   创建或修改触发器

```
CREATE OR MODIFY TRIGGER trigger_name
WHEN EVENT
ON table_name TRIGGER_TYPE
EXECUTE stored_procedure;
```

*   当
    前>时—事件发生前调用
    后>时—事件发生后调用
*   事件
    >插入—调用插入
    >更新—调用更新
    >删除—调用删除
*   TRIGGER _ TYPE
    FOR 每行> FOR 每条语句

*   创建在将新行插入人员表之前调用的触发器

```
CREATE TRIGGER before_insert_person
BEFORE INSERT
ON person FOR EACH ROW
EXECUTE stored_procedure;
```

*   删除特定触发器

```
DROP TRIGGER trigger_name
```

## 最终想法

在这里，我试图涵盖本文中所有有用的 SQL 命令。如果我错过了什么，请在评论区提出来。感谢阅读。

***下面是我的另一篇关于 SQL 连接类型的文章。希望对你们也有帮助。***

[](https://javascript.plainenglish.io/what-are-sql-join-types-described-with-image-281c10949818) [## 什么是 SQL 连接类型(用图片描述)

### 通过理解连接类型的基础知识，有效地使用 SQL

javascript.plainenglish.io](https://javascript.plainenglish.io/what-are-sql-join-types-described-with-image-281c10949818)