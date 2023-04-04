# BigQuery:数据海洋中强大的统一分析接口

> 原文：<https://blog.devgenius.io/bigquery-the-powerful-unified-analytics-interface-in-the-data-ocean-c3778eb64723?source=collection_archive---------11----------------------->

> 继 [G Cloud Day 2022](https://gcloud.devoteam.com/pt-pt/devoteam-g-cloud-day-2022/) 事件之后，[奉献者 G Cloud](https://gcloud.devoteam.com/) 博客上发布的[葡萄牙语原文](https://gcloud.devoteam.com/pt-pt/blog/bigquery-a-poderosa-interface-analitica-unificada-no-data-ocean/)英文版。

![](img/f2986a848e06e10afdcf5e7e4289597e.png)

瑞安·拉夫林在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

是的，我说的是**数据海洋**而不是[数据仓库](https://en.wikipedia.org/wiki/Data_warehouse)或[数据湖](https://en.wikipedia.org/wiki/Data_lake)！这是因为两者都是通常局限于某个领域的概念，比如一个组织，目前仅限于 [BigQuery](https://cloud.google.com/bigquery) 功能。

在一个数据海洋中，我们在一个无界的空间中拥有各种各样的数据，它们以不同的方式排列，或者说杂乱无章，并不容易以一种分析的方式使它们变得可访问。

谷歌一直在努力扩展 BigQuery 的功能，使其越来越成为任何存储层上的统一分析界面，无论数据的格式和位置如何。拥有一个[架构](https://cloud.google.com/blog/products/bigquery/separation-of-storage-and-compute-in-bigquery)，其中[存储](https://cloud.google.com/bigquery/docs/storage_overview)和[计算](https://cloud.google.com/bigquery/docs/query-overview)是分开的，这是利用该策略的一个重要因素。

最近， [BigLake](https://cloud.google.com/blog/products/data-analytics/unifying-data-lakes-and-data-warehouses-across-clouds-with-biglake) 在[Google Data Cloud Summit 2022](https://cloudonair.withgoogle.com/events/summit-data-cloud-2022?talk=vod_da1_s1_big_lake)上宣布，这是这种投资的一个例子，他们正在扩展 BigQuery，以在多云环境中通过细粒度治理来统一数据仓库和湖泊。

这是一个强大的成就，因为它消除了所有的数据边界，打破了数据湖和数据仓库之间的壁垒。无需在对象存储之间复制或移动数据，只需从一个位置访问所有数据。

例如，它还允许公司之间的合作伙伴共享战略信息，而无需在单个分析平台上以完全受控和安全的方式在不同存储之间移动信息。

这是谷歌最近宣布的消息之一，但围绕 BigQuery 的**分析生态系统**远不止于此，如下所示:

# **特色**

[**BigQuery ml**](https://cloud.google.com/bigquery-ml/docs/)**:**让我们在 BigQuery 中构建并运行机器学习模型。与 [Vertex AI](https://cloud.google.com/vertex-ai) 和 [TensorFlow](https://cloud.google.com/tensorflow-enterprise) 的集成允许在几分钟内训练和运行结构化数据上的强大模型。所有这一切都只需要 SQL。

[**BigQuery GIS**](https://cloud.google.com/bigquery/docs/geospatial-intro) :独特地将 BigQuery 的无服务器架构与使用标准 SQL 的地理空间分析的原生支持相结合。

[**BigQuery Omni**](https://cloud.google.com/bigquery-omni/docs) :一个灵活的多云分析解决方案，由 [Anthos](https://cloud.google.com/anthos) 提供支持，允许跨云分析数据。使用标准 SQL 和 BigQuery 熟悉的界面。

[**BigQuery BI Engine**](https://cloud.google.com/bigquery/docs/bi-engine-intro):与 big query 集成的内存分析服务，允许用户以亚秒级查询响应时间和高并发性交互分析大型复杂数据集。

[**BigQuery 数据传输服务**](https://cloud.google.com/bigquery-transfer/docs/introduction) :自动将外部数据源的数据，如谷歌营销平台、谷歌广告、YouTube、合作伙伴 SaaS 应用程序、Teradata 和亚马逊 S3，在预定和完全管理的基础上传输到 BigQuery。

[**连接的工作表**](https://cloud.google.com/blog/products/g-suite/connected-sheets-is-generally-available) :这允许用户在不需要 SQL 知识的情况下，分析 Google Sheets 中数十亿行实时 BigQuery 数据。

[**BigLake**](https://cloud.google.com/biglake) :一个存储引擎，允许统一数据仓库和湖泊，使它们能够执行统一的细粒度访问控制，并加速跨多云存储和开放格式的查询性能。来自[外部数据源](https://cloud.google.com/bigquery/external-data-sources)的连接对于 BigQuery 来说并不陌生，因为已经可以创建[外部表](https://cloud.google.com/bigquery/docs/external-tables)或[联邦查询函数](https://cloud.google.com/bigquery/docs/reference/standard-sql/federated_query_functions)。

[**在 BigQuery**](https://cloud.google.com/bigquery/docs/search-intro) 中搜索:这让你可以使用 Google 标准 SQL 轻松找到埋藏在非结构化文本和半结构化 JSON 数据中的唯一数据元素，而不必事先知道表模式。

[允许用 SQL 和 Javascript 之外的其他语言实现函数，或者用 BigQuery 用户定义函数中不允许的库或服务实现函数。](https://cloud.google.com/bigquery/docs/reference/standard-sql/remote-functions)

[**标准 SQL 中的 JSON 数据**](https://cloud.google.com/bigquery/docs/reference/standard-sql/json-data) : BigQuery 标准 SQL 现在支持 JSON 数据类型来存储 JSON 数据。这意味着您可以将半结构化的 JSON 接收到 BigQuery 中，而无需预先为 JSON 数据提供模式。这允许您存储和查询不总是遵循固定模式和数据类型的数据。

[**BigQuery connector for SAP**](https://cloud.google.com/blog/products/data-analytics/bigquery-connector-for-sap):一种快速、简单、经济高效且可大规模扩展的方式，通过利用客户现有的 SAP Landscape Transformation Replication Server(SLT)工具和技能组合，使 SAP 数据在 big query 中完全可访问。**目前免费提供**。

[**公共数据集**](https://cloud.google.com/datasets) :提供来自不同行业的 200 多个高需求公共数据集的强大数据存储库。

[**BigQuery 沙箱**](https://cloud.google.com/bigquery/docs/sandbox) :始终自由访问 BigQuery 的全部功能，但受某些限制。不需要信用卡，也不需要创建或启用账单账户。

[**免费使用层**](https://cloud.google.com/bigquery/pricing#free-tier) :作为 [Google Cloud 免费层](https://cloud.google.com/free)的一部分，BigQuery 提供了一些特定限制的免费资源。这些免费使用限制在免费试用期期间和之后可用。

# **相关解决方案**

[**Serverless Spark through BigQuery**](https://docs.google.com/forms/d/e/1FAIpQLSccIXlE5gJNE0dNs6vQvCfrCcSjnoHqaW2lxpoVkAh56KLOwA/viewform):py Spark 编辑器进入 big query 控制台，由 server less Spark 后端驱动。找到相关信息[在这里](https://cloud.google.com/blog/products/data-analytics/making-serverless-spark-even-more-powerful)和[在这里](https://cloud.google.com/blog/products/data-analytics/simplify-data-processing-and-data-science-jobs-with-spark-on-google-cloud)。

[**分析中心**](https://cloud.google.com/analytics-hub):big query 数据集的发布和订阅模型。支持组织间高效、安全的数据资产交换，以应对数据可靠性和成本挑战。

[**Google Cloud Cortex Framework**](https://cloud.google.com/solutions/cortex):一套模板、构建模块和参考架构，将简化和加速 Google Cloud 平台上的规划、工程和部署。借助分析参考架构、预定义的 BigQuery 视图和变更数据捕获脚本、BigQuery ML 模板以及即插即用的 Looker 仪表盘，您可以使用 BigQuery 中的 [SAP 数据快速启动 insights。](https://cloud.google.com/solutions/sap)

[**Dataplex**](https://cloud.google.com/dataplex) :一种智能数据结构，有助于统一分布的数据，无需任何数据移动，根据业务需求组织数据，并进行集中管理、监控和治理。

[**Vertex AI** :](https://cloud.google.com/vertex-ai/docs) 将 [AutoML](https://cloud.google.com/automl) 和 [AI 平台](https://cloud.google.com/ai-platform/docs/technical-overview)集合到一个统一的 API、客户端库和用户界面中。还有 [Vertex AI Workbench](https://cloud.google.com/vertex-ai-workbench) ，一个用于整个数据科学工作流程的单一开发环境，以及 [Vertex AI Model Registry](https://cloud.google.com/vertex-ai/docs/model-registry/introduction) ，一个管理和治理 ML 模型生命周期的中央存储库。

[](https://cloud.google.com/dlp/docs)****【DLP】**:完全托管的服务，提供对强大的敏感数据检查、分类和去识别平台的访问。有像 BigQuery data 的 [Data profiles](https://cloud.google.com/dlp/docs/data-profiles) 这样的服务，可以自动扫描整个组织、个人文件夹和项目中的所有表和列，识别敏感和高风险数据所在的位置。你可以在这里和这里找到更多关于 BigQuery [自动 DLP 的信息。](https://cloud.google.com/blog/products/identity-security/automatic-dlp-for-bigquery)**

**[**日志分析**](https://cloud.google.com/logging/docs/log-analytics) :通过针对分析日志数据而优化的新用户界面，直接在云日志中为您提供 BigQuery 的分析能力。您可以使用 SQL 来执行高级日志分析，还可以在 BigQuery 中直接使用您的日志数据。您可以将您的日志与存储在 BigQuery 中的其他业务数据相关联，从而让您更全面地了解您的 Google 云服务。**

**[**主动协助 BigQuery 推荐**](https://cloud.google.com/blog/products/data-analytics/google-cloud-launches-bigquery-capacity-recommendations) :为使用按需计费的客户创建推荐。这些建议有助于您理解您的 BigQuery 容量需求以及购买不同数量的插槽容量的成本和性能权衡。这项功能由 [Active Assist](https://cloud.google.com/solutions/active-assist) 提供支持，这是谷歌云 AIOps 解决方案的一部分，该解决方案使用数据、智能和机器学习来降低云复杂性和管理工作。**

# ****附加主题****

**[**数据安全和治理**](https://cloud.google.com/bigquery/docs/data-governance) :数据治理概念以及保护 BigQuery 资源可能需要的控制措施。**

**[**BigQuery Admin 参考指南**](https://cloud.google.com/blog/search;query=bigquery%20admin%20reference%20guide;paginate=25;order=newest) :一组文章，涵盖监控、治理、查询处理和优化、资源层次结构等。**

**[**Google Cloud Ready-BigQuery**](https://cloud.google.com/bigquery/docs/bigquery-ready-overview):一个验证程序，Google Cloud 工程团队通过使用一系列数据集成测试和基准来评估和验证 big query 集成和连接器。**

**[**行业解决方案**](https://cloud.google.com/solutions#section-2) :谷歌云解决方案可以帮助提高效率和敏捷性，降低成本，参与新的商业模式，捕捉新的市场机会。BigQuery 是这些分析解决方案的重要组成部分。**

**[**将数据仓库迁移到 BigQuery**](https://cloud.google.com/architecture/dw2bq/dw-bq-migration-overview) :本文是帮助您从本地数据仓库迁移到 BigQuery 的系列文章的一部分。后面一节将详细介绍如何从特定的数据仓库技术迁移到 BigQuery，比如 Netezza、Oracle、Amazon Redshift、Teradata 和 Snowflake。**

# ****结论****

**这些只是 BigQuery 特性和解决方案的一个子集，并不是一个详尽的列表。BigQuery 与谷歌云服务及其他服务完全集成，符合谷歌的[开放云](https://cloud.google.com/open-cloud)战略，越来越开放和民主，例如我们可以通过 BigLake 和 BigQuery Omni 看到。**

**你可以在谷歌云[产品](https://cloud.google.com/products)、[解决方案](https://cloud.google.com/solutions)、[博客](https://cloud.google.com/blog)和[文档](https://cloud.google.com/docs)中找到更多信息，例如 [BigQuery 文档](https://cloud.google.com/bigquery/docs)、 [BigQuery 发行说明](https://cloud.google.com/bigquery/docs/release-notes)、 [Dataproc 无服务器 Spark](https://cloud.google.com/dataproc-serverless/docs) 和[谷歌云上的 Spark](https://cloud.google.com/solutions/spark)。**