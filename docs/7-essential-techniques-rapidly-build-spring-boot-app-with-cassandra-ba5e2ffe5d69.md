# 7 个基本技巧——用 Cassandra 快速构建 Spring Boot 应用

> 原文：<https://blog.devgenius.io/7-essential-techniques-rapidly-build-spring-boot-app-with-cassandra-ba5e2ffe5d69?source=collection_archive---------4----------------------->

## 快速学习 Spring Data Cassandra 的方法

![](img/1a0b5f591df8ccd0b2cf3a24d37e737a.png)

萨曼莎·福特尼在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

NoSQL Cassandra 数据库以高性能著称。许多软件设计者选择这种数据库，以便为不断增长的请求量实现高吞吐量系统。

虽然 Cassandra 上的查询看起来像 MySQL 等传统关系数据库的 SQL 查询，但它们有明显的不同。因此，当工程团队第一次开始使用 Cassandra 构建软件时，问题就出现了。在继续开发之前，软件工程师需要知道“如何做”。弄清楚所有的细节需要时间。

为了帮助您加快开发速度，在本文中，您将找到一个关于如何构建在 Cassandra 上运行查询的 Spring Boot 应用程序的编码提示列表。

## 卡珊德拉是什么？

本节让您快速了解 Cassandra 的概念。如果您已经熟悉 Cassandra 数据库，请跳到下一节。

Cassandra 是一个分布式数据库，它将数据表分割成分区，分散存储在多个节点上。数据分区基于数据表上定义的分区键。

为了说明这个概念，下图显示了一个以客户 id 作为分区键的客户订单表。您可以看到，该表根据客户 id 被分割成多个分区，并分布在不同的节点上。

![](img/246b30ea207b6296fc30ab39e69f6713.png)

Cassandra 表分区

如果分区键不是唯一标识符，那么簇键标识同一分区内的唯一记录。聚簇键指定分区内记录的自然顺序，它可以是查询中 WHERE 子句的一部分。

以下示例显示了客户订单表中的“分类关键字-交货日期”列。记录按交付日期降序排列。

![](img/88e95804767592c797c02c5b1114309e.png)

Cassandra 按群集键的自然顺序

Cassandra 上的表定义类似于 SQL 定义。分区键+簇键构成复合唯一键。

```
CREATE TABLE IF NOT EXISTS demo.orders (
  customer_id int,
  delivery_date date,
  order_ref text,
  status text,
  total_amount double,
  delivery_address text,
  PRIMARY KEY (customer_id, delivery_date)
) WITH CLUSTERING ORDER BY (delivery_date DESC);
```

参考这篇文章，它通过实际动手练习详细阐述了这个概念。

[](/the-essential-concepts-of-high-speed-distributed-database-cassandra-b87267d5f83e) [## 高速分布式数据库的基本概念— Cassandra

### Cassandra 分区键、簇键、查询设计实用指南

blog.devgenius.io](/the-essential-concepts-of-high-speed-distributed-database-cassandra-b87267d5f83e) 

## 开始你的第一个卡珊德拉节点

首先，让我们在本地机器上启动 Cassandra 节点并进行探索。与其经历复杂的安装步骤，不如在 docker 容器上运行实例。

1.  克隆此 GitHub 存储库:

[](https://github.com/gavinklfong/spring-cassandra-demo) [## GitHub-gavinklfong/spring-Cassandra-demo

### 在 GitHub 上创建一个帐户，为 gavinklfong/spring-Cassandra-demo 开发做贡献。

github.com](https://github.com/gavinklfong/spring-cassandra-demo) 

2.启动 Cassandra docker 容器

确保您的本地机器上运行着 docker 引擎，然后将目录切换到文件夹 **resources/docker-compose** 中，并运行以下命令:

```
docker compose up
```

这个 docker compose 自动在本地机器上启动一个 Cassandra 节点，并加载数据库脚本 **tables.cql** ，在一个 keyspace **超市**下加载一组示例数据库表和数据

3.运行数据查询工具

Cassandra 上有许多查询客户端供您查看数据，如 **DBeaver Enterprise 版本**和 **Intellij Ultimate 版本**。Cassandra 默认带有 **cqlsh** 用于控制台中的查询。运行这个命令，您将在 Cassandra 容器上启动一个 CQLSH 命令会话:

```
docker exec -it cassandra cqlsh localhost 9042 --cqlversion='3.4.5'
```

如果您已经成功连接到 docker 容器，您将会看到 CQLSH 的这个命令提示符。

```
Connected to Test Cluster at localhost:9042
[cqlsh 6.0.0 | Cassandra 4.0.3 | CQL spec 3.4.5 | Native protocol v5]
Use HELP for help.
cqlsh>
```

4.执行示例查询

例如，下面是订单记录检索的示例结果

```
SELECT * FROM supermarket.customers;
```

![](img/91112bb7a5ad14f0e3e4b2b1bd613e35.png)

# 春季数据卡珊德拉

现在，让我们来探索软件框架如何与 Cassandra 集成。Spring Data 提供了一个通用层来支持许多流行的数据库，包括关系数据库、MongoDB、Elasticsearch、Cassandra 等。

该框架在方法调用和配置方面对开发人员友好，提供了一个平滑的开发旅程，隐藏了低级代码的复杂性。当然，如果开发人员想对原生查询进行更多的控制，它允许灵活性。

Spring 数据框架的范例通常为系统集成提供存储库(高层)、DSL 和本地查询(低层)。该图为您提供了一个概览:

*   最简单的方法是用自动生成的实现来定义 Cassandra 存储库接口。
*   为了更多的控制，Cassandra Template 提供了用于查询构造的 DSL。
*   原生查询的使用允许更多的控制，但是代码很难维护，因为实现与 Cassandra 查询语法紧密耦合。

![](img/1275b7ba8ac8d48d78dfc932b71b7eb4.png)

春季数据—卡珊德拉

## Maven 依赖性

要准备依赖项列表，请使用 Spring Intializr 生成起始项目。记得在依赖列表中包含 Apache Cassandra 的春季日期。

![](img/0f90066f3685f19b88f23e601bca2db4.png)

## CQL 档案协会

如果您的 Intellij 无法识别 Cassandra 查询文件(即 CQL)的文件扩展名*。cql，然后在 ***设置→编辑器→文件类型*** 中添加与 SQL 语法高亮关联的文件类型。由于 CQL 的语法类似于 SQL，因此快速设置是将 CQL 文件类型与 SQL 语法高亮区相关联。

![](img/0fc1612efced57e1c3ec428577a910b9.png)

## 连通性

应用程序启动后，Spring Boot 自动连接到一个卡珊德拉数据库。连接配置在**应用程序属性**中定义。

下面是示例 Spring Boot 配置，它指示框架使用用户名和密码在本地主机的端口 9042 上创建一个 Cassandra 连接，默认的密钥空间是“超市”。

```
spring.data.cassandra.keyspace-name = supermarket
spring.data.cassandra.contact-points = 127.0.0.1
spring.data.cassandra.port = 9042
spring.data.cassandra.local-datacenter = datacenter1
spring.data.cassandra.username = cassandra-user
spring.data.cassandra.password = 1Password@
```

# GitHub 知识库

基于代码学习是掌握编程技术的最佳方式，我将基于这个 GitHub 库的源代码，用示例代码和单元测试用例来演练技巧，所以请在继续之前将其克隆到您的本地工作区。

[](https://github.com/gavinklfong/spring-cassandra-demo) [## GitHub-gavinklfong/spring-Cassandra-demo

### 在 GitHub 上创建一个帐户，为 gavinklfong/spring-Cassandra-demo 开发做贡献。

github.com](https://github.com/gavinklfong/spring-cassandra-demo) 

# 技巧#1 —自动实现的存储库

定义数据存储库接口是用最少的代码构建 Cassandra 数据访问层的最快方法。

下面简单的客户表定义以文本数据类型存储客户记录，包括姓名、电话和电子邮件，并且 **id** 是唯一标识客户记录的主键。

对于从数据库模式到对象属性的映射，Spring Data 为到表名和主键字段的链接提供了注释`@Table`和`@PrimaryKey`。框架通过属性名称自动将其他类属性映射到表字段。

存储库接口的使用非常简单，它只是一个从运行时生成实现的框架提供的接口扩展而来的接口。

默认情况下，自动实现附带了适合大多数用例的查询方法，如`findById()`和`save()`。

# 技巧 2——使用自动化测试验证 Cassandra 查询

越早对代码进行测试，您的系统就越有信心以预期的行为运行。

感谢 [Testcontainer](https://www.testcontainers.org/) 库，它提供了一种在 docker 容器中构建一次性 Cassandra 数据库进行测试的便捷方式。

下面的单元测试示例代码启动 Cassandra 并执行一个初始数据库脚本，以便创建包含示例数据的表。然后，连接性被动态地注入到应用程序上下文中，因此 spring boot 应用程序能够连接到 Cassandra 实例以执行单元测试用例。

这是一个验证记录保存和检索的简单测试场景。此数据库脚本创建插入了示例记录的客户表:

单元测试类利用 Testcontainer 注释@Testcontainers 和@Container，用测试执行的初始脚本启动 Cassandra。@DynamicPropertySource 在启动时动态地将配置注入到 Spring 应用程序上下文中。

最后，运行测试用例来验证存储库的`findById()`和`save()`

在 GitHub 源代码中找到`SimpleCustomerRepo.java`并运行测试，您将看到类似的结果如下:

![](img/25ab344348967a0bb08291a6dc1a8e5d.png)

# 技巧#3 —用户定义类型的对象映射

除了内置的数据类型，Cassandra 还支持用户定义的数据类型。让我们设置地址数据类型，并在客户表中定义它。地址类型用 ***冻结*** 关键字括起来，确保 Cassandra 将地址字段更新为单个字段，以避免[逻辑删除](https://www.beyondthelines.net/databases/cassandra-tombstones/)。

注释`@UserDefinedType`在 Spring 框架中支持用户定义的类型。**下面的 Address** 类是 Cassandra 中的用户自定义类型，类型名为 **address_type** 。这个例子演示了注释`@Column`的用法，它将对象属性映射到指定的数据库列名。

将 address 字段添加到对应于 Cassandra 表模式定义的 customer 数据模型中。注释@ **冻结**对应 Cassandra 中的冻结数据字段

同样，我们有另一个存储库接口，用于地址为的客户的数据模型:

现在，运行单元测试`CustomerWithAddressRepoTest.java`来验证带有地址的客户的数据模型。

# 技巧#4 —数据收集的对象映射

Cassandra 支持表模式中的数据集合(映射、集合和列表)。由于每个客户可能有多个地址，我们将地址放在一个数据图中。输入关键字是一个文本字段类型，如“家庭”和“办公室”，它在数据映射中标识一个唯一的地址。

相应数据模型中的地址字段是`Map<String, Address> addresses.`

执行单元测试类`CustomerWithAddressesRepoTest.java`，你将能够用数据图验证数据模型。

# 技巧 5 —组合键的对象映射

Cassandra 数据表中有两种主键—分区键和簇键。基于分区键，数据表被分成均匀分布在多个数据库节点上的分区。簇键标识一个分区内的唯一记录。

例如，下面的表格模式显示了网上超市的交货时间。记录根据交付日期进行划分，而交付开始时间和交付团队 id 是群集键。

为了定义对象映射，我们有一个包含 3 个关键字段的容器类。如您所见，该类是使用注释`@PrimaryKeyClass`在类级别定义的，并且`@PrimaryKeyColumn`允许我们指定键类型— `PrimaryKeyType.PARTITIONED` / `PrimaryKeyType.CLUSTERED`

然后，使用`@PrimaryKey`指定数据模型中的关键字段

# 技巧 6 —本地查询

存储库的默认实现可能不灵活，无法满足系统需求。然后，是时候应用本地查询了。

下面的查询方法执行`@Query,`中定义的查询字符串，它是获取指定交付日期的记录，结果按开始时间排序。

查看 GitHub 源代码，查询由单元测试类`DeliveryTimeslotRepoTest.java`验证，初始数据库脚本在 2022-02-15 插入交付时隙记录，测试预计查询结果中有 3 个记录计数，按开始时间排序。

# 技巧 6 —使用 DSL 进行查询

除了存储库和原生查询，Spring 还为查询执行提供了 DSL。如果您想在不耦合 CQL 语法的情况下对数据访问进行更多的控制，这是一个明智的选择。

查询执行依赖于一个助手类`CassandraOperation`。DSL 是高度可读的，可能，你看下面的代码就知道它是干什么的了。这个示例实现的查询与上面的本地查询相同，即按交付日期提取记录并按开始时间排序。

查看 GitHub 源代码，查询由单元测试类`DeliveryTimeslotDslRepoTest.java`验证

# 技巧 7——轻量级事务

尽管为了高性能，Cassandra 没有事务会话的概念，但替代方案是执行“比较和设置”操作的轻量级事务。

其思想是在应用任何数据更新之前确保满足某些条件，以确保数据的一致性。

比方说，只有当目标记录的预留客户 id 是空 UUID(即全零 UUID)时，我们才会预留时隙。

在 Cassandra 原生查询中，轻量级事务由 **IF <条件>** 子句支持:

带有 IF 子句的 Cassandra 本机查询

Spring Data DSL 提供了一种简单的实现方式。下面的示例代码运行轻量级事务并返回一个布尔值，该值指示记录是否成功更新。

查看单元测试代码`DeliveryTimeslotDslRepoTest.java`，它验证了成功和失败的测试场景。

# 最终想法

谈到 Cassandra 数据访问的实现，Spring Data 是一个强大的框架。它提供自动对象映射、自动实现和 DSL，允许 Cassandra 查询的快速开发和与业务逻辑的集成。

此外，如果您正在寻找非阻塞 I/O 的实现，Spring framework 支持反应式查询。

本文涵盖了 Cassandra 数据集成的最常见场景。此外，示例代码和自动化测试为您提供了有用的参考。享受你的编码之旅。