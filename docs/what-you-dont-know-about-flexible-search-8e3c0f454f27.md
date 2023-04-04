# 关于灵活搜索，你不知道的是

> 原文：<https://blog.devgenius.io/what-you-dont-know-about-flexible-search-8e3c0f454f27?source=collection_archive---------5----------------------->

## 如何编写有用的灵活搜索查询

![](img/9abb9dc174865cd698159f3a3a426031.png)

由[卢克·切瑟](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 什么是灵活搜索？

> FlexibleSearchService 是一个基于 SQL 的项目类型搜索，它允许对属性值以及直接对数据库列进行搜索。与 SQL 语句不同，FlexibleSearch 查询可以包含对 hybris 平台中项目(产品、单位、客户等)的引用，作为查询语句的一部分(标记为 **{** 和 **}** )。来自 help.sap.com

Hybris 开发人员经常使用灵活的搜索，所以了解它很重要。灵活的搜索为 hybris 模型的内部工作提供了一个细粒度的接口。生成的 SQL 查询相当复杂，所以开发人员不必学习这些。

如果没有灵活的搜索查询，创建新的 [Solr](https://lucene.apache.org/solr/) 索引或创建新的 [DAO](https://www.baeldung.com/java-dao-pattern) 是不可能的。提取模型间条件的模型，可以用这个特性来完成，因为 BackOffice 不能提供这个功能。

灵活搜索查询的执行是在 hybris 管理控制台中完成的。[如果要在 DAO 中使用，需要使用 FlexibleSearchService](https://help.sap.com/doc/a4265d5ea8314eb2929e6cf6fb8e35a5/1811/en-US/de/hybris/platform/servicelayer/search/FlexibleSearchService.html) 。

# 获取项目

要在 Java 中使用它，您需要创建一个查询字符串。把它作为灵活搜索服务的一个论据。

```
```Java
String query = "SELECT {" + UnitModel.PK + "} FROM {" + UnitModel._TYPECODE + "}`
```
```

要获取类型的实体，需要创建这样的查询。在这个查询中，我们正在获取类型为`YourCustomer`的每个属性的条目，在`userName`中有一个感叹号。

```
SELECT * FROM {YourCustomer} WHERE {userName} LIKE ‘%!%’
```

# 连接表格

该查询将根据外键将订单与单位连接起来。嵌套查询仅用于获取具有特定属性集的模型。

在测试特定行为时，基于条件连接模型非常有用。这在 Backoffice 中是无法复制的，这是查询的一个优势。

```
SELECT {o.prop1}, {u.prop2} FROM { Order as o JOIN Unit as u ON {o. unit} = {u.pk} } WHERE {o.unit} IN ({{ SELECT {pk} FROM {Unit} WHERE {prop2} IS NOT NULL}})
```

# 嵌套查询

有时在开发过程中，我们有一些带有剩余外键的模型。模型的外键指向不存在的模型。嵌套查询帮助我找到了这些模型。

首先，我搜索了所有具有外键集的类型，并检查是否有任何具有该键的模型。这样我们就找到了`Type`的现有模型。在搜索不在该集合中的键后，我们得到剩余的外键。

```
SELECT * FROM {Type} WHERE {fKey} NOT IN ({{ SELECT {pk} FROM {AbstractType} WHERE {pk} IN ({{SELECT {fKey} FROM {Type} WHERE {fKey} IS NOT NULL}}) }})
```

感谢您阅读这篇文章。