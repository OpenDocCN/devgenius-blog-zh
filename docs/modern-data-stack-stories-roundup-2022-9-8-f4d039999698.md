# 现代数据堆栈故事综述 2022.9

> 原文：<https://blog.devgenius.io/modern-data-stack-stories-roundup-2022-9-8-f4d039999698?source=collection_archive---------5----------------------->

## 有趣的数据工程文章

[数据是新油](https://medium.com/swlh/data-is-the-new-oil-83a03983e594)。随着越来越多的公司利用 AI/ML，数据变得越来越重要，数据工程也是如此。看到这一领域的快速发展是令人兴奋的，我想总结并分享一些我认为对保持这一领域的更新有用的文章。

业界谈论“现代数据堆栈”已经有一段时间了。因此，作为本系列的第一篇文章，我们将在较高的层次上介绍“数据工程”和“现代数据堆栈”。在后面的系列文章中，我们将在不同的组件中讨论更多的技术内容。

# 数据工程

[](https://learningdaily.dev/practical-data-engineering-concepts-and-skills-a165286591f) [## 实用数据工程概念和技能

### 数据工程师是现代数据驱动型企业的中坚力量。他们负责争吵、操纵和…

每日学习](https://learningdaily.dev/practical-data-engineering-concepts-and-skills-a165286591f) 

数据工程的主要职责是数据摄取、数据处理、数据存储、数据可视化。与传统的 ETL、数据库、商业智能相比，现代数据工程师需要利用更多的技术，如 SQL/NoSQL、数据湖/数据仓库/大数据/BI、容器化、云计算、数据管道/编排、数据可视化、编程、DevOps/CI/CD。

[](https://medium.com/@rchang/a-beginners-guide-to-data-engineering-part-i-4227c5c457d7) [## 数据工程初学者指南—第一部分

### 数据工程:数据科学的近亲

medium.com](https://medium.com/@rchang/a-beginners-guide-to-data-engineering-part-i-4227c5c457d7) 

数据工程中的主要技术，ETL，代表提取(数据摄取)、转换(数据处理)、加载(数据存储)。有两种主要 ETL 范式:以编程为中心(使用 Java、Scala、Python)，以 SQL 为中心。

[](https://medium.com/@rchang/a-beginners-guide-to-data-engineering-part-ii-47c4e7cbda71) [## 数据工程初学者指南—第二部分

### 数据建模、数据分区、流程和 ETL 最佳实践

medium.com](https://medium.com/@rchang/a-beginners-guide-to-data-engineering-part-ii-47c4e7cbda71) 

讨论带有维度建模的数据建模(事实表、维度表)、带有气流的数据管道(DAG、操作符、工作流)、ETL 最佳实践。

[](https://towardsdatascience.com/modern-or-not-what-is-a-data-stack-e6e09e74ae7f) [## 现代与否:什么是数据堆栈？

### “现代数据堆栈”简单地解释为一个堆栈

towardsdatascience.com](https://towardsdatascience.com/modern-or-not-what-is-a-data-stack-e6e09e74ae7f) 

本文为核心数据堆栈的组件搭建了舞台:提取、加载、转换、利用/消费、存储、治理，并展示了核心数据堆栈中的一些现代工具。

# 现代数据堆栈

以下是我对“现代数据堆栈”的理解

1.  更多业务应用程序的连接器，而不是支持更多数据源的传统文件、存储、数据库
2.  更加关注治理和可观察性
3.  反向 ETL(传统的 OLTP -> OLAP 用于洞察，现在 OLAP -> OLTP 用于行动。我们可以使用工作流引擎来做吗？逻辑应用程序，阶跃函数，IFTTT。)新区
4.  用于标准化业务关键指标定义的指标存储(可以定义一次，然后在任何地方重用)。新区域
5.  关注 [ELT 而不是](https://rivery.io/blog/etl-vs-elt/)(7-8 年前出现的)。
6.  更加关注 [SQL](https://blog.dataminded.com/why-dbt-will-one-day-be-bigger-than-spark-2225cadbdad0) 意味着市场上有更多的可用资源，这意味着更加关注结构化数据(数据应该是 SQL 能够处理的，例如，如果你需要用 dbt 从 S3 转换到雪花，你需要依赖雪花原生能力[外部表](https://medium.com/slateco-blog/doing-more-with-less-usingdbt-to-load-data-from-aws-s3-to-snowflake-via-external-tables-a699d290b93f)，同样适用于[红移光谱外部表](https://towardsdatascience.com/a-data-warehouse-implementation-on-aws-a96d0e251abd)

## 概观

[](/what-is-the-modern-data-stack-e27b729ef615) [## 什么是现代数据堆栈？

### 解释什么是现代数据堆栈，为什么它对数据专业很重要，以及这种趋势是如何出现在…

blog.devgenius.io](/what-is-the-modern-data-stack-e27b729ef615) 

本文讨论了现代数据栈的历史，从 ETL 到 ELT 的过渡，以及现代数据栈核心的 dbt。此外，作为数据工程师和数据分析师之间的桥梁，分析工程师有了新的角色。分析工程师可以使用大众化工具、SQL 和版本控制，而不是数据工程师(通常是以数据为中心的软件工程师)来完成从摄取、转换、存储到构建分析数据集的所有工作。数据工程师可以专注于更高的价值，例如构建可重用的框架，而不是自己构建数据管道。文末的[链接](https://www.getdbt.com/what-is-analytics-engineering/)描述了一个“现代数据团队”，数据工程师、分析工程师、数据分析师的关系更加清晰。

然而，我不同意文章中关于星型模式不再相关的说法。在不同的场景中，数据仓库、维度建模(将在后面的文章中讨论)、OBT 都可能是相关的。

[](https://atlan.com/modern-data-stack-101/) [## 现代数据堆栈(MDS):组件、架构和工具

### 现代数据栈近来享有许多创新和关注。筛选…可能很有挑战性

atlan.com](https://atlan.com/modern-data-stack-101/) 

这篇文章来自一家现代数据堆栈公司。它列出了数据平台的基本组件:

*   数据摄取:将原始数据从其真实来源提取并加载到中央数据仓库/湖的机制
*   数据转换:对原始数据进行清理、规范化、过滤、连接、建模和汇总以使其更易于理解和查询的过程
*   数据存储(数据仓库/湖):充当组织的所有行为和事务数据的真实历史记录
*   度量层(Headless BI):位于数据模型和 BI 工具之间，允许数据团队跨不同维度声明性地定义度量。它提供了一个 API，将度量计算请求转换成 SQL 查询，并针对数据仓库运行它们
*   BI 工具:数据消费者使用的分析、报告和仪表板工具，用于理解数据并支持组织中的业务决策
*   反向 ETL:将转换后的数据从数据仓库转移到下游系统(如运营、财务、市场营销、CRM、销售，甚至返回到产品中)以促进运营决策的过程
*   编排(工作流引擎):按计划运行数据管道的系统
*   数据管理、质量和治理:通过有效收集和利用元数据来管理数据质量、沿袭、发现、编目、信息安全和数据隐私的总称

## 工具作业

[](https://snowplowanalytics.com/blog/2021/05/12/modern-data-stack/) [## 2021 年的现代数据堆栈

### 在过去的几年里，组织可以用来推动更好决策的数据工具数量激增…

snowplowanalytics.com](https://snowplowanalytics.com/blog/2021/05/12/modern-data-stack/) 

这篇文章是关于通用数据平台的，包括开源的，基于云的数据栈。类别和工具列表比下面的文章更全面。

[](https://continual.ai/post/the-modern-data-stack-ecosystem-spring-2022-edition) [## 现代数据堆栈生态系统:2022 年春季版

### 欢迎来到现代数据堆栈生态系统 2022 年春季版。在本文中，我们将深入探讨…

continual.ai](https://continual.ai/post/the-modern-data-stack-ecosystem-spring-2022-edition) 

这篇文章来自人工智能公司 [continual.ai](https://continual.ai) ，紧随其[第一篇现代数据栈生态系统文章](https://continual.ai/post/the-modern-data-stack-ecosystem-fall-2021-edition)。在数据平台的每个类别中，都有一个包含更详细描述的主要工具列表。

[](https://medium.com/data-manypets/how-manypets-implemented-the-modern-data-stack-35877715c0da) [## 有多少宠物实现了现代数据堆栈

### 使用最新工具构建数据平台，为数据用户提供强大支持

medium.com](https://medium.com/data-manypets/how-manypets-implemented-the-modern-data-stack-35877715c0da) 

讨论了许多 Pets 如何实现他们的现代数据堆栈，包括数据接收、数据仓库、数据建模、商业智能、反向 ETL 以及他们的工具选择和替代方案。你会看到流行的工具在不同的文章中一次又一次地出现(例如 dbt)。

[](https://medium.com/vertexventures/trends-in-the-modern-data-stack-looking-ahead-c4ff8b08f0f1) [## 现代数据堆栈的趋势:展望未来

### 关于数据堆栈的第 2 部分，我们将讨论事物是如何发展的。

medium.com](https://medium.com/vertexventures/trends-in-the-modern-data-stack-looking-ahead-c4ff8b08f0f1) 

本文着眼于现代数据堆栈生态系统，例如行业中的一些范例，如 Snowflake 的仓库优先方法和 Databricks 的 Delta Lake 的湖泊优先方法。一些行业趋势像“自动反馈循环”或更花哨:反向 ETL，数据操作趋势:元数据管理，数据可观察性和质量，数据治理。

[](/open-source-data-stack-for-small-team-c33f148f7d66) [## 面向小型团队的开源数据堆栈

### 构建数据堆栈的工具，帮助您的公司成为数据驱动型企业

blog.devgenius.io](/open-source-data-stack-for-small-team-c33f148f7d66) 

小型团队的开源工具推荐

提取和装载:Airbyte / Fivetran

事件跟踪/流:卡夫卡/汇流

数据仓库:德鲁伊/雪花

数据湖之家:冰山+德雷米奥/数据布里克斯三角洲

转型与建模:DBT /火花

数据验证:远大前程/ SodaSQL

可视化:超集/外观器

分析:崔诺

反向 ETL: Grouparoo / High touch

数据观察和编目:Datahub / Amundsen / OpenMetadata

编排:气流

[](https://medium.com/codex/modern-data-stack-for-startups-f4ca4693b2db) [## 面向初创企业的现代数据堆栈

### 这就是我们构建数据堆栈的方式。如果你是一个关心所有人的数据访问的组织，遵循这个指南，我们…

medium.com](https://medium.com/codex/modern-data-stack-for-startups-f4ca4693b2db) 

与上面类似的文章，也适用于小型团队/创业公司，不像上一篇文章那样全面，但是提到了更多的工具

提取和加载:缝合

可视化:元数据库

[](https://towardsdatascience.com/bootstrap-a-modern-data-stack-in-5-minutes-with-terraform-32342ee10e79) [## 使用 Terraform 在 5 分钟内引导一个现代数据堆栈

### 设置 Airbyte、BigQuery、dbt、Metabase 以及使用 Terraform 运行现代数据堆栈所需的一切。

towardsdatascience.com](https://towardsdatascience.com/bootstrap-a-modern-data-stack-in-5-minutes-with-terraform-32342ee10e79) 

这篇文章分享了一个 Git repo，它使用 Terraform 在 [Google Cloud](https://medium.com/u/4f3f4ee0f977?source=post_page-----f4d039999698--------------------------------) 、Airbyte、Airflow、Metabase(这三个都作为 docker 容器在 Google 云计算引擎上运行)上提供流行的现代数据堆栈，并使用 GCP BigQuery 作为存储/数据仓库，使用 dbt cloud 来避免基础架构。

[](https://towardsdatascience.com/the-future-of-the-modern-data-stack-in-2022-4f4c91bb778f) [## 2022 年现代数据堆栈的未来

### 介绍 2021 年你应该知道的 6 大想法

towardsdatascience.com](https://towardsdatascience.com/the-future-of-the-modern-data-stack-in-2022-4f4c91bb778f) 

本文讨论了现代数据堆栈中的 6 个顶级概念。

数据网格:这是一种设计原则，侧重于将数据所有权分散回领域专家、自助式数据平台、数据即产品、联合计算治理。不要武断地看待它(就像不是所有的事情都会发生在分权的区块链)。

度量层:现有的 BI 工具并没有真正地将外部度量层集成到它们的工具中。dbt 是事实上的现代数据堆栈转换工具，凭借其对指标层的支持，它将鼓励至少现代 BI 工具深入集成到 dbt 指标 API 中，这可能会给较大的 BI 参与者带来竞争压力。

逆向 ETL:解决将洞察力转化为行动的“最后一英里”问题。然而，这是一个有争议的问题，是否反向 ETL 应该是自己的空间，或者只是与一个数据集成工具相结合。

主动元数据和第三代数据目录:传统元数据是被动的，不会导致行动。[活动元数据](https://towardsdatascience.com/the-gartner-magic-quadrant-for-metadata-management-was-just-scrapped-d84b2543f989)支持更多嵌入式协作，第三代数据目录利用活动元数据支持协作(例如共享查询、共享数据)

作为产品团队的数据团队:数据团队应该像软件产品团队一样，然而，现实是[只有 27%的数据项目是成功的](https://www.capgemini.com/gb-en/resources/cracking-the-data-conundrum-how-successful-companies-make-big-data-operational/)

数据可观察性:可观察性在数据和应用软件领域都很热门(然而，它不仅仅意味着被动的监控，而是可操作的洞察力)。有争议的是，数据可观测性是将继续作为它自己的类别存在，还是将被合并到一个更广泛的类别中(如活动元数据或数据可靠性

# 数据流平台

现代数据栈已经使普通人的工具民主化了很多，然而，纯粹的现代数据栈仍然倾向于批处理。人们总是希望更快地获得数据，并且有许多努力来统一批处理和流式传输，例如 [Kafka](https://www.confluent.io/blog/unifying-stream-processing-and-interactive-queries-in-apache-kafka/) ，但迄今为止，核心堆栈和工具仍然不同。

[](https://thenewstack.io/streaming-data-and-the-modern-real-time-data-stack/) [## 流数据和现代实时数据堆栈

### Shruti 是 Rockset 的首席产品官和营销高级副总裁。在 Rockset 之前，她负责产品…

thenewstack.io](https://thenewstack.io/streaming-data-and-the-modern-real-time-data-stack/) 

现代实时数据堆栈范例不同于批处理，尤其是在数据转换方面。关键层是(不同于批处理):

事件和 CDC 流:除了实时点击流和物联网传感器数据，像 [Debezium](https://debezium.io/) 这样的工具可以将大量 OLTP 数据库变成实时数据源

实时 ETL(或 ELT)服务:Apache Spark、Apache Flink 都提供批处理和流处理能力。有意思，dbt 也列在这里

实时分析数据库:传统的分析数据库是为批量摄取而设计的。但是，也有像 Rockset 这样的工具作为实时分析数据库。此外，主要的云分析数据库还增加了实时摄取功能， [1](https://docs.microsoft.com/en-us/azure/synapse-analytics/data-explorer/ingest-data/data-explorer-ingest-data-streaming) 、 [2](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-streaming-ingestion.html) 、 [3](https://cloud.google.com/bigquery/docs/samples/bigquery-table-insert-rows)

[](https://medium.com/analytics-and-data/real-time-data-pipelines-complexities-considerations-eecad520b70b) [## 实时数据管道——复杂性和注意事项

### 向实时数据流的转变对应用程序的设计方式和数据工作产生了重大影响。

medium.com](https://medium.com/analytics-and-data/real-time-data-pipelines-complexities-considerations-eecad520b70b) 

这篇文章列举了实时流数据接收、处理、存储和服务过程中的复杂性。

模式验证:像[融合模式注册表](https://docs.confluent.io/platform/current/schema-registry/index.html)这样的工具支持摄取期间与 Kafka 的实时数据模式验证集成

数据验证:可以在不同的层完成，如在原始源(符合期望值和/或业务逻辑)、在接收时(模式是预期的，并且数据平台没有接收到意外事件)，以及在接收后(参照完整性、生命周期检查，以确保对于给定的一系列动作接收到所有事件)

处理:实时数据可能需要用历史数据来丰富，挑选一个支持这种范式的平台；实时机器学习，机器学习模型可以使用特定的“在线学习者”进行训练，或者根据输入的数据生成预测。

存储:OLAP、OLTP、HTAP(混合事务/分析处理能够处理 OLTP 和 OLAP 类型的工作负载)

[](https://medium.com/event-driven-utopia/unbundling-the-modern-streaming-stack-451f75eaf1d) [## 拆分现代流堆栈

### 为什么现代流堆栈正在取代经典流架构？构成是什么，有什么价值…

medium.com](https://medium.com/event-driven-utopia/unbundling-the-modern-streaming-stack-451f75eaf1d) [](https://rockset.com/blog/the-rise-of-streaming-data-and-the-modern-real-time-data-stack/) [## 流式数据和现代实时数据堆栈的兴起

### 现代数据堆栈出现在十年前，是对大数据缺点的直接回应。承担…的公司

rockset.com](https://rockset.com/blog/the-rise-of-streaming-data-and-the-modern-real-time-data-stack/) 

改变现代流栈的数据捕获层就是把批量数据源改为流数据源:合流云/卡夫卡、亚马逊 Kinesis、Striim

流处理层:Apache Flink、Beam 和 Spark 等现代工具支持 Python 和 SQL 等语言(相对于基于 JVM 的语言)。 [Materialize](https://materialize.com/) 与 Postgres 有线兼容，支持运行流 SQL 查询。

服务层必须提供:亚秒级**延迟**查询性能、**每秒数十万次查询的吞吐量**、数据新鲜度、复杂查询(例如连接、聚合和过滤)。提到的几个工具:[阿帕奇比诺](http://pinot.apache.org/)，[阿帕奇德鲁伊](https://druid.apache.org/)， [Clickhouse](https://clickhouse.com/) ， [Rockset](http://rockset.com/)

# 附录

[](https://github.com/datastacktv/data-engineer-roadmap) [## GitHub-datastacktv/data-engineer-Roadmap:2021 年成为数据工程师的路线图

### 2021 年成为数据工程师的路线图该路线图旨在全面展示现代数据工程…

github.com](https://github.com/datastacktv/data-engineer-roadmap) [](https://medium.com/coriers/data-engineering-roadmap-for-2021-eac7898f0641) [## 2021 年数据工程路线图

### 帮助你从 0 到数据工程的 12 个步骤

medium.com](https://medium.com/coriers/data-engineering-roadmap-for-2021-eac7898f0641) [](https://c-nemri.medium.com/your-2022-data-engineering-roadmap-3bfe6691ec50) [## 您的 2022 年数据工程路线图

### 亲爱的读者们，你们好吗？我已经离开快两个月了。这是一个很好的理由，我正在做我的一个…

c-nemri.medium.com](https://c-nemri.medium.com/your-2022-data-engineering-roadmap-3bfe6691ec50)