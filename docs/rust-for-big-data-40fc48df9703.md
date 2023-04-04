# 为大数据生锈！

> 原文：<https://blog.devgenius.io/rust-for-big-data-40fc48df9703?source=collection_archive---------5----------------------->

![](img/74ea4b65c3013772f08367f4d702250b.png)

桑德拉·伊格莱西亚斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## Rust 语言集成到大数据解决方案！

本出版物是 Rust 编程语言在大数据场景中使用不同解决方案的可能性的积累。

Rust 是一种函数式编程语言，每天都在市场上获得更大的空间，因为它被设计为高度安全和高性能，是其他语言(如 C、C++、Java、Google CarbonLang 和 Go 等)的可行替代语言…

大数据场景通常涉及使用 5V:

*   **V** 数据量:如何高效地处理大量数据而不产生不正常的成本？
*   **V** elocity:在业务需要短时间甚至实时信息的情况下，如何捕获、转换、加载和理解这一堆数据？
*   ariety:我们如何处理来自不同来源、不同格式的数据？
*   **V** eracity:随着不同的源和数据经历不同的转换，即使源数据的格式发生变化，我们如何确保源数据中包含的信息继续存在？
*   价值:如何从收集的数据中产生价值？现在我们知道了什么是铁锈，也理解了这个大问题

数据解决方案回答，让我们继续讨论集成的可能性:

## 数据库

*   Scylladb

[](https://www.scylladb.com/2021/07/13/scylla-rust-driver-update-and-benchmarks/) [## Rust 驱动程序更新和基准

### ScyllaDB Rust Driver 诞生于 ScyllaDB 内部开发者黑客马拉松期间。努力并没有因为…而停止

www.scylladb.com](https://www.scylladb.com/2021/07/13/scylla-rust-driver-update-and-benchmarks/) 

*   Neo4J

[](https://community.neo4j.com/t5/drivers-stacks/rust-driver/m-p/20854) [## 铁锈去除剂

### 有人在应用或生产环境中使用 Rust 和 Neo4j 吗？或者可能有人知道关于…

community.neo4j.com](https://community.neo4j.com/t5/drivers-stacks/rust-driver/m-p/20854) 

*   Sqlite

[](https://github.com/rusqlite/rusqlite) [## GitHub - rusqlite/rusqlite:符合人体工程学的 sqlite 防锈绑定

### Rusqlite 是使用 Rusqlite 的人体工程学包装器。从历史上看，API 是基于…

github.com](https://github.com/rusqlite/rusqlite) 

*   关系型数据库

[](https://github.com/blackbeam/rust-mysql-simple) [## GitHub-black beam/rust-mysql-simple:在 rust 中实现的 Mysql 客户端库。

### 此机箱提供:纯 rust 中的 MySql 数据库驱动程序；连接池。特点:支持 macOS、Windows 和 LinuxTLS…

github.com](https://github.com/blackbeam/rust-mysql-simple) 

*   一种数据库系统

[](https://github.com/sfackler/rust-postgres) [## GitHub-sfackler/Rust-postgres:Rust 编程语言的本地 PostgreSQL 驱动程序

### PostgreSQL 对 Rust 的支持。文档本地同步 PostgreSQL 客户端。文档是本地的…

github.com](https://github.com/sfackler/rust-postgres) 

*   MongoDB

[](https://www.mongodb.com/docs/drivers/rust/) [## MongoDB Rust 驱动程序

### 欢迎来到官方 MongoDB Rust 驱动程序的文档站点。您可以将驱动程序添加到您的应用程序中，以…

www.mongodb.com](https://www.mongodb.com/docs/drivers/rust/) 

*   雷迪斯

[](https://medium.com/geekculture/implementing-pub-sub-pattern-in-rust-with-redis-publish-f0eeac3354b1) [## 用 Redis Publish 在 Rust 中实现发布/订阅模式

### 发布/订阅模式，也称为发布/订阅，是一种广泛使用的模式，当需要创建…

medium.com](https://medium.com/geekculture/implementing-pub-sub-pattern-in-rust-with-redis-publish-f0eeac3354b1) 

其他连接器

*   Diesel(PostgreSQL、MySQL、SQLite):

[](https://github.com/diesel-rs/diesel) [## GitHub-diesel-RS/diesel:Rust 的安全、可扩展的 ORM 和查询生成器

### API 文档:最新版本——master branch Diesel 去掉了数据库交互的样板文件，并且…

github.com](https://github.com/diesel-rs/diesel) 

*   Presto(Accumulo、BigQuery、黑洞、Cassandra、Delta Lake、Druid、Elasticsearch、Hive、Hive 安全配置、Iceberg、JMX、Kafka、Kudu、Lark Sheets、本地文件、内存、MongoDB、MySQL、Oracle、Apache Pinot、PostgreSQL、Prometheus、Redis、Redshift、SQL Server、System、Thrift、TPCDS、TPCH ) / Trino (Accumulo、Atop、BigQuery、黑洞、Cassandra、ClickHouse、Delta Lake、Druid、Elasticsearch、Google Sheets

[](https://github.com/nooberfsh/prusto) [## GitHub - nooberfsh/prusto:一个用 rust 编写的 presto/trino 客户端库。

### 用 rust 编写的 presto/trino 客户端库。通过在…上创建帐户，为 nooberfsh/prusto 开发做出贡献

github.com](https://github.com/nooberfsh/prusto) 

## ***数据格式:***

*   三角洲湖

[](https://github.com/delta-io/delta-rs) [## GitHub-Delta-io/Delta-RS:Delta Lake 的原生 Rust 库，绑定到 Python 和…

### 这个库提供了对 Rust 中 Delta 表的低级访问，可以用于数据处理框架，如…

github.com](https://github.com/delta-io/delta-rs) 

*   冰山

[](https://github.com/oliverdaff/iceberg-rs) [## GitHub - oliverdaff/iceberg-rs

### Iceberg 是用于分析数据集的开放式表格格式。访问冰山表非常方便…

github.com](https://github.com/oliverdaff/iceberg-rs) 

*   阿帕奇兽人

[](https://github.com/DataEngineeringLabs/orc-format) [## GitHub-data engineering labs/ORC-format:Rust lib 从 Apache ORC 中读取

### Rust lib 从 Apache ORC 中读取。通过在…上创建帐户，为数据工程实验室/orc 格式开发做出贡献

github.com](https://github.com/DataEngineeringLabs/orc-format) 

*   镶木地板

[](https://github.com/jorgecarleitao/parquet2) [## GitHub-jorgecaleitao/parquet 2:parquet 最快最安全的 Rust 实现。`不安全'免费…

### 这是一个重新编写的官方拼花板条箱与性能，平行和安全的想法。查看指南…

github.com](https://github.com/jorgecarleitao/parquet2) 

*   箭

[](https://github.com/DataEngineeringLabs/arrow-format) [## GitHub-data engineering labs/Arrow-format:Apache Arrow 规范生成的 Rust

### 生成了阿帕奇箭的铁锈。通过创建帐户，为数据工程实验室/箭头格式开发做出贡献…

github.com](https://github.com/DataEngineeringLabs/arrow-format) 

*   Avro

[](https://github.com/DataEngineeringLabs/avro-schema) [## GitHub -数据工程实验室/avro-模式

### 这个机箱包含了本地 Rust 中 Avro 规范模式的完整实现。参见 API…

github.com](https://github.com/DataEngineeringLabs/avro-schema) [](https://github.com/apache/avro) [## GitHub - apache/avro: Apache Avro 是一个数据序列化系统。

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/apache/avro) [](https://github.com/jpastuszek/odbc-avro) [## GitHub-jpastuszek/odbc-avro:Rust crate 扩展了 odbc-iter crate 的功能，能够…

### Rust crate 扩展了 odbc-iter crate 的功能，能够查询 Avro 记录并将整个结果集写成…

github.com](https://github.com/jpastuszek/odbc-avro) 

## 数据转换解决方案

*   DBT

[](https://github.com/andygrove/bdt) [## GitHub - andygrove/bdt:钻孔数据工具

### 无聊的数据工具。在 GitHub 上创建一个帐户，为 andygrove/bdt 开发做贡献。

github.com](https://github.com/andygrove/bdt) 

## 行程安排

[](https://medium.com/@knoldus/schedule-the-program-in-rust-a368f710a17f) [## 在 Rust 中安排程序

medium.com](https://medium.com/@knoldus/schedule-the-program-in-rust-a368f710a17f) 

## 事件流平台

*   融合的卡夫卡

[](https://www.confluent.io/blog/getting-started-with-rust-and-kafka/) [## 开始使用 Rust 和 Apache Kafka

### 我用 clo jure(Java 虚拟机或 JVM 的 lisp 版本)编写了一个事件源银行模拟，名为…

www.confluent.io](https://www.confluent.io/blog/getting-started-with-rust-and-kafka/) 

*   阿帕奇卡夫卡

[](https://github.com/kafka-rust/kafka-rust) [## GitHub-Kafka-rust/Kafka-Rust:Apache Kafka 的 Rust 客户端

### 这个项目开始由约翰·沃德维护，目前的状态是我正在更新这个项目…

github.com](https://github.com/kafka-rust/kafka-rust) 

*   段

[](https://github.com/meilisearch/segment) [## GitHub-meilisearch/Segment:Rust 的细分分析客户端…

### Rust https://segment.com/docs/libraries/rust 分部分析客户端-GitHub-meilisearch/Segment:Segment…

github.com](https://github.com/meilisearch/segment) 

*   兔子 q

 [## 客户端库和开发工具

### RabbitMQ 在许多操作系统上都得到官方支持，并且有几个官方客户端库。在…

www.rabbitmq.com](https://www.rabbitmq.com/devtools.html#rust-dev) 

*   脉冲星

[](https://github.com/streamnative/pulsar-rs) [## GitHub - streamnative/pulsar-rs:用于 Apache Pulsar 的 Rust 客户端库

### 这是一个用于 Apache Pulsar 的纯 Rust 客户端，它不…

github.com](https://github.com/streamnative/pulsar-rs) 

*   MQTT

 [## 如何在 MQTT 中使用 Rust

### Rust 是一种多范例编程语言，旨在提高性能和安全性，实现异常安全的并发性。生锈是…

emqx.medium.com](https://emqx.medium.com/how-to-use-rust-in-mqtt-29b5f918dd59) 

*   AWS 室壁运动

 [## 使用 SDK 进行 Rust 的 Kinesis 示例

### 这是针对 Rust 的 AWS SDK 开发人员预览版的文档。不要在生产中使用它…

docs.aws.amazon.com](https://docs.aws.amazon.com/sdk-for-rust/latest/dg/rust_kinesis_code_examples.html) 

*   谷歌发布订阅

[](https://lib.rs/crates/google-cloud-pubsub) [## 谷歌-云-公共订阅

### Google 云平台 pubsub 客户端库| Rust/Cargo 包

图书馆](https://lib.rs/crates/google-cloud-pubsub) 

## 存储选项

*   HDFS

 [## hdfs - Rust

### hdfs-rs 是一个用于访问 hdfs 集群的库。基本上，它提供了 libhdfs FFI APIs。它还提供了更多…

hyunsik.github.io](https://hyunsik.github.io/hdfs-rs/hdfs/index.html) 

*   最小 io

[](https://github.com/minio/minio-rs) [## GitHub - minio/minio-rs:用于亚马逊 S3 兼容云存储的 MinIO Rust SDK

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/minio/minio-rs) 

*   AWS S3

 [## 亚马逊 S3 示例使用 SDK 进行 Rust

### 这是针对 Rust 的 AWS SDK 开发人员预览版的文档。不要在生产中使用它…

docs.aws.amazon.com](https://docs.aws.amazon.com/sdk-for-rust/latest/dg/rust_s3_code_examples.html) 

## 数据处理:

*   阿帕奇·李维

[](https://github.com/kjmrknsn/livy-rs) [## GitHub-kjmrknsn/Livy-RS:Apache Livy REST API 客户端

### Apache Livy REST API 客户端。在 GitHub 上创建一个帐户，为 kjmrknsn/livy-rs 的开发做出贡献。

github.com](https://github.com/kjmrknsn/livy-rs) 

*   数据融合

 [## crates.io: Rust 包注册表

### 编辑描述

crates.io](https://crates.io/crates/datafusion) 

*   弗林克

[](https://lib.rs/crates/rlink) [## rllink

### 高性能流处理框架。在 Rust 中，Apache Flink 的一个新的、更快的实现。纯…

图书馆](https://lib.rs/crates/rlink) 

这些是将 Rust 连接到各种大数据解决方案的一些方法，还有许多其他方法我还不知道，但几乎每当我需要进行新的集成时，我都会在 Medium 或 crates . io(Rust 社区的 crate registry)或 GitHub(代码库)上查找库。

***#感谢阅读；分享这个帖子！*:)**