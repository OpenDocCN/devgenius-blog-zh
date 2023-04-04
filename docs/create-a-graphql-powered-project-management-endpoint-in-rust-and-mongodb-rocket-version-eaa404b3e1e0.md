# 在 Rust 和 MongoDB-Rocket 版本中创建一个基于 GraphQL 的项目管理端点

> 原文：<https://blog.devgenius.io/create-a-graphql-powered-project-management-endpoint-in-rust-and-mongodb-rocket-version-eaa404b3e1e0?source=collection_archive---------15----------------------->

![](img/a2c781d260e91b08c5ea1c221a9fb37f.png)

涉及

GraphQL 是一种用于读取和操作 API 数据的查询语言。它通过提供灵活、直观的语法来描述数据，从而优先向客户端或服务器提供准确的数据需求。

与传统的 REST API 相比，GraphQL 提供了一个类型系统来描述数据的模式，从而为 API 的消费者提供了使用单个端点来探索和请求所需数据的能力。

这篇文章将讨论使用 [Async-graphql](https://github.com/async-graphql/async-graphql) 库和 [MongoDB](https://www.mongodb.com/) 用 Rust 构建项目管理应用程序。在本教程的最后，我们将学习如何创建一个支持读取和操作项目管理数据的 GraphQL 端点，并使用 MongoDB 持久化我们的数据。

GitHub 库可以在[这里](https://github.com/Mr-Malomz/project-mngt-rust-graphql-rocket)找到。

# 先决条件

要完全掌握本教程中介绍的概念，需要有 Rust 方面的经验。使用 MongoDB 的经验不是必需的，但是拥有它是很好的。

我们还需要以下物品:

*   GraphQL 的基础知识
*   一个 [MongoDB 帐户](https://www.mongodb.com/)来托管数据库。 [**报名**](https://www.mongodb.com/cloud/atlas/register) **完全免费**

# 让我们编码

## 入门指南

首先，我们需要导航到所需的目录，并在我们的终端中运行下面的命令

```
cargo new project-mngt-rust-graphql-rocket && cd project-mngt-rust-graphql-rocket
```

该命令创建一个名为`project-mngt-rust-graphql-rocket`的 Rust 项目，并导航到项目目录。

接下来，我们通过修改`Cargo.toml`文件的`[dependencies]`部分来安装所需的依赖项，如下所示:

`rocket = {version = “0.5.0-rc.2”, features = [“json”]}`是一个基于 Rust 的框架，用于构建 web 应用。它还指定了所需的版本和特性类型(json)。

`async-graphql = { version = “4.0”, features = [“bson”, “chrono”] }`是一个用于在 Rust 中构建 GraphQL 的服务器端库。它还具有`bson`和`chrono`。

`async-graphql-rocket = “4.0”`是一个帮助 async-grapql 与 Rocket 集成的库。

`serde = “1.0.136”`是一个序列化和反序列化 Rust 数据结构的框架。例如，将 Rust 结构转换为 JSON。

`dotenv = “0.15.0”`是一个管理环境变量的库。

`[dependencies.mongodb]`是连接 MongoDB 的驱动程序。它还指定了所需的版本和特性类型(同步 API)。

我们需要运行下面的命令来安装依赖项:

```
cargo build
```

# 生锈的模块系统

模块就像我们应用程序中的文件夹结构；它们简化了我们管理依赖性的方式。

为此，我们需要导航到`src`文件夹，并创建`config`、`handler`和`schemas`文件夹及其对应的`mod.rs`文件来管理可见性。

![](img/e18adb48338053d58795719fa387a664.png)

`config`用于模块化配置文件。

`handler`用于模块化 GraphQL 逻辑。

`schemas`用于模块化 GraphQL 模式。

**添加对模块的引用**

为了使用模块中的代码，我们需要将它们声明为一个模块，并将它们导入到`main.rs`文件中。

# 设置 MongoDB

完成后，我们需要登录或注册我们的 [MongoDB](http://) 账户。点击项目下拉菜单并点击**新建项目**按钮。

![](img/d14f0fba428b88475c091c90cb9a7f63.png)

输入`projectMngt`作为项目名称，点击下一个**，点击**创建项目。****

![](img/1df22e872292a7c68113afdff3822ed1.png)![](img/41f6cfc4f5ccf5cad28797008f7112b5.png)

点击**建立数据库**

![](img/82237d251545f2961cf17bd5e0f16662.png)

选择**共享**作为数据库类型。

![](img/e4a523e05f1140905aedd8dda6643954.png)

点击**创建**来设置集群。这可能需要一些时间来设置。

![](img/3d773c2f6c3277d728094640ebd86134.png)

接下来，我们需要通过输入**用户名**、**密码**然后点击**创建用户**来创建一个从外部访问数据库的用户。我们还需要添加我们的 IP 地址，以便通过点击**添加我当前的 IP 地址**按钮安全地连接到数据库。然后点击**完成并关闭**保存更改。

![](img/84173cae2f3966f23691c00ef9ba3b20.png)![](img/13ff43ba8c3dbe87ce69f769f4a55f91.png)

保存更改后，我们应该会看到一个数据库部署屏幕，如下所示:

![](img/473380e87d8a6df6f05fd5f017bf96da.png)

# 将我们的应用程序连接到 MongoDB

配置完成后，我们需要将应用程序与创建的数据库连接起来。为此，点击**连接**按钮

![](img/05b13655d3ff2641b2cc556f69f45fd8.png)

点击**连接您的应用**，将**驱动**更改为`Rust`和**版本**，如下图所示。然后点击复制图标**复制连接字符串。**

![](img/4b05b3ea3d41cba99ab33d7fa542f401.png)![](img/f49f045f83220efb16385c09e718a99d.png)

**设置环境变量**

接下来，我们必须用之前创建的用户密码修改复制的连接字符串，并更改数据库名称。为此，首先，我们需要在根目录中创建一个`.env`文件，并在该文件中添加下面的代码片段:

```
MONGOURI=mongodb+srv://<YOUR USERNAME HERE>:<YOUR PASSWORD HERE>@cluster0.e5akf.mongodb.net/<DATABASE NAME>?retryWrites=true&w=majority
```

下面是正确填充的连接字符串示例:

```
MONGOURI=mongodb+srv://malomz:malomzPassword@cluster0.e5ahghkf.mongodb.net/projectMngt?retryWrites=true&w=majority
```

# 创建 GraphQL 端点

设置完成后，我们需要创建一个模式来表示我们的应用程序数据。为此，我们需要导航到`schemas`文件夹，在这个文件夹中，创建一个`project_schema.rs`文件并添加下面的代码片段:

上面的代码片段执行了以下操作:

*   导入所需的依赖项
*   使用`derive`宏生成对`Owner`、`CreateOwner`、`FetchOwner`、`Status`、`Project`、`CreateProject`和`FetchProject`的实现支持。该代码片段还使用来自`serde`和`async-graphql`库的程序宏来序列化/反序列化 Rust 结构并将其转换为 GraphQL 模式。

接下来，我们必须将`project_schema.rs`文件注册为`schemas`模块的一部分。为此，打开 schemas 文件夹中的`mod.rs`并添加下面的代码片段:

**数据库逻辑** 

*   创建项目所有者
*   获取所有所有者
*   获得单一所有者
*   创建项目
*   获取所有项目
*   获得单个项目

为此，首先，我们需要导航到`config`文件夹，在该文件夹中，创建一个`mongo.rs`文件并添加下面的代码片段:

上面的代码片段执行了以下操作:

*   导入所需的依赖项
*   创建一个带有`db`字段的`DBMongo`结构来访问 MongoDB 数据库
*   创建一个向`DBMongo`结构添加方法的实现块
*   向实现块添加一个`init`方法来加载环境变量，创建一个到数据库的连接，并返回一个`DBMongo`结构的实例
*   增加了一个`col_helper`方法；创建 MongoDB 集合的帮助函数

接下来，我们需要将剩余的方法添加到`DBMongo`实现中，以满足项目管理操作的需要:

上面的代码片段执行了以下操作:

*   添加一个`create_owner`方法，该方法接受一个`self`和`new_owner`作为参数，并返回创建的所有者或一个错误。在方法内部，我们使用`Owner`结构创建了一个新文档。然后我们使用`col_helper`方法创建一个新的集合，并访问`insert_one`函数来创建一个新的所有者并处理错误。最后，我们返回了创建的所有者信息
*   添加一个`get_owners`方法，该方法接受一个`self`作为参数，并返回所有者列表或一个错误。在该方法中，我们使用`col_helper`方法创建一个新的集合，并在没有任何过滤器的情况下访问`find`函数，以便它可以匹配数据库中的所有文档，并使用`map()`方法循环遍历所有者列表，以最佳方式返回列表，并处理错误
*   添加一个`single_owner`方法，该方法接受一个`self`和`id`作为参数，并返回所有者详细信息或错误。在方法内部，我们将`id`转换为`ObjectId`，并将其用作过滤器来获取匹配的文档。然后，我们使用`col_helper`方法创建一个新的集合，并从该集合中访问`find_one`函数，以获取所有者的详细信息并处理错误
*   添加一个`create_project`方法，该方法接受一个`self`和`new_project`作为参数，并返回创建的项目或一个错误。在方法内部，我们使用`Project`结构创建了一个新文档。然后我们使用`col_helper`方法创建一个新的集合，并访问`insert_one`函数来创建一个新的项目并处理错误。最后，我们返回了创建的项目信息
*   添加一个`get_projects`方法，该方法接受一个`self`作为参数，并返回项目列表或一个错误。在该方法中，我们使用`col_helper`方法创建一个新的集合，并在没有任何过滤器的情况下访问`find`函数，这样它就可以匹配数据库中的所有文档，并使用`map()`方法循环遍历项目列表并处理错误，从而以最佳方式返回列表
*   添加一个`single_project`方法，该方法接受一个`self`和`id`作为参数，并返回项目细节或一个错误。在方法内部，我们将`id`转换为`ObjectId`，并将其用作过滤器来获取匹配的文档。然后，我们使用`col_helper`方法创建一个新的集合，并从集合中访问`find_one`函数，以获取项目的详细信息并处理错误

最后，我们必须将`mongo.rs`文件注册为`config`模块的一部分。为此，打开 config 文件夹中的`mod.rs`,并添加以下代码片段:

**GraphQL 处理程序** 随着数据库逻辑的整理，我们可以开始使用它们来创建我们的 GraphQL 处理程序。为此，首先，我们需要导航到`handler`文件夹，在这个文件夹中，创建一个`graphql_handler.rs`文件并添加下面的代码片段:

上面的代码片段执行了以下操作:

*   导入所需的依赖项
*   创建一个`Query`结构，其实现方法与使用数据库逻辑中的相应方法查询数据库相关
*   创建一个`Mutation`结构，其实现方法与使用数据库逻辑中的相应方法修改数据库相关。
*   创建一个`ProjectSchema`类型来构造我们的 GraphQL 如何使用`Query`结构、`Mutation`结构和`EmptySubscription`，因为我们没有任何订阅。

**创建 GraphQL 服务器** 最后，我们可以通过将`ProjectSchema`和 MongoDB 与 Actix web 集成来开始创建我们的 GraphQL 服务器。为此，我们需要导航到`main.rs`文件并修改它，如下所示:

上面的代码片段执行了以下操作:

*   导入所需的依赖项
*   使用`get`程序宏创建一个`graphql_query`函数来指定 GraphQL 路径，并使用`ProjectSchema`类型来执行与查询数据库相关的方法
*   使用`post`程序宏创建一个`graphql_mutation`函数来指定 GraphQL 路径，并使用`ProjectSchema`类型来执行与修改数据库相关的方法
*   创建一个`graphql_playground`函数来创建**GraphiQL**；我们可以通过浏览器访问的 GraphQL 游乐场
*   使用`#[rocket::launch]`宏运行 rocket 函数，该函数生成应用程序入口点并运行服务器。rocket 函数还执行以下操作:
    –创建一个`db`变量，通过调用`init()`方法建立与 MongoDB 的连接，并使用它构建一个 GraphQL 数据
    –使用`build`函数构建应用程序，将模式添加到 state，并将`route`配置为包含`graphql_query`、`graphql_mutation`和`graphql_playground`

完成后，我们可以通过在终端中运行下面的命令来测试我们的应用程序。

```
cargo run
```

然后在网络浏览器上导航至`[http://127.0.0.1:8000](http://127.0.0.1:8000)`。

![](img/91eaec20abbfb72b91569c0c936a397d.png)

创建项目

![](img/eb8b0333431ce9b66290adb73f3fd4d7.png)

获取项目列表

![](img/acb931e86b0440d03b37685eec68c024.png)

获得单一所有者

![](img/702b725d088ae2458cc08d248195e46a.png)

创建所有者

![](img/aaf1a11759b3567e680c2db36f3e03cd.png)

有数据的数据库

# 结论

这篇文章讨论了如何模块化 Rust 应用程序，构建 GraphQL 服务器，并使用 MongoDB 持久化我们的数据。

这些资源可能会有所帮助:

*   [异步图 QL](https://github.com/async-graphql/async-graphql)
*   [火箭](https://rocket.rs/)
*   [MongoDB Rust 驱动](https://github.com/mongodb/mongo-rust-driver)
*   [Async-GraphQL 火箭集成](https://github.com/async-graphql/async-graphql/tree/master/integrations/rocket)
*   [Serde(序列化和反序列化库)](https://serde.rs/)