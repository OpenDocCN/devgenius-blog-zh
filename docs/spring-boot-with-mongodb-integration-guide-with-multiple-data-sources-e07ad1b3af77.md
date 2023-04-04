# Spring Boot 与 MongoDB 多数据源集成指南。

> 原文：<https://blog.devgenius.io/spring-boot-with-mongodb-integration-guide-with-multiple-data-sources-e07ad1b3af77?source=collection_archive---------3----------------------->

![](img/a19190e52910c3d75f1d79bafcd06f0c.png)

MongoDB 是一个无模式的 NoSQL 数据库。它是专门为存储大量数据而设计的，并且允许您非常高效地处理这些数据。

**MongoDB 的一些特殊特性是:**

**1)无模式数据库:**一个集合可以保存多个文档，这些文档可以由不同数量的字段、内容和大小组成。在关系数据库的情况下，不要求一个文档与另一个文档相似。

**2)面向文档:**数据存储在键值对中，而不是行和列中，这使得数据更加灵活。

**3)索引:**它使用主索引和次索引对文档中的每个字段进行索引，这使得从数据池中获取或搜索数据更加容易，花费的时间也更少。

**4)可伸缩性:**它提供了水平可伸缩性。我们可以向正在运行的数据库添加新的机器。它使用分片在多台机器上存储数据。分片指的是在多个服务器上分发数据的过程，这里使用分片键将大量数据划分成数据块，这些数据块平均分布在驻留在许多物理服务器上的分片上

**5)高性能:**由于其可伸缩性、索引、复制等特性。与任何其他数据库相比，MongoDB 的性能和数据持久性变得非常高。

**这里我们将讨论 MongoDB 的多个数据源与 spring boot 的集成。**

我们可以通过两种方法连接到 MongoDB 数据库——

MongoRepository

MongoTemplate。

这里我们将使用 MongoTemplate，因为它提供了高级用途和复杂的查询执行。MongoTemplate 让我们能够更好地控制向 MongoDB 请求什么。它提供了一个比 MongoRepository 更简单的步骤来编写复杂的查询。甚至一些业内人士也发现 MongoTemplate 是轻松编写复杂查询的好选择。

让我们从 spring boot 项目开始集成。

**第一步:在 pom.xml 中添加 maven 依赖关系**

```
<dependency>
<groupId>org.springframework.data</groupId>
<artifactId>spring-data-mongodb</artifactId>
</dependency>
```

**第二步:在 application.properties 中添加连接属性**

```
spring.data.mongo.firstdb.uri=mongodb://<username>:<password>@<Host>/<DB Name>
```

**步骤 3:创建 DataSource 类来管理数据库连接。**

```
@Configuration
public class MongoConfig {@Autowired
private Environment env;@Bean
public MongoDbFactory mongoFirstDbFactory() {
return new SimpleMongoDbFactory(new MongoClientURI(env.getProperty("spring.data.mongo.firstdb.uri")));
}@Bean
public MongoTemplate mongoFirstDbTemplate() {
MongoTemplate mongoFirstDbTemplate = new MongoTemplate(mongoFirstDbFactory());
return mongoFirstDbTemplate;
}}
```

步骤 4:创建实体类

```
@Document(collection="user_mongo")
public class UserMongo {@Id
private String userId;
private String name;
private Date creationDate = new Date();public String getUserId() {
return userId;
}public void setUserId(String userId) {
this.userId = userId;
}public String getName() {
return name;
}public void setName(String name) {
this.name = name;
}public Date getCreationDate() {
return creationDate;
}public void setCreationDate(Date creationDate) {
this.creationDate = creationDate;
}}
```

**步骤 5:创建存储库类**

```
@Repository
public class UserMongoRepository {
@Autowired
@Qualifier("mongoFirstDbTemplate")
private MongoTemplate mongoTemplate;public void saveUsers() {
UserMongo user = new UserMongo();
user.setName("medium");
mongoTemplate.save(user);
}}
```

仅此而已！

数据保存在 user_mongo 集合中。如果您有任何问题，我们可以在下面发布。