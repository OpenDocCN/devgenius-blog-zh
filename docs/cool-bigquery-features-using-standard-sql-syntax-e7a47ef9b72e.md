# 使用标准 SQL 语法的超酷 BigQuery 特性

> 原文：<https://blog.devgenius.io/cool-bigquery-features-using-standard-sql-syntax-e7a47ef9b72e?source=collection_archive---------3----------------------->

BigQuery 中有几个很酷的特性，我们可以通过标准的 SQL 语法来使用，即使是最频繁的用户也常常不知道。本文件旨在展示其中一些，而不是详尽无遗的工作。

下面的主题没有逻辑顺序，只是收集了一些我感兴趣的特性/功能。

以下内容不必按顺序阅读，只是一种目录。

源代码可从:[https://github . com/brbep/big query-cool-features-standard-SQL](https://github.com/brbep/bigquery-cool-features-standard-sql)获得。

## **开始前的假设**

*   你需要有一个谷歌云帐户。如果没有，可以从[的**这里**的](https://cloud.google.com/free)创建。点击右上角的**免费开始**按钮。
*   例如，您需要一个 Bigquery 项目(参见 [**文档**](https://cloud.google.com/resource-manager/docs/creating-managing-projects) )以及编辑表格和访问数据的权限。

## **参考文献**

*   [Bigquery 文档](https://cloud.google.com/bigquery/docs)
*   [BigQuery 管理参考指南](https://cloud.google.com/blog/topics/developers-practitioners/bigquery-admin-reference-guide-recap)
*   [前 25 个谷歌搜索词，现在在 BigQuery](https://cloud.google.com/blog/products/data-analytics/top-25-google-search-terms-now-in-bigquery)
*   [BigQuery 发布说明](https://cloud.google.com/bigquery/docs/release-notes)

# **目录**

1.  [删除重复数据](https://medium.com/p/e7a47ef9b72e#b094)
2.  [在一列或整个表格中搜索字符串](https://medium.com/p/e7a47ef9b72e#7df2)
3.  [从日期表达式](https://medium.com/p/e7a47ef9b72e#a759)中返回最后一天
4.  [通过 SQL 实现安全](https://medium.com/p/e7a47ef9b72e#638d)
5.  [重命名表](https://medium.com/p/e7a47ef9b72e#7f61)
6.  [访问历史数据](https://medium.com/p/e7a47ef9b72e#b7b3)
7.  [脚本和存储过程](https://medium.com/p/e7a47ef9b72e#1f8e)
8.  [多对帐单交易](https://medium.com/p/e7a47ef9b72e#e567)
9.  [动态 SQL](https://medium.com/p/e7a47ef9b72e#b090)
10.  [根据列值生成行 ID](https://medium.com/p/e7a47ef9b72e#987a)
11.  [调试报表](https://medium.com/p/e7a47ef9b72e#a9c2)
12.  [创建外部表格](https://medium.com/p/e7a47ef9b72e#acfa)
13.  [导出数据](https://medium.com/p/e7a47ef9b72e#30f7)
14.  [创建和执行 ML 模型](https://medium.com/p/e7a47ef9b72e#efd0)
15.  [分析和可视化地理空间数据](https://medium.com/p/e7a47ef9b72e#3f28)

# 1.删除重复数据

```
# Data Source
WITH produce AS
 (SELECT 'kale' as item, 23 as purchases, 'vegetable' as category, CURRENT_TIMESTAMP()-MAKE_INTERVAL(minute => 2) AS updated_timestamp
  UNION ALL SELECT 'kale' as item, 23 as purchases, 'vegetable' as category, CURRENT_TIMESTAMP()-MAKE_INTERVAL(second => 63) AS updated_timestamp
  UNION ALL SELECT 'kale' as item, 23 as purchases, 'vegetable' as category, CURRENT_TIMESTAMP()-MAKE_INTERVAL(second => 75) AS updated_timestamp
  UNION ALL SELECT 'orange', 2, 'fruit', CURRENT_TIMESTAMP()-MAKE_INTERVAL(second => 72) AS updated_timestamp
  UNION ALL SELECT 'cabbage', 9, 'vegetable', CURRENT_TIMESTAMP()-MAKE_INTERVAL(hour => 7) AS updated_timestamp
  UNION ALL SELECT 'apple', 8, 'fruit', CURRENT_TIMESTAMP()-MAKE_INTERVAL(second => 33) AS updated_timestamp
  UNION ALL SELECT 'leek', 2, 'vegetable', CURRENT_TIMESTAMP()-MAKE_INTERVAL(second => 157) AS updated_timestamp
  UNION ALL SELECT 'lettuce', 10, 'vegetable', CURRENT_TIMESTAMP()-MAKE_INTERVAL(hour => 1000) AS updated_timestamp),# Deduplication Options# By ROW_NUMBER
dedup_by_rownumber AS (
  SELECT * EXCEPT(rnumber)
  FROM (
    SELECT
      *,
      ROW_NUMBER() OVER (PARTITION BY item ORDER BY updated_timestamp DESC) AS rnumber
    FROM produce )
  WHERE rnumber = 1
),# By QUALIFY
dedup_by_qualify AS (
  SELECT *
  FROM produce
  WHERE true
  QUALIFY ROW_NUMBER() OVER (PARTITION BY item ORDER BY updated_timestamp DESC) = 1
),# By ARRAY_AGG
dedup_by_arrayagg AS (
  SELECT event.*
  FROM (
    SELECT 
      ARRAY_AGG(pd ORDER BY pd.updated_timestamp DESC LIMIT 1)[OFFSET(0)] AS event
    FROM produce AS pd
    GROUP BY pd.item)
)SELECT *, 'ROW_NUMBER' AS dedup_by FROM dedup_by_rownumber
UNION ALL SELECT *, 'QUALIFY' FROM dedup_by_qualify
UNION ALL SELECT *, 'ARRAY_AGG' FROM dedup_by_arrayagg
```

**提示**

*   `QUALIFY`子句过滤分析函数的结果。
*   **第一条或最后一条记录**:当试图计算数据子集中的第一条或最后一条记录时，如果单个分区中的`ORDER BY`元素太多，使用`ROW_NUMBER()`函数可能会失败，并出现资源超出错误。取而代之的是，尝试使用`ARRAY_AGG()`——这样运行起来更有效率，因为`ORDER BY`被允许在每个`GROUP BY`上丢弃除顶部记录之外的所有内容。

**文档:**[MAKE _ INTERVAL](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#make_interval)；[排 _ 号](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#row_number)；[晋级](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#qualify_clause)；[数组 _ 聚集](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#array_agg)

# 2.在列或整个表格中搜索字符串

执行规范化的、不区分大小写的搜索，以查看某个值是否存在于表达式中。如果值存在，则返回`TRUE`，否则返回`FALSE`。

```
DECLARE search_value STRING DEFAULT 'toast';WITH recipes AS
 (SELECT 'Blueberry pancakes' as breakfast, 'Egg salad sandwich' as lunch, 'Potato dumplings' as dinner UNION ALL
  SELECT 'Potato pancakes', 'Toasted cheese sandwich', 'Beef stroganoff' UNION ALL
  SELECT 'Ham scramble', 'Steak avocado salad', 'Tomato pasta' UNION ALL
  SELECT 'Avocado toast', 'Tomato soup', 'Blueberry salmon' UNION ALL
  SELECT 'Corned beef hash', 'Lentil potato soup', 'Glazed ham')

SELECT 
  *,
  CONTAINS_SUBSTR(breakfast, search_value) AS breakfast_result
FROM recipes 
WHERE CONTAINS_SUBSTR(recipes, search_value);
```

**文档:** [包含 _SUBSTR](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#contains_substr)

# 3.从日期表达式中返回最后一天

```
--DATE PART: YEAR, QUARTER, MONTH, WEEK, WEEK(SUNDAY, MONDAY etc.), ISOWEEK, ISOYEAR
SELECT LAST_DAY(DATE '2021-02-25', MONTH) AS last_day;
```

**文档:** [最后 _ 天](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#last_day)

# 4.通过 SQL 保护

```
GRANT `roles/bigquery.dataViewer` ON TABLE my_project.my_dataset.my_table
TO "user:[tom@example.com](mailto:tom@example.com)", "user:[sara@example.com](mailto:sara@example.com)";REVOKE `roles/bigquery.admin` ON SCHEMA my_project.my_dataset
FROM "group:[example-team@example.com](mailto:example-team@example.com)", "serviceAccount:[user@test-project.iam.gserviceaccount.com](mailto:user@test-project.iam.gserviceaccount.com)"
```

**文档:** [标准 SQL 中的数据控制语言语句](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-control-language)

# 5.重命名表格

```
ALTER TABLE my_project.my_dataset.my_table RENAME TO new_table_name
```

**文档:** [将表重命名为语句](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language?hl=en#alter_table_rename_to_statement)

# 6.访问历史数据

使用`time travel`您可以从最近**七天**内的任何点访问数据。这个特性是由 Google 提供和管理的，你不需要做任何事情来获得它，只需要运行一个 SQL 查询来访问历史数据。

```
SELECT *
FROM `my_project.my_dataset.my_table`
FOR SYSTEM_TIME AS OF TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 30 SECOND);
```

**文档:** [时间旅行](https://cloud.google.com/bigquery/docs/time-travel)

# 7.脚本和存储过程

```
CREATE OR REPLACE TABLE my_project.my_dataset.my_table (customer_id STRING);
```

*   设置变量，运行 INSERT 语句，并将结果显示为格式化的文本字符串:

```
DECLARE id STRING;
SET id = GENERATE_UUID();INSERT INTO my_project.my_dataset.my_table (customer_id)
   VALUES(id);SELECT FORMAT("Created customer ID %s", id);
```

*   将相同的脚本转换成一个过程:

```
CREATE OR REPLACE PROCEDURE my_dataset.create_customer()
BEGIN
  DECLARE id STRING;
  SET id = GENERATE_UUID();
  INSERT INTO my_project.my_dataset.my_table (customer_id)
    VALUES(id);
  SELECT FORMAT("Created customer %s", id);
END;CALL my_dataset.create_customer();
```

**文档:** [脚本和存储过程](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting-concepts)

# 8.多语句交易

```
CREATE OR REPLACE TABLE my_dataset.inventory
(
 product string,
 quantity int64,
 supply_constrained bool
);CREATE OR REPLACE TABLE my_dataset.new_arrivals
(
 product string,
 quantity int64,
 warehouse string
);INSERT my_dataset.inventory (product, quantity)
VALUES('top load washer', 10),
     ('front load washer', 20),
     ('dryer', 30),
     ('refrigerator', 10),
     ('microwave', 20),
     ('dishwasher', 30);INSERT my_dataset.new_arrivals (product, quantity, warehouse)
VALUES('top load washer', 100, 'warehouse #1'),
     ('dryer', 200, 'warehouse #2'),
     ('oven', 300, 'warehouse #1');BEGIN TRANSACTION;-- Create a temporary table that holds new arrivals from 'warehouse #1'.
CREATE TEMP TABLE tmp
  AS SELECT * FROM my_dataset.new_arrivals WHERE warehouse = 'warehouse #1';-- Delete the matching records from the NewArravals table.
DELETE my_dataset.new_arrivals WHERE warehouse = 'warehouse #1';-- Merge the records from the temporary table into the inventory table.
MERGE my_dataset.inventory AS I
USING tmp AS T
ON I.product = T.product
WHEN NOT MATCHED THEN
 INSERT(product, quantity, supply_constrained)
 VALUES(product, quantity, false)
WHEN MATCHED THEN
 UPDATE SET quantity = I.quantity + T.quantity;-- Drop the temporary table and commit the transaction.
DROP TABLE tmp;COMMIT TRANSACTION;
```

**单据:** [多对帐单交易](https://cloud.google.com/bigquery/docs/reference/standard-sql/transactions?hl=en)

# 9.动态 SQL

动态执行动态 SQL 语句。

```
DECLARE y INT64;-- y = 1 * (3 + 2) = 5
EXECUTE IMMEDIATE "SELECT ? * (? + 2)" USING 1, 3;-- y = 1 * (3 + 2) + 3 = 8
EXECUTE IMMEDIATE "SELECT [@a](http://twitter.com/a) * ([@b](http://twitter.com/b) + 2) + [@b](http://twitter.com/b)" INTO y USING 1 as a, 3 as b;SELECT y;
```

**文档:** [立即执行](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting?hl=en#execute_immediate)

# 10.基于列值生成行 ID

```
WITH produce AS
 (SELECT 'kale' as item, 23 as purchases, 'vegetable' as category
  UNION ALL SELECT 'kale', 23, 'vegetable' 
  UNION ALL SELECT 'kale', 23, 'vegetable' 
  UNION ALL SELECT 'orange', 2, 'fruit'
  UNION ALL SELECT 'cabbage', 9, 'vegetable'
  UNION ALL SELECT 'apple', 8, 'fruit'
  UNION ALL SELECT 'leek', 2, 'vegetable'
  UNION ALL SELECT 'lettuce', 10, 'vegetable')

SELECT 
  *,
  TO_JSON_STRING(t) AS json_from_columns,
  FARM_FINGERPRINT(TO_JSON_STRING(t)) AS rowid_from_columns
FROM produce AS t
```

**文档:**[TO _ JSON _ STRING](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#to_json_string)；[农场 _ 指纹](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#farm_fingerprint)

# 11.调试语句

评估表达式。如果表达式的计算结果为`TRUE`，则该语句成功返回，没有任何结果。如果表达式的计算结果为`FALSE`或`NULL`，该语句将生成一个错误。如果存在描述，描述将出现在错误消息中。

```
ASSERT
(SELECT COUNT(*) FROM UNNEST([1, 2, 3, 4, 5, 6])) > 5
AS 'Table must contain more than 5 rows.';ASSERT EXISTS(
  SELECT X
  FROM UNNEST([7877, 7879, 7883, 7901, 7907]) X
  WHERE X = 7919
) AS 'Column X must contain the value 7919';
```

**文档:**断言

# 12.创建外部表

外部表允许 BigQuery 查询存储在 BigQuery 存储之外的数据。

```
CREATE OR REPLACE EXTERNAL TABLE my_dataset.ext_produce 
(
  item STRING,
  purchases INT64,
  category STRING,
  path STRING
)
OPTIONS (
  format = 'CSV',
  uris = ['gs://my-bucket/file1.csv', 'gs://my-bucket/file2.csv']
);SELECT * 
FROM `my_dataset.ext_produce`
```

**文档:**创建外部表语句

# 13.导出数据

将查询结果导出到外部存储位置。支持的格式包括:AVRO，CSV，JSON，拼花。

```
EXPORT DATA OPTIONS(
  uri='gs://my-bucket/my-folder/*.json',
  format='JSON',
  overwrite=true) AS
SELECT * 
FROM `my-project.my-dataset.my-table`
```

**文档:** [导出数据报表](https://cloud.google.com/bigquery/docs/reference/standard-sql/other-statements?hl=en#export_data_statement)

# 14.创建和执行 ML 模型

```
#CreateModel
CREATE OR REPLACE MODEL `my_dataset.sample_model`
OPTIONS(model_type='logistic_reg') AS
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX = '20160801';

#EvaluateModel
SELECT
  *
FROM
  ML.EVALUATE(MODEL `my_dataset.sample_model`, (
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'));

#PredictOutcomes
SELECT
  country,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ML.PREDICT(MODEL `my_dataset.sample_model`, (
SELECT
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(totals.pageviews, 0) AS pageviews,
  IFNULL(geoNetwork.country, "") AS country
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'))
GROUP BY country
ORDER BY total_predicted_purchases DESC
LIMIT 10
```

**文档:** [ML 文档](https://cloud.google.com/bigquery-ml/docs)；[快速启动](https://cloud.google.com/bigquery-ml/docs/bigqueryml-web-ui-start)； [ML 教程](https://cloud.google.com/bigquery-ml/docs/tutorials)

# 15.分析和可视化地理空间数据

地理空间分析允许您通过使用地理数据类型和标准 SQL 地理函数来分析和可视化 BigQuery 中的地理空间数据。要可视化地理位置数据，您可以使用:

*   谷歌数据工作室
*   BigQuery Geo Viz
*   谷歌地球引擎
*   Jupyter 笔记本

[BigQuery Geo Viz 示例](https://user-images.githubusercontent.com/3390857/129909651-8d19afda-68c3-4efb-83a3-c5b2d6bc5c7c.png)

**文档:**[big query GIS](https://cloud.google.com/bigquery/docs/gis)； [BigQuery GIS 教程](https://cloud.google.com/bigquery/docs/gis-tutorial-hurricane#run_a_standard_sql_query_on_data)；[可视化地理空间数据](https://cloud.google.com/bigquery/docs/geospatial-visualize)