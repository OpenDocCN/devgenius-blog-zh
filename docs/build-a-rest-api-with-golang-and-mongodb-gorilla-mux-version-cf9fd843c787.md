# 用 Golang 和 MongoDB 构建 REST API—Gorilla/Mux 版本

> 原文：<https://blog.devgenius.io/build-a-rest-api-with-golang-and-mongodb-gorilla-mux-version-cf9fd843c787?source=collection_archive---------7----------------------->

![](img/b4ac4c8a4264bc7a24c9b88d4b64eba5.png)

封面照片

表述性状态转移(REST)是一种指导应用程序编程接口(API)设计和开发的架构模式。REST APIs 已经成为产品的服务器部分和它的客户机之间的通信标准，以提高性能、可伸缩性、简单性、可修改性、可见性、可移植性和可靠性。

这篇文章将讨论用 Golang 使用 [Mux](https://github.com/gorilla/mux) 包和 [MongoDB](https://www.mongodb.com/) 构建一个用户管理应用程序。在本教程的最后，我们将学习如何构建一个 Mux 应用程序，构建一个 REST API，并使用 MongoDB 持久化我们的数据。

Gorilla/Mux，俗称 **Mux** ，是一个强大的 HTTP 路由器和 URL 匹配器，用于构建 Go web 服务器。

MongoDB 是一个基于文档的数据库管理程序，用作关系数据库的替代方案。MongoDB 支持处理大型分布式数据集，并提供无缝存储或检索信息的选项。

您可以在这个[库](https://github.com/Mr-Malomz/mux-mongo-api)中找到完整的源代码。

# 先决条件

这篇文章中的以下步骤需要 Golang 经验。使用 MongoDB 的经验不是必需的，但是拥有它是很好的。

我们还需要以下物品:

*   一个 [MongoDB 帐户](https://www.mongodb.com/)来托管数据库。 [**报名**](https://www.mongodb.com/cloud/atlas/register) **完全免费**。
*   [Postman](https://www.postman.com/downloads/) 或者你选择的任何 API 测试应用

# 让我们编码

## 入门指南

首先，我们需要导航到所需的目录，并在我们的终端中运行下面的命令

```
mkdir mux-mongo-api && cd mux-mongo-api
```

该命令创建一个`mux-mongo-api`文件夹，并导航到项目目录。

接下来，我们需要通过运行以下命令来初始化 Go 模块以管理项目依赖关系:

```
go mod init mux-mongo-api
```

该命令将创建一个`go.mod`文件，用于跟踪项目依赖关系。

我们继续安装所需的依赖项:

```
go get -u github.com/gorilla/mux go.mongodb.org/mongo-driver/mongo github.com/joho/godotenv github.com/go-playground/validator/v10
```

`github.com/gorilla/mux`是一个用于构建网络服务器的软件包。

`go.mongodb.org/mongo-driver/mongo`是连接 MongoDB 的驱动。

`github.com/joho/godotenv`是一个管理环境变量的库。

`github.com/go-playground/validator/v10`是用于验证结构和字段的库。

# 应用程序入口点

安装了项目依赖项后，我们需要在根目录下创建`main.go`文件，并在下面添加代码片段:

上面的代码片段执行以下操作:

*   导入所需的依赖项。
*   使用`NewRouter`功能初始化多路路由器。
*   使用将`net/http`包作为参数路由到`/`路径的`HandleFunc`函数，以及使用`encoding/json`包将头类型设置为 **JSON** 并返回`Hello from Mux & mongoDB`的 JSON 的处理函数。我们还将 HTTP 方法附加到这个函数上
*   使用`http.ListenAndServe`功能在端口`6000`上运行应用程序。

接下来，我们可以通过在终端中运行下面的命令来启动开发服务器，从而测试我们的应用程序。

```
go run main.go
```

![](img/73f9fc18155588b4856dc046a7e74dc4.png)

测试应用程序

# 戈朗的模块化

对于我们的项目来说，有一个好的文件夹结构是至关重要的。良好的项目结构简化了我们在应用程序中处理依赖关系的方式，并使我们和其他人更容易阅读我们的代码库。

为此，我们需要在项目目录中创建`configs`、`controllers`、`models`、`responses`和`routes`文件夹。

![](img/8a0864a7a81d8431225ca2a396d09de5.png)

更新了文件夹结构

**PS**:*`*go.sum*`*文件包含所有的依赖校验和，由 go 工具管理。我们不必担心它。**

*`configs`用于项目配置文件的模块化*

*`controllers`用于应用逻辑的模块化。*

*`models`用于数据和数据库逻辑的模块化。*

*`responses`用于模块化描述我们希望我们的 API 给出的响应的文件。这将在以后变得更加清晰。*

*`routes`用于模块化 URL 模式和处理程序信息。*

# *设置 MongoDB*

*完成后，我们需要登录或注册我们的 [MongoDB](https://www.mongodb.com/) 账户。点击项目下拉菜单，点击**新建项目**按钮。*

*![](img/9988d8e4c6748045a6fb2a6b7e511f94.png)*

*输入`goland-api`作为项目名称，点击下的**，点击**创建项目。*****

*![](img/e91e2d034eb7b8dc873d4f5e7bd80c77.png)**![](img/03816a381c1fa0844dbd0c87e5521fa3.png)*

*点击**建立数据库***

*![](img/44a5ecb824fa315627d0da7fa58b4bf3.png)*

*选择**共享**作为数据库类型。*

*![](img/1904585faf9df2ae69542d6a95c2756f.png)*

*点击**创建**以设置集群。这可能需要一些时间来设置。*

*![](img/51640f3f5ca538912ef21d743d0ac0dd.png)*

*接下来，我们需要创建一个用户，通过输入**用户名**、**密码**，然后点击**创建用户**，从外部访问数据库。我们还需要添加我们的 IP 地址，以便通过点击**添加我当前的 IP 地址**按钮安全地连接到数据库。然后点击**完成并关闭**保存更改。*

*![](img/44e0947da796557590cce6fca9a78b1b.png)**![](img/10259814e28f3fe085e5586eca711dcc.png)*

*保存更改后，我们应该会看到一个数据库部署屏幕，如下所示:*

*![](img/44cbc6a75a6c909a5479194562e8789a.png)*

# *将我们的应用程序连接到 MongoDB*

*配置完成后，我们需要将应用程序与创建的数据库连接起来。为此，点击**连接**按钮*

*![](img/89ce6de722015a086fd97234af5de845.png)*

*点击**连接你的应用**，改变**驱动**和**版本**，如下图所示。然后点击复制**图标**复制连接字符串。*

*![](img/89ace11598110960b4844d0a28fcd8b9.png)**![](img/b094533eb5d8235f47d7d0b4e62cd9cf.png)*

***设置环境变量***

*接下来，我们必须用之前创建的用户密码修改复制的连接字符串，并更改数据库名称。为此，首先，我们需要在根目录中创建一个`.env`文件，并在该文件中添加下面的代码片段:*

```
*MONGOURI=mongodb+srv://<YOUR USERNAME HERE>:<YOUR PASSWORD HERE>@cluster0.e5akf.mongodb.net/myFirstDatabese?retryWrites=true&w=majority*
```

*下面是正确填充的连接字符串示例:*

```
*MONGOURI=mongodb+srv://malomz:malomzPassword@cluster0.e5akf.mongodb.net/golangDB?retryWrites=true&w=majority*
```

*![](img/97535478d225754aad2002806f616911.png)*

***加载环境变量***

*完成后，我们需要创建一个助手函数，使用我们之前安装的 github.com/joho/godotenv 库来加载环境变量。为此，我们需要导航到 configs 文件夹，在这个文件夹中，创建一个`env.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个 EnvMongoURI 函数，该函数检查环境变量是否正确加载并返回环境变量。*

***连接到 MongoDB***

*要从我们的应用程序连接到 MongoDB 数据库，首先，我们需要导航到 configs 文件夹，在这个文件夹中，创建一个 setup.go 文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个`ConnectDB`函数，首先配置客户端使用正确的 URI 并检查错误。其次，我们定义了一个 10 秒的超时，我们希望在尝试连接时使用。第三，检查连接到数据库时是否有错误，如果连接时间超过 10 秒，则取消连接。最后，我们 pinged 数据库来测试我们的连接，并返回了`client`实例。*
*   *创建一个`ConnectDB`的`DB`变量实例。这将在创建收藏时派上用场。*
*   *创建一个`GetCollection`函数来检索并在数据库上创建`collections`。*

*接下来，我们需要在应用程序启动时连接到数据库。为此，我们需要修改`main.go`,如下所示:*

# *设置 API 路由处理程序和回应类型*

***路线处理程序** 完成后，我们需要在`routes`文件夹中创建一个 user_route.go 文件来管理应用程序中所有与用户相关的路线，如下所示:*

*接下来，我们需要将新创建的路由附加到 **http。通过修改服务器`main.go`中的**，如下所示:*

*接下来，我们需要创建一个可重用的结构来描述我们的 API 的响应。为此，导航到`responses`文件夹，在该文件夹中，创建一个`user_response.go`文件并添加下面的代码片段:*

*上面的代码片段创建了一个 UserResponse 结构，它具有`Status`、`Message`和`Data`属性来表示 API 响应类型。*

***PS** : `json:"status"` *，* `*json:"message”*` *，* `*json:”data”*` *称为* ***struct 标签*** *。结构标签允许我们将元信息附加到相应的结构属性上。换句话说，我们使用它们来重新格式化 API 返回的 JSON 响应。**

# *最后，创建 REST API*

*接下来，我们需要一个模型来表示我们的应用程序数据。为此，我们需要导航到`models`文件夹，在该文件夹中，创建一个`user_model.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个具有所需属性的结构。我们在 struct，tag 中添加了`omitempty`和`validate:”required”`,分别告诉纤程忽略空字段并使字段成为必填字段。*

***创建用户端点** 有了模型设置，我们现在可以创建一个函数来创建用户。为此，我们需要导航到`controllers`文件夹，在这个文件夹中，创建一个`user_controller.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建`userCollection`和`validate`变量，分别使用我们之前安装的`github.com/go-playground/validator/v10`库来创建集合和验证模型。*
*   *创建一个返回`net/http`处理程序的`CreateUser`函数。在返回的处理程序中，当将用户插入到文档中时，我们首先定义了一个 10 秒的超时，使用验证器库来验证请求体和必填字段。我们使用之前创建的`UserResponse`结构返回了适当的消息和状态代码。其次，我们创建了一个`newUser`变量，使用`userCollection.InsertOne`函数插入它，并检查是否有错误。最后，如果插入成功，我们返回正确的响应。*

> *`w.WriteHeader`功能用于设置 API 状态码。*
> 
> *`json.NewDecoder‘s`解码和编码方法用于将 **JSON** 转换为 **Go** 值，反之亦然。*

*接下来，我们需要用路由 API URL 和相应的控制器更新 user_routes.go，如下所示:*

***获取用户端点** 要获取用户的详细信息，我们需要修改 user_controller.go，如下所示:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个返回一个`net/http`处理程序的`GetAUser`函数。在返回的处理程序中，我们首先定义了在文档中查找用户时 10 秒的超时时间、从 URL 参数中获取用户 Id 的 userId 变量和一个用户变量。我们将`userId`从字符串转换为`primitive.ObjectID`类型，即 MongoDB 使用的 **BSON** 类型。其次，我们使用`userCollection.FindOne`搜索用户，将`objId`作为过滤器传递，并使用`Decode`属性方法获取相应的对象。最后，我们返回解码的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`，如下所示:*

***PS:** *我们还将一个 userId 作为参数传递给了 URL 路径。指定的参数必须与我们在控制器中指定的参数相匹配。**

***编辑用户端点** 要编辑用户，我们需要修改 user_controller.go 如下所示:*

*上面的`EditAUser`功能与`CreateUser`功能的作用相同。然而，我们包含了一个`update`变量来获取更新的字段，并使用`userCollection.UpdateOne`更新了集合。最后，我们搜索更新后的用户详细信息，并返回解码后的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***删除一个用户端点** 要删除一个用户，我们需要修改 user_controller.go 如下所示:*

*`DeleteAUser`功能遵循前面的步骤，使用`userCollection.DeleteOne`删除匹配的记录。我们还检查一个项目是否被成功删除，并返回适当的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***获取用户列表端点** 要获取用户列表，我们需要修改 user_controller.go，如下所示:*

*`GetAllUsers`功能遵循前面的步骤，使用`userCollection.Find`获取用户列表。我们还使用`Next`属性方法遍历返回的用户列表，以最佳方式读取返回的列表。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***完成 user_controller.go***

***完成 user_route.go***

*完成后，我们可以通过在终端中运行下面的命令启动开发服务器来测试我们的应用程序。*

```
*go run main.go*
```

*![](img/0a4f593957747883fe7bc390ccc73b71.png)**![](img/945db39fe3a3e59430263b8640ae5473.png)**![](img/c28142bb197724d6501793be6aee7e1a.png)**![](img/7eb72f2e8d994a09a38b6ab2fb42a96a.png)**![](img/3240e3f155cc4a43f2e70fc70760936c.png)**![](img/68228f37b057b6b5ea5a6c56866e1da1.png)**![](img/1dcc9ed3bb796701b0137a94124ba54d.png)*

# *结论*

*这篇文章讨论了如何构建 Gin-gonic 应用程序，构建 REST API，以及使用 MongoDB 持久化我们的数据。*

*您可能会发现这些资源很有帮助:*

*   *[大猩猩/多路复用器](https://github.com/gorilla/mux)*
*   *[MongoDB Go 驱动](https://docs.mongodb.com/drivers/go/current/)*
*   *[Go 验证器](https://github.com/go-playground/validator)*
*   *[转到环境加载器](https://github.com/joho/godotenv)*