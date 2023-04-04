# node . js dynamo db 初学者指南

> 原文：<https://blog.devgenius.io/beginners-guide-to-dynamodb-with-node-js-285a467009f?source=collection_archive---------17----------------------->

![](img/5517e60e8443287b280832a538f15d66.png)

DynamoDB 是一个存储大量数据的伟大数据库

长期以来，我对所谓的 NoSQL 数据库非常怀疑。我相信传统的 SQL 数据库为定义数据结构和处理数据提供了更好的高层抽象。然而，我收到了一些针对我的项目构建者 [Goldstack](https://goldstack.party) 的 [DynamoDB](https://aws.amazon.com/dynamodb/) 模板的查询，我认为处理 DynamoDB 访问的模块可以很好地添加到模板库中。

具体来说，我认为 DynamoDB 可以很好地适应无服务器应用程序，因为它提供了一个冷启动时间最少的数据存储，从而可以预测无服务器功能的低延迟访问。

在 DynamoDB 中对数据进行正确建模以及在 Node.js 应用程序中进行基本操作非常复杂。因此，我想我整理了一篇文章，涵盖了我过去几周的学习。本文涵盖:

*   如何为 DynamoDB 建模数据
*   如何创建表和运行迁移
*   如何创建和查询数据

# TL；博士；医生

正如我的许多文章一样，我已经整理了一个开源示例项目和模板，它处理了启动和运行 DynamoDB 应用程序的大量繁重工作:

*   [DynamoDB 模板](https://goldstack.party/templates/dynamodb)
*   [DynamoDB 样板/示例项目](https://github.com/goldstack/dynamodb-boilerplate)

上述模板和样板文件会定期更新和自动测试(项目安装和建立基础设施)。如果您仍然遇到任何问题，请[在 GitHub](https://github.com/goldstack/goldstack/issues) 上提出问题。

# 数据建模

DynamoDB 本质上是一个修饰过的[键值存储](https://en.wikipedia.org/wiki/Key%E2%80%93value_database)。因此，其基本结构如下:

```
key --> value
```

例如，如果我们想要定义一个用户数据库，我们需要确定我们想要用来识别用户的*键*。识别正确的键通常比值更重要。因为 DynamoDB 是无模式的，所以我们基本上可以将任何我们喜欢的东西放入值中，而没有任何限制。因此，我们可以将用户数据定义如下:

```
`joe@email.com` --> {name: 'Joe', dob: '31st of January 2021'}`
`jane@email.com` --> {name: 'Jane', newsletterSubscribed: false}`
```

请注意，虽然我们的密钥是一致的(始终是用户的电子邮件地址)，但这两个值之间的值结构是不同的。如前所述，因为 DynamoDB 是无模式的(至少对于值来说)，这一切都很好。

不过，这种灵活性是有代价的。虽然在传统的 SQL 数据库中，我们通常可以为表中的所有列编写查询，但是 DynamoDB 只允许对键进行高效的查询。因此，例如，在一个 SQL 数据库中，我只需要创建一个查询来获取在某一年出生的所有用户，这并不容易。

为了解决这个基本的缺点，我们可以在 DynamoDB 中使用一些策略。两个最重要的是复合键和全球二级索引(GSI)。

复合键是一个简单的技巧，将两个不同的字段组合成一个键。例如，如果查询订阅我们时事通讯的所有用户对我们很重要，我们可以定义以下键:

```
[newsletterSubscribed, email] -> value
```

实现这一点的一个简单方法是编写一个复合字符串，比如`false#jane@email.com`，但是 DynamoDB 有一个特殊的功能，我们可以使用它:排序键。DynamoDB 允许我们将我们的键定义为由两个元素组成的复合键:一个*分区键*和一个*排序键*。我不喜欢名字分区键，因为对我来说，它听起来太像主键了，本质上分区键和排序键一起就是我们表的主键。

无论如何，使用分区键和排序键，我们可以定义一个复合键，如下所示:

```
[partitionKey: email, sortKey: newsletterSubscribed] -> value
```

排序键非常强大，因为 DynamoDB 允许我们对其使用许多查询运算符:如`begins_with`、`between`、`>`、`<`。

正如您可能已经收集到的，当我们对查询表中的一个特定属性感兴趣时，这种完整的排序键方法非常有效。然而，我们不能轻易地将这种方法扩展到我们感兴趣的其他属性。例如，如果我们还想查询用户的出生日期，我们不能使用与上面相同的排序键。

为了解决这个问题，DynamoDB 提供了[全局二级索引](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html)。全局辅助索引本质上是将您的表中的所有数据(与索引相关的数据)克隆到另一个动态数据库表中。因此，我们可以定义一个不同于表的分区键和排序键。例如，我们可以定义以下内容:

```
Table: [partitionKey: email, sortKey: newsletterSubscribed] -> value
GSI: [partitionKey: email, sortKey: dob] -> value
```

(请注意，我们也可以使用不同的分区键)。

这揭示了动态数据库的一个有趣的局限性。我们需要定义一个“模式”(例如，我们使用什么样的分区键、排序键和 GSI ),专门用于我们想要对表执行的查询。然而，必须注意的是，在传统的基于 SQL 的数据库中，我们也需要考虑同样的问题，因为我们通常需要为运行重要查询的关键属性定义索引。

在我们结束数据建模之前，我想介绍两种更常见的数据建模模式:多对一和多对多关系。

多对一关系相对简单，因为我们有分区键和排序键。例如，假设我们想要表达公司和用户之间的关系；其中每个用户都属于一个公司，而一个公司可以有多个用户。

我们的第一种方法是为公司创建一个表，为用户创建另一个表。在 DynamoDB 中不建议这样做。相反，我们通常以所谓的[单表设计](https://www.sensedeep.com/blog/posts/2021/dynamodb-singletable-design.html)为目标。由于表中每条记录的值不遵循公共模式，因此在同一个表中存储多个实体的数据相对容易。

有点棘手的部分是我们将使用的键。本质上，我们至少由两部分组成我们的键:我们引用的实体的类型和匹配的 id。例如，我们可以有关键字，例如:`user#{email}`。

注意，虽然排序键允许我们在查询中使用像`starts_with`这样的操作，但是分区键不允许。因此，如果我们对诸如`give me all user entities`这样的查询感兴趣，我们需要确保将实体标识符`user`添加到排序关键字中。

现在，为了模拟用户和公司之间的关系，我们可以定义一个模式如下:

```
Company Entity: [partitionKey: company#{name}, sortKey: company#]
User Entity: [partitionKey: company#{name}, sortKey: user#{email}]
```

注意，我们对两个实体使用相同的分区键。分区键的主要功能是帮助我们建立一个可伸缩的系统。DynamoDB 根据提供的分区键在节点之间分配工作负载。因此，我们想要做的是定义分区键，使相关的数据被分配到同一个节点，但不会有太多的记录链接到一个节点，以至于我们得到一个热键。

上面的模式现在允许我们非常容易地查询一个公司的所有用户。当我们构建查询时，我们只需提供:

```
partitionKey equals company#{name}
sortKey starts_with user#
```

然而，我们不能通过电子邮件方便地查询用户。DynamoDB 查询总是需要一个分区键(以便 DynamoDB 知道将查询发送到哪个节点),如果我们只有一个用户电子邮件，我们不会知道用户属于哪个公司。为此，我们将全球二级指数(`gsi1`)定义如下:

```
Company Entity: [partitionKey: company#{name}, sortKey: company#]
User Entity: [partitionKey: company#{name}, sortKey: user#{email}, gsi1_partitionKey: user#{email}]
```

现在，我们可以通过查询全局二级索引来为特定用户启动查询。

我想讨论的第二种模式是多对多关系。比方说，一个用户可能属于多个公司。在关系数据库中，我们需要定义一个额外的表来表示多对多关系。在 DynamoDB 中，我们同样引入了新的实体。具体来说，我们需要介绍两个实体:*公司-用户关系*和*用户-公司关系*。这将导致以下模式:

```
Company Entity: [partitionKey: company#{name}, sortKey: company#]
User Entity: [partitionKey: user#{email}, sortKey: user#]
Company-User Relationship: [partitionKey: company#{name}, sortKey: user#{email}]
User-Company Relationship: [partitionKey: user#{email}, sortKey: company#{name}]
```

这允许我们查询属于一个公司的所有用户和一个用户所属的所有公司，因为我们可以简单地使用新关系的分区键。关系实体可能没有任何值——但是如果我们添加了值，这些值在语义上就是关系的属性。例如，我们可以提供一个属性`joinedAt`,表示用户何时加入一家公司。

注意，所有这些实体都属于同一个 DynamoDB 表。我们只为这个表定义了一个分区键和一个排序键:都是 string 类型。Key 是我们为这些键提供的值。可以想象，这很快会变得有点混乱。因此，我建议用代码来表达这个“模式”(例如，我们在基表中放置什么类型的键)。在本文的后面，我将展示如何使用 [DynamoDB 工具箱](https://github.com/jeremydaly/dynamodb-toolbox)框架来实现这一点。

整个大学课程都致力于为传统数据库建立关系数据模型，这种情况并不少见。因此，不要期望在阅读完以上内容后成为 DynamoDB 建模数据的大师。我的意图是提供最低水平的理解，使我们能够开始编写一些相当好的代码。但是，如果您正在考虑构建更大规模的系统，我强烈建议您查看更多的参考资料。AWS 文档通常是一个很好的起点:

*   [有效设计和使用分区键的最佳实践](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html)
*   [在 DynamoDB 中使用二级索引的最佳实践](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-indexes.html)
*   [管理多对多关系的最佳实践](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-adjacency-graphs.html)

# 创建表和运行迁移

创建 DynamoDB 表的方法有很多，比如使用 [AWS 控制台](https://naveenvasamsetty.wordpress.com/2015/11/29/introduction-to-amazon-dynamodb-lab-instruction/)、[。NET SDK](https://sachabarbs.wordpress.com/2018/12/09/aws-dynamodb/) 或[通过 ORM 层](https://dev.to/agusnavce/dynamodb-cri-dynamodb-model-wrapper-to-enhance-dynamodb-access-aia)动态实现。

在我看来，通常最好使用 [Terraform](https://www.terraform.io/) 来定义无服务器基础设施。在 Terraform 中定义一个 DynamoDB 表可以让我们很容易地将它链接到其他资源，比如 Lambda 函数。然而，在本地测试 Terraform 中定义的资源并不容易。相比之下，通过 CLI 或 SDK 之一创建一个表使得使用 [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) 进行本地测试变得容易。

此外，虽然 Terraform 在技术上允许更新 DynamoDB 表，但它确实不是这项工作的合适工具，因为在应用更改时，存在意外副作用的重大风险。相反，用代码定义迁移在定义迁移时提供了更大的灵活性和安全性。

您可能会问:既然 DynamoDB 是无模式的，我们为什么还要担心迁移呢？虽然从技术上讲，DynamoDB 不要求我们在开始插入和查询数据之前定义模式，但是分区键、排序键和全局二级索引将排序函数定义为模式，并且需要随着应用程序的发展而发展。例如，新出现的查询模式可能要求我们定义新的全局二级索引。

允许我们利用 Terraform 的声明能力以及在代码中定义“模式”的优势的一种方法是，在使用 [aws_dynamodb_table](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/dynamodb_table) 数据属性的同时，在代码中创建我们的表并管理迁移。我们只需要向这个资源提供 DynamoDB 表的名称，然后就能够在 Terraform 中为这个表定义补充资源(比如 IAM 权限)。

在附带的示例项目中，从 Terraform ( [main.tf](https://github.com/goldstack/dynamodb-boilerplate/blob/master/packages/dynamodb-1/infra/aws/main.tf) )引用 DynamoDB 表如下:

```
data "aws_dynamodb_table" "main" {
  name = var.table_name
}
```

现在的问题是，如果还没有创建这个特定的表，`terraform plan`和`terraform apply`将会失败。为此，我开发了一个简单的库，确保在执行任何地形操作之前创建 DynamoDB 表`[@goldstack/template-dynamodb](https://www.npmjs.com/package/@goldstack/template-dynamodb)`。

该库将使用 AWS SDK 通过`[createTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#createTable-property)`操作( [dynamoDBData.ts#L13](https://github.com/goldstack/goldstack/blob/master/workspaces/templates-lib/packages/template-dynamodb/src/dynamoDBData.ts#L13) )来创建表:

这创建了一个非常普通的 DynamoDB 表。这足以确保 Terraform 在建立进一步的基础设施时可以参考一些东西。

如果我们想要改变这个表的设置(比如`BillingMode`)或者定义额外的全局二级索引，我们可以在需要的时候使用迁移。在示例项目中，我使用 [Umzug](https://github.com/sequelize/umzug) 配置了迁移。这仅仅需要为 um zug:[umzugdynamodbstorage . ts](https://github.com/goldstack/goldstack/blob/master/workspaces/templates-lib/packages/template-dynamodb/src/umzugDynamoDBStorage.ts)定义一个 DynamoDB 存储。

这样就可以定义 Umzug 迁移，既可以用来插入、删除和更新项目，也可以用来更新表本身以更新表设置或添加/删除索引( [migrations.ts](https://github.com/goldstack/dynamodb-boilerplate/blob/master/packages/dynamodb-1/src/migrations.ts) ):

以这种方式定义我们的表使我们能够使用 [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) 编写复杂的本地测试。

例如，在下面的测试中，模板库将在本地 DynamoDB 实例中创建一个表，并作为`[connect](https://github.com/goldstack/goldstack/blob/master/workspaces/templates-lib/packages/template-dynamodb/src/templateDynamoDBTable.ts#L80)`方法的一部分运行所有需要的迁移。

在我们的应用程序每次冷启动时，断言表存在和运行迁移只需要做一次。因此，`connect`方法保存了一个已经实例化的 DynamoDB 表的缓存(`[templateDynamoDBTable.ts#L80](https://github.com/goldstack/goldstack/blob/master/workspaces/templates-lib/packages/template-dynamodb/src/templateDynamoDBTable.ts#L80)`):

# 使用数据

为了在我们的应用程序中使用 DynamoDB，我们需要插入、检索和查询数据。最简单的方法是使用 [DynamoDB JavaScript SDK](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html) 。为此，我们只需要实例化类`AWS.DynamoDB`:

```
const dynamodb = new AWS.DynamoDB({apiVersion: '2012-08-10'});
```

这个类提供了修改表的配置(例如使用`[updateTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#updateTable-property)`)和处理数据的方法。通常，在我们的应用程序中，我们只想向表中写入和读取数据。为此，我们可以使用类`[AWS.DynamoDB.DocumentClient](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html)`。

在提供的示例项目和模板中，我创建了许多实用程序类，以使与 DynamoDB 的连接更容易(考虑到我们已经设置的基础设施)。我们不必自己实例化客户端，但可以使用如下包装方法:

其中`./table`引用项目中包含的文件`[table.ts](https://github.com/goldstack/dynamodb-boilerplate/blob/master/packages/dynamodb-1/src/table.ts)`。虽然用 DynamoDB 表连接起来并不太难，但是这些实用程序解决了我们最头疼的一个问题:本地测试。

DynamoDB 为本地运行 DynamoDB 的[提供了一个可执行文件](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html)。这些实用程序将透明地下载所需的 Docker 映像，并根据需要创建我们的表和运行迁移。这使得本地测试和编写单元测试变得非常容易。

当我们将代码部署到一个真实的环境中时，相应的方法将尝试与我们真正的 DynamoDB 实例连接。

在本文的第一部分，我们讨论了为 DynamoDB 定义数据模型。推荐的方法是所谓的[单表设计](https://aws.amazon.com/blogs/compute/creating-a-single-table-design-with-amazon-dynamodb/)。这只是我们在 DynamoDB 中构建数据的众多方式之一，在我们的代码中，遵循严格的单表设计很容易变得繁琐和难以实施。

[DynamoDB 工具箱](https://github.com/jeremydaly/dynamodb-toolbox)让我们很容易在代码中遵循单个表格的设计。为此，DynamoDB 工具箱要求我们为定义分区键和排序键的`[Table](https://github.com/jeremydaly/dynamodb-toolbox#installation-and-basic-usage)`定义一个覆盖。在示例项目中，这是在文件( [entities.ts](https://github.com/goldstack/dynamodb-boilerplate/blob/master/packages/dynamodb-1/src/entities.ts#L16) )中定义的:

注意，这些是我们在前面创建表时定义的相同的分区键和排序键。

DynamoDB 工具箱也允许我们定义我们想要使用的实体(`[entities.ts#L28](https://github.com/goldstack/dynamodb-boilerplate/blob/master/packages/dynamodb-1/src/entities.ts#L28)`):

最后，我们可以使用定义的实体和表来读写数据:

# 最后的想法

虽然 DynamoDB 的底层数据结构很简单，但是为一个无服务器的应用程序获得一个合适的 DynamoDB 设置是相当复杂的。在本文中，我试图涵盖让您开始使用 DynamoDB 所需的大部分基础知识。我还创建了一个模板和样板，希望可以帮助简化初始设置中的一些复杂问题；这样您就可以专注于数据建模和尽快编写应用程序逻辑。

我建议浏览样板项目中的代码， [dynamodb-1 包](https://github.com/goldstack/dynamodb-boilerplate/tree/master/packages/dynamodb-1)，并使用 [Goldstack 项目构建器](https://goldstack.party/build)来启动 Node.js 项目。当您将 [DynamoDB 模板](https://goldstack.party/templates/dynamodb)与后端(如[无服务器 API 模板](https://goldstack.party/templates/serverless-api)和前端(如 [Next.js 模板](https://goldstack.party/templates/nextjs))相结合时，这尤其有用，因为这将产生一个功能性的端到端 fullstack 项目。

如果您对改进本文中描述的方法以及模板中提供的方法有任何想法或反馈，欢迎通过[在 GitHub](https://github.com/goldstack/goldstack/issues) 上提出问题。

封面图片由 [Tobias Fischer](https://unsplash.com/photos/PkbZahEG2Ng) 拍摄