# 现代数据栈中的 Apache 开源项目

> 原文：<https://blog.devgenius.io/apache-open-source-projects-in-modern-data-stacks-562329260fc?source=collection_archive---------6----------------------->

![](img/98596936af0fbf29bd923f47e4441ba4.png)

> 编辑:茶桶，github.com/mischaZhang

现代数据堆栈重建了企业数据生态系统。有像 Hadoop、Snowflake、Databricks 和 data lake 这样的数据引擎，也有像 Flink、Spark Streaming、Amazon Kinesis 和许多其他云服务这样的实时数据引擎来支持数据处理。还有很多 BI 工具和现代数据应用。

借助所有这些工具，我们可以构建良好的大数据平台。

![](img/6f119d02dd4324f15b0dc033b63ad41d.png)

数据操作技术包含数据编排、集成、转换和治理。DolphinScheduler、Airflow、Airbyte、Fivetran、SeaTunnel、DBT、Collibra、Bigeye 是 DataOps 中常用的工具和平台。

Apache SeaTunnel(孵化)是一个专注于同步数据和将数据连接到不同系统的项目，Apache DolphinScheduler 是一个数据编排系统。

![](img/5e403f36953953fcc94f59a839b44915.png)

这是现代数据堆栈中的 Apache 项目和企业中的数据操作的大图。有两种类型的数据，即流数据和批量数据。

流数据可以由传感器、物联网设备、网络日志、社交媒体、ERP 和 CRM 系统生成。SeaTunnel 连接器可以将流数据连接到数据消费者，SeaTunnel 代理可以收集网络日志，社交媒体和 SeaTunnel CDC(更改数据捕获)可以从数据库 binlog 中捕获数据。

SeaTunnel Engine、Flink 和 Spark 用于过滤、加载、规范化和将流数据下沉到目标，如 Kafka、alert systems 或任何其他数据库。Kafka 中的数据可以加载到类似 Elasticsearch 和 ClickHouse 的数据库中，并由 Kibana 或 superset 加载以进行可视化。

在某些情况下，需要将实时数据复制到批量数据中。这可以通过使用 SeaTunnel 完成，方法是将您的实时数据流复制到胡迪 Hive 中，您可以将数据用于实时数据仓库，也可以使用 SeaTunnel 将数据进一步加载到您的批处理数据系统中。

批量数据可以由非结构化数据生成，如文件、消息队列、关系数据库、云数据库、NoSQL 数据库和大数据系统。批量数据可以由 SeaTunnel 收集，并存储在 HDFS 和 S3 以备将来使用。或者使用 SeaTunnel 将数据加载到 Hive、Apache 胡迪和数据湖中进行更多的转换。

您可以将非结构化批处理数据加载到数据湖中，然后将这些数据加载到数据仓库中，如 Snowflake 和 Teradata，或者使用没有数据湖的 SeaTunnel 将非结构化数据加载到数据仓库中。

为了使用数据和运行计算，需要数据编排系统来编排作业。DolphinScheduler 可以编排批处理工作流、流数据工作流和 MLOps 工作流。

使用 DolphinScheduler 调度 SeaTunnel sink 作业，可以将批处理数据反馈给 Oracle、SAP、SaaS 系统、社交媒体和关系数据库。

*   阿帕奇海底隧道(孵化)

Apache SeaTunenl(孵化)是一个数据同步项目，支持 50 多个数据源和目的地，如 MySQL、Presto、PostgreSQL、TiDB 和 Elasticsearch。SeaTunnel 支持 Spark、Flink 和 SeaTunnel 引擎。

![](img/ba7b990925bdace147661369ec8dea9e.png)

SeaTunnel 使用标记来生成 Flink、Spark 和 SeaTunnel 引擎代码，这些代码支持流和批处理数据同步。

![](img/e3bb89a89921a0db4d6ccf0aea19b14e.png)

SeaTunnel(开源)有两个主要的用例。

1.  **大数据平台的高容量和高频率**

哔哩哔哩具有高频率的多源数据提取和加载，尤其是每天在数据库和数据仓库之间进行数据同步。平均每天的数据量超过 100TB，数据记录超过数千亿条。

哔哩哔哩正在使用 SeaTunnel 向 ClickHouse 批量加载数据。

将数据加载到 ClickHouse 可能会很慢且效率低下。使用 SeaTunnel，可以高效地将数据加载到 ClickHouse 中。而不是插入每一行数据，SeaTunnel 可以直接生成 ClickHouse 文件，并复制到 ClickHouse 系统中，使数据插入过程更快。我们称之为点击屋系统的批量加载。SeaTunnel 还可以将数据批量加载到数据库和系统中，比如 HBase。

**2。实时异构数据同步**

Vip.com 和滴滴使用 SeaTunnel 将各种数据源(如 MySQL、日志文件、Presto、Kafka、Spark、ClickHouse 和胡迪)的实时数据同步到其他数据系统，涵盖数十个集群。

![](img/93e6517610695e4f2c427195f19ffde2.png)

SeaTunnel 的设计目标是提供一个易于使用、分布式、可扩展的数据集成平台，以高吞吐量和高/低延迟支持超大数据量。

海底隧道解决了许多问题:

*   **各种数据来源**

有数百个数据源，版本不兼容，并且新的数据源不断出现，企业经常使用大量的数据系统。SeaTunnel 集成了多个数据连接器。使用 SeaTunnel 可以节省构建已实现连接器的时间。

*   **批量加载数据和 CDC 冲突**

要开始将数据从一个系统加载到另一个系统，我们需要将所有数据从一个系统批量加载到另一个系统。存储在源数据系统中的新数据通过 CDC(变更数据捕获)过程加载到目标数据系统中。

如果没有 SeaTunnel，就必须有两个不同的数据处理过程来同步数据。SeaTunnel 既可以处理批量加载数据，也可以处理 CDC 数据，用户只需要在 SeaTunnel 中定义一次数据处理过程。

*   **技术体系很复杂**

用户不用在 SeaTunnel 里写 Scala 或者 SQL，我们提供脚本语言，以后还会有 UI 支持。

*   **质量和监控**

SeaTunnel 确保了数据的一致性。用户可以回滚到特定的点并继续数据处理。

*   **难以管理和维护**

管理各种数据处理工作是一项艰巨的任务。我们可以看到不同的数据同步过程被分离并存储在不同的地方，这使得这些作业很难维护。

SeaTunnel 不仅可以帮助用户管理和维护数据同步作业，还可以管理数据切割，并帮助在离线批量数据同步和实时 CDC 之间切换。

SeaTunnel 现在支持 50 多种连接器，包括 20 多种数据源、20 多种类型的接收器和 10 多种类型的转换。明年连接器的数量将达到 150 个。

海底隧道社区很活跃。我们今年将支持的连接器数量增加了一倍，支持 InfluxDB、IceBerg、MongoDB、ClickHouse、Doris、Kudu 等数据系统。

![](img/0360f1d89b096ee38f4b75110f1bb725.png)

SeaTunnel 支持 JSON 和许多其他数据转换工具来转换数据，我们支持使用 Flink、Spark 和 SeaTunnel Engine 作为数据处理引擎。当数据在同步过程中没有被转换时，SeaTunnel Engine 比其他引擎有更高的效率。

![](img/4ae3bbd0634b878c527ad2aaaf068d71.png)

SeaTunnel 基于 SeaTunnel 引擎提供的实时处理或微批处理提供低延迟数据转换，并支持源/转换/接收并行化以提高吞吐量性能。SeaTunnel 实现了分布式快照算法、2PC 提交和幂等写，保证了恰好一次交付。

**网址:**[**https://seatunnel.apache.org**](https://seatunnel.apache.org)**GitHub:https://GitHub . com/Apache/incubator-sea tunnel Slack:**[**https://apacheseatunnel.slack.com**](https://apacheseatunnel.slack.com)**推特:**[**https://twitter.com/asfseatunnel**](https://twitter.com/asfseatunnel)**视频:https://space . bilibili . com/1542095008**

*   阿帕奇海豚调度程序

DolphinScheduler 是一个编排工具。

对于 Linux 用户来说，用 cron 调度任务和脚本会使脚本难以管理，并且部署任务脚本会很复杂。频繁更新脚本可能会导致不稳定。对于 Azkaban 和 Airflow 等其他调度工具来说，管理大容量任务的性能很差，并且缺乏多云数据管理。

DolphinScheduler 在各种任务类别中提供可视化的任务调度。分散式设计保证了调度系统的高稳定性和高可用性。DolphinScheduler 可以稳定支持百万数据，任务同时运行。

DolphinScheduler 拥有超过 1000 名活跃用户和 350 多名社区贡献者。

![](img/17fc681a008c332dcbf8746ffacef33f.png)

DolphinScheduler 无需编码即可拖放任务。下图显示了一个包含多个任务的工作流，shell 工作流连接到 SQL。我们称整个栈为 DAG 图。DAG 映射可以是复杂的，在 DAG 映射中可以有子工作流，并且 DAG 映射可以被其他工作流用作功能。

![](img/55d948446d63c56c40136ae354d00600.png)

DolphinScheduler 提供高性能和高稳定性的多工人和主。DolphinScheduler 支持多个主服务器和工作服务器。每个主服务器管理自己的任务和作业。主节点不是活动-备用或主从节点。DolphinScheduler 使用哈希算法为不同的主服务器调度任务和工作流。

![](img/dc62eb4201e56fd83970540399de8796.png)

DolphinScheduler 受到了世界各地行业领导者的信任，如 IBM、腾讯、思科等。

![](img/521ec175a8a2d7af8953d41e9453571a.png)

DolphinScheduler 通常用于 3 种情况。

1.  高性能、高容量的任务调度

中国联通最初使用一个企业调度系统来支持他们与 Shell (HiveSQL)结合的全球数据平台的数据处理和任务调度。在比较了 Airflow，Azkaban，以及其他商业调度器之后，中国联通最终选择了 DS。

DlophinScheduler 支持中国联通按省市调度任务运行，工作流和任务数量巨大，DolphinScheduler 很好地契合了业务和调度功能需求，支持大数据量，性价比高。

2.数据消费者易于使用的全球云部署

SHEIN 最初使用气流来调度全局任务。但 Airflow 采用集中式设计，缺乏可视化和 K8S 支持，并且很难在云原生环境中进行全球部署。SHEIN 选择从 Airflow 迁移到 DolphinScheduler 是因为它的全球云部署、K8S 支持、确保稳定性的去中心化架构以及对数据用户友好的设计。

DolphinScheduler 支持 SHEIN 在 Kubernetes 上的 5 万个任务。SHEIN 的数据科学家和数据分析师使用 DolphinScheduler 来编排数据任务和工作流，无需编码。

3.人工智能/人工智能编排

DolphinScheduler 支持荔枝 FM 和 360 的数据科学平台。通过 DolphinScheduler 的 DAG 执行引擎，可以管理和重用机器学习任务、AI 训练任务和大数据任务。并且借助 DolphinScheduler 的低代码 IDE，荔枝 FM 将数据采集过程与模型训练任务轻松连接起来。

DolphinScheduler 即将推出新版本 3.1.0。

海豚调度程序 2。x 版本提供简单和所见即所得的工作流，高可靠性，丰富的工作流功能，并且是云原生和可扩展的。3.1.0 版 DolphinScheduler 支持更多与机器学习相关的编排、数据流功能、Python 和 YAML 工作流。

在 3.1.0 中，用户可以使用 DolphinScheduler 进行数据准备和 MLOps，支持 ML flow、Sagemaker、DVC、Jupyter 和 PyTorch。DolphinScheduler 今年将增加 Kubeflow、TensorFlow 和 Bentoml。DlophinScheduler 3.1.0 支持 Flink 和 Spark 流以及数据流工作流管理。在 3.1.0 中，DolphinScheduler 工作流可以直接从 Python 和 YAML 生成，这使得代码审查和版本管理变得容易。

PyDolphinScheduler 是用于 Apache DolphinScheduler 的 Python API，用户可以用 Python 或 YAML 代码定义工作流，也称为 workflow-as-codes。工作流实例可以通过 DolphinScheduler 进行管理。

![](img/08c793f33ace035c15eb29fc8cb66f6e.png)

dolphin scheduler MLOps Orchestration 为 MLOps 提供编排能力，支持多种机器学习相关的任务插件，帮助用户高效、低成本地构建机器学习平台和连接大数据平台。

DolphinScheduler 可以编排用户现有的机器学习任务，为主流的 MLOps 项目提供开箱即用的编排，通过开源的机器学习项目预设算法能力，并提供连接机器学习平台和大数据平台的能力。

![](img/eb0f500b80631e42375702c19f9fc88a.png)

用户可以使用 shell 或 EMR 准备数据，然后使用 PyTorch 进行机器学习任务。

![](img/063a22053846902dcd30d4ba0546946b.png)

用户可以将机器学习工作流连接到 Spark 和 SageMaker。SageMaker 在 DolphinScheduler 中被视为任务。

![](img/44597b2eb9130e3b912222d354c15940.png)

DolphinScheduler 提供支持机器学习工作流的插件，并帮助数据科学家用 DVC 和 SageMaker 管理数据，用 MLflow 和 SageMaker 管理模型。DolphinScheduler 支持 OpenMLDB 和 SageMaker 这样的特性库。用户可以使用 shell、python、Jupyter、MLflow、PyTorch 和 SageMaker 训练模型，使用 Shell、Python、MLflow 和 SageMaker 部署模型。

DolphinScheduler 支持 MLOps 编排的任务插件:

![](img/b90c548ce3994ca91f40c867fd195f40.png)

DolphinScheduler 将支持更多的机器学习项目，如 TensorFlow、BentoML、KubeFlow 和 Core。如果你对地图和编排感兴趣，请加入我们的 slack 频道。

**Website:** [**https://dolphinscheduler.apache.org**](https://dolphinscheduler.apache.org) **GitHub：https://github.com/apache/dolphinscheduler wehchat：海豚调度 Slack:** [**https://s.apache.org/dolphinscheduler-slack**](https://s.apache.org/dolphinscheduler-slack) **Twitter : @dolphinschedule Video：https://space.bilibili.com/515596012**

SeaTunnel 和 DolphinScheduler 在现代数据堆栈中的位置如下所示。它们使用户能够更高效地集成数据和编排任务。

![](img/441f05e316cf20313d1a4062ce0017cf.png)![](img/090f9fbd679eda10fea23e51ffba860e.png)

郭炳湘:

Apache Software Foundation 成员 Apache IPMC 成员 Apache DolphinScheduler 导师 Apache SeaTunnel(孵化)创始人 ClickHouse 中国社区 Track 主席 Apache Con Asia 2021/2022 William 曾任易观国际 CTO 和联想高级大数据总监。他曾在 CICC、IBM 和 Teradata 担任大数据总监/经理。他在大数据技术和管理方面拥有超过 20 年的经验。

[https://www.linkedin.com/in/williamk2000/](https://www.linkedin.com/in/williamk2000/)

郭炳联电邮:guowei@apache.org

推特:国威 _ 威廉

微信:guodaxia2999

# 关于海底隧道

SeaTunnel(原 Waterdrop)是一个简单易用、超高性能的分布式数据集成平台，支持海量数据的实时同步，可以稳定高效地同步每天数千亿的数据。

我们为什么需要海底隧道？

SeaTunnel 竭尽所能解决你在同步海量数据时可能遇到的问题。

*   数据丢失和重复
*   任务构建和延迟
*   低吞吐量
*   从应用到生产周期长
*   缺乏应用程序状态监控

**海底隧道使用场景**

*   海量数据同步
*   海量数据集成
*   大量数据的 ETL
*   海量数据聚合
*   多源数据处理

**海底隧道的特点**

*   丰富的组件
*   高可扩展性
*   使用方便
*   成熟稳重

**如何快速上手 SeaTunnel？**

想快速体验海底隧道？SeaTunnel 2.1.0 只需 10 秒钟即可启动并运行。

[https://seatunnel.apache.org/docs/2.1.0/developement/setup](https://seatunnel.apache.org/docs/2.1.0/developement/setup)

**我能做些什么？**

我们邀请所有对本地开源全球化感兴趣的合作伙伴加入 SeaTunnel 贡献者大家庭，共同促进开源！

提交问题:

[https://github.com/apache/incubator-seatunnel/issues](https://github.com/apache/incubator-seatunnel/issues)

将代码贡献给:

[https://github.com/apache/incubator-seatunnel/pulls](https://github.com/apache/incubator-seatunnel/pulls)

订阅社区发展邮件列表:

dev-subscribe@seatunnel.apache.org

开发邮件列表:

dev@seatunnel.apache.org

加入时差:

[https://join . slack . com/t/Apache seatunnel/shared _ invite/ZT-1 HSO 5 N2 TV-mkfkwxonc 70 heqgxtvi 34 w](https://join.slack.com/t/apacheseatunnel/shared_invite/zt-1hso5n2tv-mkFKWxonc70HeqGxTVi34w)

关注 Twitter:

[https://twitter.com/ASFSeaTunnel](https://twitter.com/ASFSeaTunnel)

来加入我们吧！