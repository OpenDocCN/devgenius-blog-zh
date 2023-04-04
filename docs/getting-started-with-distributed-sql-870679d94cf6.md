# 分布式 SQL 入门

> 原文：<https://blog.devgenius.io/getting-started-with-distributed-sql-870679d94cf6?source=collection_archive---------9----------------------->

![](img/98bd053743c6c963b00a1bd9dc592be6.png)

照片由[法比奥](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了扩展这两种应用程序的数据层，管理单节点数据库实例的中间件并不少见。但是这种方法有其局限性和管理开销挑战，这导致了[分布式数据库](http://entradasoft.com/blogs/getting-started-with-distributed-sql)在整个工作负载范围内越来越受欢迎。

近年来， [NoSQL 分布式数据库](http://entradasoft.com/blogs/getting-started-with-distributed-sql)已经变得很普遍，因为它们是从底层开始构建的，是分布式的。然而，它们迫使人们做出困难的设计选择，例如选择可用性而不是一致性、数据完整性和易于查询，以满足应用程序的扩展需求。

NoSQL 数据库的使用已经变得广泛，因为对分布式数据库有很大的需求。与传统的关系数据库不同，NoSQL 数据库是为分布式而从头开始构建的。然而，NoSQL 也有明显的缺点，比如缺乏数据一致性，数据完整性方面的漏洞，以及由于固有的缺乏 SQL 支持而导致的缓慢且难以编写的查询。现在有一类新的数据库:分布式 SQL 数据库，也称为 NewSQL。

**全文:**[http://entradasoft . com/blogs/getting-started-with-distributed-SQL](http://entradasoft.com/blogs/getting-started-with-distributed-sql)

**分布式 SQL 数据库具有三层架构。**

**1。SQL API**

顾名思义，分布式 SQL 数据库必须有一个 SQL API，应用程序才能对关系数据建模并执行涉及这些关系的查询。这些[数据库特有的典型数据建模结构是索引](http://entradasoft.com/blogs/getting-started-with-distributed-sql)、外键约束、连接查询和多行 ACID 事务。

**2。分布式查询执行**

查询应该自动分布在集群的多个节点上，这样就不会有单个节点成为查询处理的瓶颈。群集中的任何节点都应该接受传入的查询，然后请求其他节点以最小化处理延迟的方式处理它们的查询部分，处理延迟包括通过网络在节点之间传输的数据量。接受请求的原始节点然后应该将聚集的结果发送回客户端应用程序。

**3。分布式数据存储**

包括索引在内的数据应该跨集群的多个节点自动分布(也称为分片),这样就不会有单个节点成为确保高性能和高可用性的瓶颈。此外，数据库集群应该支持高度一致的复制和多行(即分布式)ACID 事务，以确保单一逻辑数据库概念。

**分布式数据库的优势**

通过水平写入可扩展性按需扩展:随着新节点的添加或现有节点的删除，碎片在所有可用节点之间保持自动平衡。[需要事务性应用的写可扩展性的微服务](http://entradasoft.com/blogs/getting-started-with-distributed-sql)现在可以直接依赖于数据库，而不是添加新的基础架构，如内存缓存(从数据库卸载读请求，以便可以保留它来处理写请求)

地理数据分布的低用户延迟:分布式 SQL 数据库可以提供多种技术来构建地理分布的应用程序，这些应用程序不仅有助于自动容忍区域故障，而且通过使数据更接近最终用户的本地区域来降低最终用户的延迟。

开发人员在 SQL 和事务方面的灵活性:尽管 NoSQL 数据库如 Amazon DynamoDB、MongoDB 和 FaunaDB 已经开始将一些操作事务化，但应用程序开发人员仍然将 SQL 数据库放在心上。这种相似性的原因之一是 SQL 作为一种数据建模语言的内在能力，它可以毫不费力地对关系和多行操作进行建模。

**分布式数据库的例子**

亚马逊极光

谷歌云扳手

TiDB

CockroachDB

**阅读更多:**[http://entradasoft . com/blogs/getting-started-with-distributed-SQL](http://entradasoft.com/blogs/getting-started-with-distributed-sql)