# 了解联邦查询引擎

> 原文：<https://blog.devgenius.io/understanding-federated-query-engines-624c57435118?source=collection_archive---------0----------------------->

![](img/c7b3ec7cf5f85d7147cbbf525e4ed6ec.png)

[**照片**](https://unsplash.com/photos/dSyhpTGhNHg) **被** [](https://unsplash.com/@karlp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)[**迈克尔·捷兹奇**](https://unsplash.com/@lazycreekimages) **踩不溅**

您是数据分析师、数据科学家还是数据工程师，想知道联邦查询引擎是否可以帮助您处理存储在不同类型数据库中的数据？想知道它们如何有用吗？那么，这篇文章将帮助你做到这一点。

让我们从理解什么是联邦开始，

每一类数据库(关系数据库、非关系数据库和对象数据库)都需要查询数据，以生成组织可以采取行动的业务洞察。标准查询语言(SQL)用于从数据库或数据仓库中提取数据。

编写 SQL 查询是为了访问数据。在日常活动中，数据来自不同的来源，如社交媒体、客户反馈或财务报告。为了处理这个问题，我们需要编写 SQL 脚本，为特定客户从所有这些来源获取数据，并将数据放入一个集中的存储库中，商业智能分析师可以轻松访问和使用该存储库来生成报告。

在这种情况下，编写查询会变得很棘手。这就是联邦查询概念的由来。**联合查询是连接来自不同数据集的表的查询**。听起来像是最佳实践，对吗？

让我们尝试得到一个更简单的联邦查询定义。根据 AWS，联邦查询是一种“使数据分析师、工程师和数据科学家能够对存储在关系、非关系、对象和定制数据源中的数据执行 SQL 查询”的能力。

联邦查询引擎在一个完全解耦的架构中运行，计算在一边，存储在另一边，网络在中间。去耦架构提供了许多优势。

下图说明了联邦查询引擎的这个过程。

![](img/e172281b988b9df861c987fd135f7ae7.png)

[来源](https://research.tableau.com/sites/default/files/fugu-aidm19-camera-ready.pdf)

当我们谈论这些联邦查询引擎时，您可能会问自己:为什么使用联邦查询引擎而不是数据仓库？请允许我对此作一个简短的回应。

与联邦引擎相比，优化的数据仓库提供了更快的性能。请注意，我说的是“优化”。优化的数据仓库更好的原因是联邦引擎不管理自己的数据。简而言之，要提高系统性能，您需要管理来自源的数据接收、管理存储以及最终用户对数据的访问。此外，优化的数据仓库更适合临时任务(为特定目的完成的任务，没有早期的计划)。

请记住，我们需要使用云数据仓库，因为我们需要存储大数据。[云数据仓库](https://www.firebolt.io/blog/cloud-data-warehouse)是一个集中的存储库，用于存储云中的报告数据。不同云提供商的数据仓库示例有 AWS Redshift、Microsoft Azure SQL 数据仓库、Google BigQuery 和雪花。

让我们讨论一下关于其中两个产品的联邦查询的使用:Amazon Redshift 和 BigQuery。

# Amazon 红移联邦查询

为了尝试这个，第一件事就是创建一个 [AWS](https://aws.amazon.com/console) 账户，如果你还没有的话。架构仍然是关键组件之一。

![](img/f0efa346b6c1a5f40b24ce42d997ef3c.png)

Amazon 包含了一个查询优化器，用于确定执行联邦查询的最有效方式。为了提高查询性能，Redshift 会将查询的一部分直接传播到目标数据库中。这种方法最大限度地降低了通过系统传输大量数据的风险。

说到数据湖，我们还需要考虑 AWS 联邦查询，并设计一个架构良好的数据湖，以确保性能更快，并在云中产生最小的成本。

您还可以[使用 AWS Athena](https://youtu.be/FRfbwu3m-kM) 通过 AWS Lambda(无服务器)执行这些查询。

# BigQuery 联邦查询

Google Big Query 是另一个联合数据仓库，使用云 SQL 作为数据库。在这种情况下，总是编写查询来从外部数据库提取数据，并将结果作为临时表返回。下面是一个谷歌大查询的例子。

```
SELECT * FROM database(external_query)(“test-fedquery-mysql”, “SELECT cust, MIN(order date) AS first_dateFROM ordersGROUP BY cust;”);The query above is used to connect with CloudSQL in Google Cloud.In GCP we use CloudSQL and cloud spanner as our external connection api to the Big Query.
```

要了解更多关于大查询的真实例子，请查看谷歌云。

# 结论

在本文中，我们已经了解了什么是联邦查询引擎，以及为什么我们需要编写这些查询。然后，我们讨论了在云结构上使用 AWS 联邦查询，以及如何编写 Google Cloud 大查询数据仓库联邦查询。正如所观察到的，理解 SQL 很重要，它是所有这些的主干。我们进一步理解了数据仓库和联邦查询引擎之间的区别。

作为一个组织中的数据工程师，您现在可以根据您的用例做出明智的决策，确定创建云数据仓库还是联合查询引擎是您的最佳解决方案。

谢谢你的阅读。希望你喜欢。让我们继续学习吧！