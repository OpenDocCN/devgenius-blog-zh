# 如何使用 Spring Boot 在嵌入式 mongo 中启用副本集

> 原文：<https://blog.devgenius.io/how-to-enable-replica-set-in-embbedded-mongo-with-spring-boot-ddeaa079c1c8?source=collection_archive---------0----------------------->

如果您有一个使用 MongoDb 数据库事务的应用程序，那么您需要一个 MongoDb 副本集来运行它。当您为使用嵌入式 mongo 的应用程序编写单元测试时，问题就来了，因为 spring boot 不配置嵌入式 mongo 来启动开箱即用的副本集。如果我们运行测试，我们会得到以下错误:

```
com.mongodb.MongoClientException: Sessions are not supported by the MongoDB cluster to which this client is connected
```

我去过那里，花了几天时间试图为几个生产应用程序解决这个问题。阅读这篇文章，了解如何轻松解决这个问题。

一个演示项目可以在我的 [GitHub](https://github.com/divyanshshekhar/springboot-embedded-mongo-replset-demo) 上找到。

如果你觉得这个教程有帮助，请跟我来。

# **Tl；博士**

## 1.以 MB 为单位定义副本集名称和操作日志大小

在您的*测试/资源*目录中定义一个*应用程序.属性*文件，并添加以下属性:

```
*spring.mongodb.embedded.storage.repl-set-name: “rs0”
spring.mongodb.embedded.storage.oplog: 10
spring.mongodb.embedded.version: "5.0.5"*
```

您可以使用任何您喜欢的 MongoDB 版本，但它应该高于 4.4，否则将需要额外的配置。如果您想使用最新的 mongodb 版本，请将您的嵌入式 mongo 库和 spring boot 升级到最新版本。

## 2.定义自定义 MongodConfig bean 以启用日志记录

我们需要定义自己的`MongodConfig` bean 来在嵌入式 mongo 实例中启用日志功能。添加以下配置类来覆盖默认的 MongodConfig:

可以看到，大部分代码都是抄袭 Spring Boot 的`*EmbeddedMongoAutoConfiguration.java*` *。*只有以下方法被重写，以便在启动 mongod 进程时启用日志选项。

```
public MongodConfig embeddedMongoConfiguration(EmbeddedMongoProperties embeddedProperties)
```

## 3.Mongod 进程启动后启动副本集

我们需要在 mongod 进程启动后启动副本集。为此，将以下内容添加到单元测试配置类中。如果您还没有单元测试的配置类，请定义一个新的。

# 完整的解释

Spring Boot 的自动配置提供了一种通过定义以下属性来定义副本集名称和操作日志大小的方法:

```
*spring.mongodb.embedded.storage.repl-set-name: “rs0”
spring.mongodb.embedded.storage.oplog: 10*
```

还可以通过定义以下属性来定义您想要使用的 mongodb 版本:

```
*spring.mongodb.embedded.version: "5.0.5"*
```

您可以使用嵌入式 mongo 实现支持的任何 mongodb 版本，但是要确保它高于 4.4，否则需要更多的配置来设置副本集。

我用的是 flapdoodle 的嵌入式 mongo 的([链接](https://mvnrepository.com/artifact/de.flapdoodle.embed/de.flapdoodle.embed.mongo))版本 3.3.1 和 Spring Boot 2.6.3。对于 flapdoodle，可以在以下文件中找到支持的版本列表:

```
de.flapdoodle.embed.process.distribution.Version
```

## 问题#1:默认情况下，日志功能是禁用的

当提供上述属性时，Spring Boot 的嵌入式 mongo 自动配置启用副本集，但不启用日志功能(必须启用日志功能副本集才能工作)，并且不提供直接的方法来这样做。因此，当我们运行测试时，它在启动嵌入式 mongod 进程时失败，并出现以下错误:

```
Running wiredTiger without journaling in a replica set is not supported. Make sure you are not using — nojournal and that storage.journal.enabled is not set to ‘false’
```

这就是为什么我们需要定义我们自己的 MongodConfig bean，并确保它在`*EmbeddedMongoAutoConfiguration*` 类启动嵌入式 mongod 进程之前得到定义。你可以找到上面定义的配置类(`EmbeddedMongoConfigOverride`)，或者在 [Github](https://github.com/divyanshshekhar/springboot-embedded-mongo-replset-demo) 的完整代码中。

## 问题 mongod 启动后需要初始化副本集

现在，在 mongod 进程/节点使用副本集选项启动后，需要再次配置它来初始化副本集。为此，我们需要连接到我们的嵌入式单节点集群，并在*管理*数据库上运行`*replSetInitiate*` 命令。为此，我在我的`*UnitTestsConfig*` 类中添加了以下方法，以便在配置类初始化并且`*mongoClient*` bean 可用后自动运行。此方法启动 replicaset 并等待初始化完成，以便在它完全准备好之前不会触发任何查询。

`*isReplicaSetReady*`方法定义如下，用于检查副本集是否完全初始化:

这就是您需要做的所有配置。现在，您可以愉快地在使用事务的方法上运行所有的单元测试了。检查我的 Github 项目的完整实现。

# 演示项目

这个项目可以在我的 Github 上找到，网址是[https://Github . com/divyanshekhar/spring boot-embedded-mongo-repl set-demo](https://github.com/divyanshshekhar/springboot-embedded-mongo-replset-demo)

源代码应该容易理解。在需要的地方提供了必要的注释。git 提交被标记，这样您就可以跳转到实现的各个阶段，如上所述。git 标签按以下顺序排列:

1.  **带测试的基本服务**:不使用 mongo 事务的基本服务实现(带 Junit 测试)。
2.  **mongo-txn-with-failed-tests**:在服务中启用了 mongo 事务，但是现在由于在嵌入式 mongo 中缺少副本集，测试用例失败了
3.  **没有日志记录的副本集**:使用 spring 属性启用了副本集，但没有启用日志记录
4.  **带日志功能的副本集**:定义自定义`MongodConfig`以启用日志功能
5.  **repl-set-initiate-and-wait-for-ready**:在`UnitTestConfig`中定义的方法，用于初始化副本集并等待它准备就绪
6.  **清除不必要的代码**:清除不必要的`@AutoConfigureBefore`和日志级别后的项目最终版本。