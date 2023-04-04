# 数据仓库数据建模

> 原文：<https://blog.devgenius.io/data-warehousing-data-modeling-5a18c85943c3?source=collection_archive---------0----------------------->

## 维度建模，数据仓库建模

# 什么是数据建模

在软件行业，[信息表示](https://cs.wellesley.edu/~cs115/readings/information-representation.html)(数字、文本、图片、声音、电影、关系)是关键。最后，它与我们存储、处理和移动信息的效率有关。类似地，数据表示在数据项目中更加重要。通常，在数据分析领域，我们有[数据湖和数据仓库](https://www.qlik.com/us/data-lake/data-lake-vs-data-warehouse) ( [另一个环节](https://www.adaltas.com/en/2022/05/17/data-warehouse-lake-lakehouse-comparison/))来存储、处理数据。今天，我们将重点讨论数据仓库和数据建模。

[](https://vertabelo.com/blog/why-do-you-need-data-modeling/) [## 为什么需要数据建模？

### 您需要数据建模来为您自己或您的组织节省大量的金钱、时间和问题。请继续阅读，了解如何…

vertabelo.com](https://vertabelo.com/blog/why-do-you-need-data-modeling/) [](https://www.phdata.io/blog/how-to-model-and-choose-the-right-data-model/) [## 什么是数据建模，我如何选择正确的数据建模？

### 揭示数据建模的来龙去脉及其重要性，并了解如何选择正确的数据模型…

www.phdata.io](https://www.phdata.io/blog/how-to-model-and-choose-the-right-data-model/) [](https://medium.com/@bojanciric/data-modeling-crash-course-43c73eb9377f) [## 数据建模速成班

### 数据建模是一门成熟的学科，并且仍然是每个数据计划的关键。本文的目标是…

medium.com](https://medium.com/@bojanciric/data-modeling-crash-course-43c73eb9377f) 

# 实体关系模型

数据仓库通常服务于数据可视化、BI 和数据分析的用例。这些工具通常使用 SQL 来查询数据，并且通常它们显示一些聚合(例如，总和、平均值、最小值、最大值)。我们通常使用实体关系模型(或表格模型，绝对不是[图形模型](https://medium.com/mlearning-ai/graph-databases-amazon-neptune-bb89582abcf7))来与 SQL 一起工作来表示数据。

[](https://medium.com/sagar-explains-azure-and-analytics-data-engineerin/introduction-to-data-modelling-c0c44432ec0b) [## 数据建模简介

### 什么是数据建模？

medium.com](https://medium.com/sagar-explains-azure-and-analytics-data-engineerin/introduction-to-data-modelling-c0c44432ec0b) 

# OLTP 与 OLAP

在进入数据仓库数据建模之前，我们需要注意 OLTP 和 OLAP 之间的区别，两者都使用实体关系模型。然而，用例是不同的:OLTP 用于事务性工作负载，它侧重于单个记录的读/写；而 OLAP 用于分析工作负载，侧重于处理大量数据。尽管它们通常都使用实体关系模型，但还是有重要的区别: [OLTP 通常使用规范化的表，而 OLAP 通常使用非规范化的表](https://techdifferences.com/difference-between-normalization-and-denormalization.html)，因为规范化的设计更有助于减少存储冗余，而非规范化的设计有助于降低连接和查询性能。

[](https://www.stitchdata.com/resources/oltp-vs-olap) [## OLTP 和 OLAP:一个实用的比较

### OLTP 和 OLAP:这两个术语看起来相似，但指的是不同种类的系统。在线事务处理(OLTP)…

www.stitchdata.com](https://www.stitchdata.com/resources/oltp-vs-olap) [](https://ashutosh-bitmesra.medium.com/oltp-and-olap-what-are-the-differences-a6e21f25bfe0) [## OLTP 和 OLAP？有什么区别？

### 还记得第一次被介绍两个流行语的时候，OLTP 和 OLTP 我觉得很困惑…

ashutosh-bitmesra.medium.com](https://ashutosh-bitmesra.medium.com/oltp-and-olap-what-are-the-differences-a6e21f25bfe0) 

OLTP -> [3NF](https://www.matillion.com/resources/blog/data-vault-vs-star-schema-vs-third-normal-form-which-data-model-to-use) ，快速写入，更新密钥级联到子挑战； [OLAP](https://www.talend.com/resources/data-model-design-best-practices-part-1/) - >维度建模:快速读取，低粒度挑战

# 维度建模与数据仓库

数据仓库通常集中在 OLAP，服务于分析工作负载。数据建模方法论通常包括 [Inmon 方法](https://www.astera.com/type/blog/data-warehouse-concepts/)、 [Kimball/dimensional 方法](https://medium.com/cloudzone/inmon-vs-kimball-the-great-data-warehousing-debate-78c57f0b5e0e)、data vault 甚至[宽表](https://robertsahlin.com/inmon-vs-kimball-vs-data-vault-vs-wide-tables/)。在 BI/Reporting 时代，Kimball 方法盖过了 Inmon 方法，因为它可以更顺利地满足最终用户的需求，宽表更像是在拥有单个数据集后的性能增强。我们将在本文中介绍 Kimball/dimensional 和 data vault。

## 维度建模

数据集市

[](https://towardsdatascience.com/star-schema-924b995a9bdf) [## 事实与维度表

### 在星型模式和数据仓库的上下文中理解事实表和维度表之间的区别

towardsdatascience.com](https://towardsdatascience.com/star-schema-924b995a9bdf) [](https://medium.com/nerd-for-tech/fundamental-data-modeling-and-data-warehousing-b599183d998a) [## 基础数据建模和数据仓库

### 什么是数据建模？

medium.com](https://medium.com/nerd-for-tech/fundamental-data-modeling-and-data-warehousing-b599183d998a) [](https://www.holistics.io/books/setup-analytics/kimball-s-dimensional-data-modeling/) [## 金博尔的维度数据建模|分析设置指南

### 这一部分涵盖了拉尔夫·金博尔和他的同事们的想法，他们在 90 年代发展了这些想法，并发表了数据…

www.holistics.io](https://www.holistics.io/books/setup-analytics/kimball-s-dimensional-data-modeling/) 

维度建模有以下一般工作流程:

1.  识别业务流程:关注业务用户想要什么
2.  识别粒度:决定存储在主事实表中的数据级别
3.  确定维度和属性:业务流程事件的上下文，例如，谁(例如，客户)、什么(例如，产品)、哪里(例如，位置)、何时(例如，销售日期)；属性是维度的各种特征，例如位置、洲/国家/地区/州/城市/邮政编码
4.  识别事实和数字度量:数据集市正在回答什么问题，例如销售数字、交易量

## 数据库

当您的数据集市结构相对固定时(例如，不经常添加新的数据源、添加新的属性),维度建模可以完美地工作。当发生这种情况时，通常意味着刷新数据集市或使用对数据集市来说太复杂的雪花模式。当您构建企业数据仓库，想要集中所有分析数据时，情况通常不是这样。您将不断添加新的数据源，当前的数据源可能会不断添加新的列，因为企业数据仓库是所有其他分析工作负载的基础，所以最好将重点放在灵活性上，这是 data vault 的主要优势。Data vault 主要有 3 个组件:

Hub:核心业务实体，如客户、产品、订单等。Hub 只存储关键信息，如业务实体 ID、源(保持沿袭)、时间戳

链接:中心实体之间的关系，仅包含连接键

卫星:集线器或链路中实体的属性

当添加新列时，您可以只向 Hub 或 Link 添加新的卫星表，并添加新的 ETL 管道(不修改当前的管道逻辑)。然而，代价是当有很多变化时，数据模型会变得越来越复杂，当需要连接这些表时，查询会变得复杂。

[](https://medium.com/@jryan999/data-vault-an-overview-27bed8a1bf9f) [## 数据仓库—概述

### 数据仓库—概述

数据仓库——Overviewmedium.com](https://medium.com/@jryan999/data-vault-an-overview-27bed8a1bf9f) [](https://www.phdata.io/blog/building-modern-data-platform-with-data-vault/) [## 如何利用数据仓库构建现代数据平台

### 构建新的数据湖？考虑使用 data vault 架构来实现最佳业务价值。了解更多关于…

www.phdata.io](https://www.phdata.io/blog/building-modern-data-platform-with-data-vault/) [](https://www.databricks.com/blog/2022/06/24/prescriptive-guidance-for-implementing-a-data-vault-model-on-the-databricks-lakehouse-platform.html) [## 什么是数据仓库，如何在 Databricks Lakehouse 平台上实现它

### 在设计分析系统时，您可以使用许多不同的数据模型，例如特定于行业的…

www.databricks.com](https://www.databricks.com/blog/2022/06/24/prescriptive-guidance-for-implementing-a-data-vault-model-on-the-databricks-lakehouse-platform.html) 

以上文章的 TLDR:数据仓库和数据湖库关系(可用于实现银层)，金层的维度模型(数据集市和报告)

维度建模仍然最适合于分析和报告，并且是业务用户最容易理解的可见模型。Data Vault 更适合大型企业数据仓库，这也是比尔·恩门推荐的，但不适合分析和报告。

Data Vault 更加灵活，更容易添加新的数据源，更容易审计，并且始终保留所有数据，因此您将能够随时重新创建您的数据管理器。

理想的情况可能是将 Data Vault 用于您的企业数据仓库，将维度建模用于您的数据集市。

# data vault 上的高级读取

[](https://infinitelambda.com/data-vault) [## Data Vault 2.0-Infinite Lambda 的云和数据工程

### 适用于可伸缩数据仓库的 Data Vault 2.0:了解建模和组件。看看你什么时候应该选择它…

infinitelambda.com](https://infinitelambda.com/data-vault) [](https://medium.com/hashmapinc/getting-started-with-data-vault-2-0-c19945874fe3) [## Data Vault 2.0 入门

### 我强烈推荐您开始 DV2 之旅

medium.com](https://medium.com/hashmapinc/getting-started-with-data-vault-2-0-c19945874fe3) [](https://medium.com/snowflake/data-vault-mysteries-effectivity-satellite-driving-key-5e96b5aab821) [## 数据保险库之谜…有效性卫星和驱动键

### 在今天的《数据保险库之谜》中，我们将讨论驱动钥匙和有效卫星！

medium.com](https://medium.com/snowflake/data-vault-mysteries-effectivity-satellite-driving-key-5e96b5aab821) [](https://makingdatameaningful.com/data-vault-hubs-links-and-satellites-with-associated-loading-patterns) [## 数据仓库:集线器、链路和卫星以及相关的加载模式——使数据有意义

### 所以我的商业赞助人和高级架构师决定建立一个数据仓库。我们已经认识到并且…

makingdatameaningful.com](https://makingdatameaningful.com/data-vault-hubs-links-and-satellites-with-associated-loading-patterns) [](https://datavaultalliance.com/news/dv/dv-standards/data-vault-2-0-suggested-object-naming-conventions) [## Data Vault 2.0 建议的对象命名约定-datavaultaliance

### 大家好，我们有一个建议命名惯例的列表，这些惯例本质上是最佳实践。有几个…

datavaultalliance.com](https://datavaultalliance.com/news/dv/dv-standards/data-vault-2-0-suggested-object-naming-conventions) [](https://www.2ndwatch.com/blog/3-reasons-implement-data-vault-model-2-reasons-not/) [## 实施数据仓库模型的 3 个理由(以及不实施的 2 个理由)

### 对于任何寻求企业数据仓库或更新报告解决方案的客户，我们首先采取的步骤之一是…

www.2ndwatch.com](https://www.2ndwatch.com/blog/3-reasons-implement-data-vault-model-2-reasons-not/) 

我什么时候不使用 Data Vault？

原因 2:您只有一个源系统和/或相对静态的数据。通常不正确

原因 1:您需要将数据直接加载到报告工具中。可能是不建设 EDW 的小公司的使用案例

# 实际数据仓库建模示例

[](https://aginic.com/blog/modelling-with-data-vaults/) [## 如何实现数据仓库建模

### 维度建模，也称为 Kimball 的星形模式，长期以来一直是构建数据模型的行业标准

aginic.com](https://aginic.com/blog/modelling-with-data-vaults/) [](https://www.scalefree.com/scalefree-newsletter/data-vault-2-0-with-dbt-part-1/) [## 带 dbt 的 Data Vault 2.0 第 1 部分|咨询和培训专家

### 数据是决策过程中的重要资产。正如我们之前在另一篇文章《数据仓库》中讨论的那样…

www.scalefree.com](https://www.scalefree.com/scalefree-newsletter/data-vault-2-0-with-dbt-part-1/) [](https://www.scalefree.com/scalefree-newsletter/data-vault-2-0-with-dbt-part-2/) [## 带 dbt 的 Data Vault 2.0 第 2 部分|咨询和培训专家

### 在这个博客系列的第一部分中，我们向您介绍了 dbt。现在让我们来看看如何实现数据…

www.scalefree.com](https://www.scalefree.com/scalefree-newsletter/data-vault-2-0-with-dbt-part-2/)