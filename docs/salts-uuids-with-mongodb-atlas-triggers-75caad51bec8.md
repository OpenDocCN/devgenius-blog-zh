# 带有 MongoDB 和 ATLAS 触发器的 Salts 和 UUIDs

> 原文：<https://blog.devgenius.io/salts-uuids-with-mongodb-atlas-triggers-75caad51bec8?source=collection_archive---------7----------------------->

在使用 MongoDB 和 ATLAS 集群触发器时，UUIDs 和 Salts & Hashes 的方便实用的实现。

![](img/bb946aa83221ec162d43ab61194e7144.png)

# 介绍

这篇文章是在 MongoDB-ATLAS 集群中实际实现 UUIDs 和 Salts 的一个例子。将讨论的主题是:

*   MongoDB _id 键/字段
*   使用标准 uuid——**MongoDB uuid()函数**
*   使用 MongoDB **ATLAS 触发器**
*   使用 **npm 外部依赖** (uuid 和 bcrypt)与 ATLAS **触发函数**
*   使用 bcrypt 和 MongoDB ATLAS 触发函数进行散列和加盐

# 先决条件

假设你理解基本的数据库概念， [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) 和[密码加盐和散列](https://www.okta.com/uk/blog/2019/03/what-are-salted-passwords-and-password-hashing/)。我之前的帖子对这些主题有所了解:

[](https://medium.com/@zzpzaf.se/salts-and-uuids-for-your-databases-intro-41cc58fe100a) [## 所有数据库的 Salts 和 UUIDs

### 这是一篇关于 Salts 和 UUIDs 的介绍性文章，也是我的其他文章中关于如何在…

medium.com](https://medium.com/@zzpzaf.se/salts-and-uuids-for-your-databases-intro-41cc58fe100a) 

此外，您应该对 MongoDB 有足够的了解，它是一个[非 SQL](https://www.mongodb.com/nosql-explained) 基于文档的数据库[。](https://www.mongodb.com/document-databases)

此外，为了理解本文的例子，您必须能够访问一个正在运行的 MongoDB 实例和 CLI 工具 [mongosh](https://www.mongodb.com/docs/mongodb-shell/) 。[ [在这里](https://www.devxperiences.com/pzwp1/2022/10/30/the-database-command-line-tools-you-can-add-to-your-dev-environment-without-database-installation/#mongosh)你可以找到如何将它作为一个独立的工具安装在你的系统中]。如果你愿意的话，你可以看看我下面关于如何创建和开始使用 [MongoDB Docker 容器](https://medium.com/@zzpzaf.se/mongodb-in-docker-bfa77346b389)的帖子。

[](https://medium.com/@zzpzaf.se/mongodb-in-docker-bfa77346b389) [## Docker 中的 MongoDB

### 4 分钟指南。轻松全面！

medium.com](https://medium.com/@zzpzaf.se/mongodb-in-docker-bfa77346b389) 

或者，您可以访问 MongoDB ATLAS 数据库集群。为此，你也可以使用我的另一个帖子，看看你能多快创建一个 [**免费共享数据库集群**](https://medium.com/@zzpzaf.se/mongodb-atlas-free-shared-database-cluster-891435bec3a9) :

[](https://medium.com/@zzpzaf.se/mongodb-atlas-free-shared-database-cluster-891435bec3a9) [## MongoDB Atlas 免费共享数据库集群

### 关于在 MongoDB ATLAS 云平台上使用免费共享数据库集群的快速介绍。

medium.com](https://medium.com/@zzpzaf.se/mongodb-atlas-free-shared-database-cluster-891435bec3a9) 

对于那些来自 [RDBMS](https://en.wikipedia.org/wiki/Relational_database#RDBMS) 世界的人来说，值得一提的是，MongoDB 数据库对象类似于 RDBMS 模式或数据库，包含表、视图和其他 RDBMS 对象。MongoDB 集合类似于一个表，而 MongoDB 文档可以被视为一个表行。因此，MongoDB 数据库可以将集合组合在一起，一个集合保存文档，最后，一个文档由许多键值对的对象和/或其他文档组成。

现在是时候开始了，首先看看 MongoDB 是如何处理惟一 id 的。

# MongoDB _id 键/字段

每次在集合中插入新文档时，MongoDB 都会自动生成一个特殊的( **_id** )属性/键/字段。在插入新文档时，您可以选择使用自己的 _id 值，但是在大多数情况下，我们让 MongoDB 自动生成它。

id 是 MongoDB 的一种特殊数据类型。它实际上是一个 12 字节大小的 [BSON](https://www.mongodb.com/basics/bson) 类型的 MongoDB 对象( [ObjectID](https://www.mongodb.com/docs/manual/reference/method/ObjectId/) )。12 字节 id 由以下内容组成:

*   4 个字节表示自 Unix 纪元以来的秒数
*   特定于主机的 3 个字节—机器标识符
*   2 字节的进程 id，以及
*   3 个字节代表一个计数器，从一个随机值开始

[![](img/d54f702a99f1d8d36dba76ed8325db3e.png)](https://www.mongodb.com/developer/products/mongodb/bson-data-types-objectid/)

[https://www . MongoDB . com/developer/products/MongoDB/bson-data-types-objectid/](https://www.mongodb.com/developer/products/mongodb/bson-data-types-objectid/)

可以认为 _id 字段是唯一的。它们是有序的(因为末尾有计数器)，默认情况下，它们被用作 MongoDB 集合的“主键”。

当您通过最初插入几个文档来创建一个新的集合时，就像我们使用下面的命令一样，

```
db.demousers.insertOne**({**“username”: “panos”,”password”:”panospassw1"**})**db.demousers.insertOne**({**“username”: “panos2”,”password”:”panospassw2"**})**
```

_ ids 自动生成为**插入 Id** :

![](img/980de5ab10384124e692a30b18a94858.png)

正如您在上面看到的，auto-create _ ids 具有连续的值:

![](img/6e0a315ebf58ea2d95a7d0133be123c2.png)

# 使用标准 uuid——MongoDB uuid()函数

然而，正如您所理解的，它们实际上不是标准的 UUIDs。因此，如果您希望在您的集合中使用真正的 UUID，MongoDB 为我们提供了[**【UUID()**](https://www.mongodb.com/docs/manual/reference/method/UUID/)内置函数。UUID()函数是非常灵活的，允许我们使用它或者有或者没有可选的 UUID 字符串(例如，以前由后端/中间件生成)。如果我们在没有任何可选参数的情况下使用它，MongoDB 会以 [RFC 4122 v4](https://tools.ietf.org/html/rfc4122) 格式生成一个随机 UUID。

让我们看看它是如何工作的:

```
db.demousers.insertMany**([{**“id”: UUID**()**,”username”: “panos”,”password”:”panospassw1"**}**, **{**“id”: UUID**()**,”username”: “panos2”,”password”:”panospassw2"**}])**
```

或者，如果不希望使用单独的字段(例如 id)，可以使用 UUID()函数将一个真实的 UUID 值传递给 MongoDB (_id)字段，如下所示:

```
db.demousers.insertOne**({**“username”: “panos”,”password”:”panospassw1"**})** db.demousers.insertOne**({**“username”: “panos2”,”password”:”panospassw2"**})**
```

# 使用 MongoDB ATLAS 触发器

## 使用 ATLAS 数据库集群

到目前为止，一切顺利！但是我们如何让 MongoDB 自动完成呢？例如，当在集合中插入新文档时，不使用 UUID()函数作为字段的值？

嗯，在一个简单的 MongoDB 独立版本中没有办法做到这一点。人们可能会认为，通过某种方式将一组适当的模式验证规则与 BSON 类型结合起来，有可能做到这一点。然而，不幸的是，这是不支持的。至少在撰写本文之前， [MongoDB JSON 模式](https://www.mongodb.com/docs/manual/reference/operator/query/jsonSchema/)

验证规则[省略了](https://www.mongodb.com/docs/manual/reference/operator/query/jsonSchema/#omissions)JSON 模式草案 4 的一些标准特性和关键字。其中，$ **是默认的**关键字，它可以完成这项工作。

我们有其他选择吗？嗯，我们有一些选择。一种选择是使用我们的后端/中间件层(不使用或使用 [ORM/ODM](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping) ，比如 NodeJS 应用程序中的[mongose](https://mongoosejs.com/))。这是可以的，我们可以这样做。但是，如果我们希望自动化和分离流程，我们可以使用 [**触发器**](https://en.wikipedia.org/wiki/Database_trigger) 将该任务分配给数据层。

独立的 Mongo DB 版本还不支持触发器。然而， [MongoDB ATLAS](https://www.mongodb.com/atlas) 的云服务支持它们。请注意，触发器(数据库、认证和预定触发器)在 MongoDB Atlas 集群中可用，运行 MongoDB 3.6 版或更高版本。对于我们的例子，我们需要一个 [**数据库触发器**](https://www.mongodb.com/features/database-triggers) 在新文档插入时被触发。

首先，如果你还没有这样做，请到 MongoDB Atlas 的[注册页面](https://www.mongodb.com/cloud/atlas/register)注册一个免费的共享数据库集群。你可以看看我的这个[帖子](https://www.devxperiences.com/pzwp1/2022/10/12/mongodb-atlas-free-shared-database-cluster%ef%bf%bc/)(我之前也提过)做个快速指南。

在创建了免费的共享数据库集群之后，您可以使用本地安装的 [mongosh](https://www.mongodb.com/docs/mongodb-shell/) CLI 来测试它。[注意:你可以在这里看到如何在你的系统中只安装 **mongosh** ，而不安装 MongoDB]。

您可以使用类似如下的脚本进行连接:

```
~$ mongosh “mongodb+srv://cluster0.hrqghnf.mongodb.net/ticket-management” — apiVersion 1 — username user1 — password u1passw1
```

现在我们准备创建我们的第一个 [**触发器**](https://en.wikipedia.org/wiki/Database_trigger) 。在这里，我们将创建一个**数据库触发器**。在官方网站上，你可以找到关于[数据库触发器](https://www.mongodb.com/docs/atlas/app-services/triggers/database-triggers/?_ga=2.166281719.832627120.1665337667-467948862.1664634787)的非常有用的信息，还有关于[数据库触发器](https://www.mongodb.com/features/database-triggers)以及如何[配置数据库触发器](https://www.mongodb.com/docs/atlas/triggers/?_ga=2.137326073.832627120.1665337667-467948862.1664634787#configure-database-triggers)的综合指南。

MongoDB Atlas 数据库触发器应该由用 **JavaScript** 编写的函数驱动。数据库触发器在以下四种类型的事件时被触发:**插入**、**更新**、**删除**和**替换**。这里，我们实际需要的是在发生**插入**事件时触发触发器。

因此，回到 MongoDB ATLAS 管理控制台，单击左侧窗格中的**触发器**链接。由于到目前为止我们还没有创建触发器，系统将提示您添加新的触发器:

![](img/88ba503582f5b452717453b55bb40818.png)

然后，为您的触发器选择选项，例如触发器名称、是否应该启用它、是否遵循事件顺序(如果您定义了多个触发器)、您的集群、您的数据源/数据库名称、触发器是否将处理整个文档、操作(事件)类型等。你可以在官方文档中找到关于数据库**触发器配置**的详细信息，[在这里](https://www.mongodb.com/docs/atlas/triggers/trigger-configuration/#std-label-database-trigger-configuration)。

重要的部分位于屏幕的底部。这是编辑窗口，在这里我们可以定义**触发功能**。

![](img/d14675ab16f0e80b0fd20623c2fee69a.png)

这是我们的触发事件将触发的函数(这里是关于**插入**事件)。正如我们所说的，我们应该使用普通的 JavaScript 来定义这个函数。正如您在上面看到的，ATLAS 将" **changeEvent** "对象作为触发函数中的唯一参数进行传递。

对象为我们提供了新插入文档的所有相关信息(id、字段、时间戳等。).该对象具有与“**新**相似的作用。<字段/列>

数据库更改事件对象具有以下一般形式:

所以，我们到了！回想一下，我们想要的是创建一个触发函数，将自动生成的 UUID 值设置为 ID 字段。因此，假设在我们的集合中插入任何新文档时，我们希望用这样的 UUID 值设置文档的“uid”字段。

人们可以认为你可以使用我们之前见过的 [MongoDB uuid()](https://www.mongodb.com/docs/manual/reference/method/UUID/) 函数。然而，我不这么认为。我看不出我们如何从一个触发函数中访问一个数据库函数，比如 MongoDB uuid()函数。我们可以访问的只是“ **changeEvent** 对象及其字段。所以，我们最初的选择是使用内置的 JavaScript 函数，比如[日期](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)和[数学](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)函数。然后添加一些[regex](https://en.wikipedia.org/wiki/Regular_expression)‘炼金术’，我们可以有一个 UUID 兼容的值。下面，您可以用 JavaScript 代码找到这样一个可以完成这项工作的实现:

之后，您可以通过插入一个新文档(例如使用 mongosh)来检查我们的触发器是否正常工作:

```
db.demousers.insertOne**({**“username”: “panos1”,”password”:”passw1"**})**
```

![](img/a6005269db0e91441007ddb3df68b6a9.png)

此外，您可以在下面看到嵌入式 MongoDB uuid()函数和触发器动作之间的区别:

![](img/0733112c2d0ba712af3c8a7b7fae3775.png)

这个工作正常。但是，我们可以使用现有的“_id”字段，而不是新的“uid”字段吗？答案是否定的。

尝试更新/更改 _id 字段，例如{ $set: { _id: myUUID() } }时出现错误。使用触发器日志查看它:

![](img/5a18acc66615ff41a86e497bad335312.png)

所以正如你所见。让我们进行一些改进。

到目前为止一切顺利。然而，更好的方法是避免使用正则表达式字符串“炼金术”的缺点，并使用标准的 UUID 值。例如，这可以通过使用标准的 npm 包来完成，例如 **npm** [uuid()](https://www.npmjs.com/package/uuid) 模块。我们如何做到这一点？

# 将 npm 模块用作外部依赖项(uuid 和 bcrypt)

MongoDB ATLAS 支持外部依赖，比如标准的 [npm](https://www.npmjs.com/) 包。因此，我们可以添加我们更喜欢的依赖项，并在触发函数中使用它们。(在[官方文档](https://www.mongodb.com/docs/atlas/triggers/external-dependencies/#std-label-external-dependencies)处阅读更多内容)。一般来说，有两个选项可以将 npm 模块安装为 Atlas 触发器的依赖项。一种选择是使用 Atlas GUI，并使用在 npm 存储库中找到的模块名称逐个添加它们。然而，如果需要使用大量的依赖项，那么更好的方法是在本地安装它们，然后将它们作为压缩的 **.tar.qz** 文件上传。在这里，我们将使用真实的案例来看看我们如何做到这一点的基本步骤。

**我们的案例依赖关系**

[uuid](https://www.npmjs.com/package/uuid) 包支持 [RFC4122](https://www.ietf.org/rfc/rfc4122.txt) 版本 1、3、4 和 5 的 uuid，因此，对于我们的目的来说，这可能是一个很好的选择。此外，因为我们稍后要处理**密码散列和加盐**，所以 [bcrypt.js](https://www.npmjs.com/package/bcryptjs) (或者这里的)模块是我们的首选，主要是因为它没有依赖性。因此，我们还将 bcrypt.js 模块与 uuid 模块一起添加。

所以，让我们开始添加那些包。你必须使用你的本地计算机。

首先，为您的依赖项创建一个文件夹(例如，将其命名为“atlasup”)。然后，安装依赖项 uuid 和 bcrypt(和/或您希望的任何其他模块)。我们将使用标准的 [npm 包管理器](https://www.npmjs.com/package/npm)来安装它们。

```
npm i uuid
npm i bcrypt.js
```

在本地安装完软件包后，为整个 node_modules 子文件夹创建一个压缩(. tar.qz)文件:

![](img/ebf52bf0704817106ef156f241509efb.png)

就是这样。最后一步是通过 ATLAS Manager 控制台上传 create .tar.qz 文件。以下截图展示了如何做到这一点:

![](img/34732dfba925433d43a324d03029410c.png)![](img/d9b27a67414629fc319fe0cd19b30f66.png)![](img/d646590a84f5932744f93da4200fce66.png)![](img/3bdba69abc33cb045e579e97f5778d39.png)![](img/6da4a46161dcf72cc126626781126899.png)

上传后，您可以在 ATLAS Dependencies 窗口中看到 **node_modules** 包的所有依赖项。就是这样！现在，我们可以使用它们了。

但是，在使用它们之前，您应该了解 ATLAS 应用服务的以下两个限制点:

*   目前还不支持 ES6 (import)语法，所以必须使用 CommonJS(“require”)语法。
*   也不支持带有触发器函数编辑器窗口的全局范围。您必须将“require”语句放在函数范围内。

也就是说，让我们看看如何使用 uuid 依赖包编写我们的触发函数。

仅此而已！现在我们可以通过在集合中插入一个新文档来测试它:

![](img/823ca7afef9e585d2f6555c7b0772522.png)

酷！它像我们预期的那样工作。不再有 UUIDs。腌制和腌制的时间到了！

# 使用 bcrypt 和 MongoDB ATLAS 触发函数进行散列和加盐

正如我们所说的，我们将使用 bcrypt.js 依赖项。出于我们的目的，我们既可以创建一个新的触发器和函数，也可以修改现有的触发器。这里，我们将**修改现有的触发器**及其功能。因此，点击它，或通过按下操作栏(右端)的 3 点按钮选择“编辑触发器”。

![](img/334e3282820748f6b2f5fef5462d9420.png)

首先，我们将现有触发器' trigger_uid '重命名为' insert _ trigger _ uid _ and _ passw _ has h1 ',以了解它的作用。之后，确保其设置:

*   名称:插入触发器 uid 和密码 hash1
*   启用:应该打开
*   重新启用时跳过事件:我们现在不关心，让它关闭
*   事件排序:同样，我们现在不关心，因为我们只有一个插入事件的触发器。
*   链接数据源:留空—我们没有/使用外部链接
*   集群名称:集群 0
*   数据库名称:票证管理
*   集合名称:demousers
*   操作类型:只应勾选插入
*   完整文档:应该打开(我们需要在修改文档字段之前访问它们——实际上我们需要用户提供的密码值
*   文档前像:保持关闭状态，因为我们只使用了一个插入触发器。
*   选择一个事件类型:当然要选择函数。

最后，我们可以继续更新它的功能。没什么可做的。我们只需使用“require”语法来导入 bcrypt 库。然后我们可以使用 changeEvent 对象提供的 fullDocument 对象，并获得明文密码。然后我们使用 bcrypt 创建一个**默认 salted** 哈希值。使用 [bcrypt.js](https://github.com/dcodeIO/bcrypt.js) ，我们可以首先通过 **bcrypt.genSaltSync** ()函数创建一个 salt create，或者我们可以通过 **bcrypt.hashSync()** 函数直接创建一个自动生成并添加 salt 的哈希值——这就是我们在这里要做的。

最后，我们更新(设置)uid 和密码字段的值。整个函数如下所示:

就是这样。让我们通过向 demousers 集合添加一个新用户来测试它。

访问您的终端并使用 **mongosh** 连接到您的 ATLAS 集群和票务管理数据库。然后使用 insertOne()函数，如下所示:

![](img/4dcbb6e0e17912f34901eb2a24aeb8a2.png)

似乎总是需要一些明显的时间来触发行动。–您无法立即察觉触发效应。因此，正如您在上面看到的，第一次查询您的收藏时，您将看不到任何结果。

该触发器在 ATLAS **日志**中的条目，意味着该触发器完成了他的工作。

![](img/eb43ff60470af11c4e8fa510c3ff1118.png)

因此，重新查询集合后，您可以看到结果:

![](img/85a1be4683adeb2dba81658bf7a20ecc.png)

酷！它工作了。你可以使用任何 [bcrypt 在线验证器](https://bcrypt.online/)来检查它，例如:

![](img/f7a1779fef512bda02deb22c1a611392.png)

暂时就这样吧！我希望你喜欢它！
感谢阅读👏敬请关注！

PS:继续阅读我的其他帖子，查看其他数据库的实现示例。