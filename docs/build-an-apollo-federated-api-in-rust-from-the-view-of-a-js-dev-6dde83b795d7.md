# 从 JS 开发人员的角度，在 Rust 中构建一个 Apollo 联邦 API

> 原文：<https://blog.devgenius.io/build-an-apollo-federated-api-in-rust-from-the-view-of-a-js-dev-6dde83b795d7?source=collection_archive---------1----------------------->

大家好！感谢您的加入。

今天，我想分享一个用 Rust 构建的联合 API 的快速示例。

在过去的一个月里，我一直在利用我的空闲时间慢慢学习一些关于 Rust 的知识。为此，我想尝试创建一个简单的小联邦 API 来帮助巩固我到目前为止学到的技能。

从一个熟悉 JS 的人的角度来看，下面是如何构建一个支持 Apollo 联邦的 Rust GraphQL API。我还会附上一些阅读推荐、回复和提示，以避免陷入和我一样的困境。

![](img/b2fb83a8c924e4a8c12665f0b5280d6e.png)

# 我们将建造什么

这个 API 将提供两个服务，包括一个`users`服务和一个`dogs`服务。为了演示联邦规范，`dogs`服务将从`users`服务扩展`user`类型。

# 项目设置

## 初始化项目子图

使用 Cargo，一个类似于`npm`的管理器，让你的 rust 项目开始。我们将为 API 创建一个新的目录，以及两个子图/微服务来管理这个 API 的数据。

```
mkdir dogs-api
cd dogs-api
cargo new users
cargo new dogs
```

## 安装子图依赖项

就像 Node 一样，我们将使用依赖关系来帮助我们创建每个子图。与 node 不同，我们不从 cli 安装依赖项——相反，我们编辑每个子图服务的`Cargo.toml`文件。

向每个子图服务添加以下五个依赖项:

```
[dependencies]
tokio = { version = "1.0.2", features = ["macros", "rt-multi-thread"] }
warp = "0.3"
http = "0.2"
async-graphql = "3.0.34"
async-graphql-warp = "3.0.34"
```

*   `tokio`是 rust 的异步运行时，允许我们使用异步等待语法
*   `warp`是我们将开始允许传入和传出请求的服务器。
*   `http`提供了可以在我们的代码库中使用的类型，以确保一切安全。
*   `async-graphql`和`async-graphql-warp`将允许我们执行 graphql 查询和变异，以及启用 API 的联邦方面。

## 创建异步主函数

调用 main 函数来启动项目。开箱即用，这个函数内部没有任何东西是异步的。如您所知，处理服务器需要一些异步/等待功能。为了启用这个特性，我们需要引入`tokio`运行时。

在`src/main.rs`中，添加以下宏以启用 main 函数中的 async/await 语法。

```
#[tokio::main]
async fn main() {
  println!("hello world");
}
```

## 声明模块

为了方便使用，如果你正在编写你自己的服务器，这里是我们需要引入的导入/模块。在本文的其余部分，您可以随意复制粘贴或简单地用作参考。

```
use async_graphql::{
    http::{playground_source, GraphQLPlaygroundConfig},
    EmptyMutation, EmptySubscription, Object, Schema, SimpleObject,
};
use async_graphql_warp::{GraphQLBadRequest, GraphQLResponse};
use http::StatusCode;
use std::convert::Infallible;
use warp::{http::Response as HttpResponse, Filter, Rejection};
```

# 创建服务器

就正在发生的事情而言，我们今天创建的服务器与典型的`apollo-server-express`设置没有太大的不同。不同之处在于语法。让我们比较一下`apollo-server-express`服务器和 rust 服务器的设置过程。

## 典型的 Apollo Server Express 设置

在 Rust 中创建 GraphQL 服务器类似于在 Node 中使用`apollo-server-express`创建 GraphQL 服务器。

使用`apollo-server-express`，您需要:

1.  创建快递服务器— `app`
2.  创建 Apollo 服务器来处理 GraphQL 请求
3.  将 express `app`作为中间件应用到`apollo-server`
4.  启动 express 服务器。

```
// Node Exampleconst app = express();const startServer = async () => {
  const apolloServer = new ApolloServer({
    schema,
  });
  await apolloServer.start();
  apolloServer.applyMiddleware({ app });
};startServer();app.listen(port, () => console.log(`Service ready at port ${port}!`));
```

用最基本的术语来说，`apollo-server`处理 graphql 方面，express 服务器处理传入和传出的请求。

## 在 Rust 中使用 Warp 和 Async GraphQL

对于铁锈，有一个非常相似的过程。

1.  定义`graphql_post`变量来处理 GraphQL Post 请求(就像启动`apollo-server`)。
2.  将该函数作为路径应用于 warp 服务器
3.  创建并启动 warp 服务器

确保自定义服务器启动的端口。例如，上面的服务器在端口 5011 上启动。

# 该模式

正如您可能已经注意到的，schema 变量应用于服务器，就像在`apollo-server`示例中一样。

首先让我们创建模式，然后我们将定义`typeDefs`和`resolvers`。在 main 函数的顶部定义 schema 变量，这样它就可以应用到我们上面写的代码中。

```
#[tokio::main]
async fn main() {
  let schema = Schema::build(Query, EmptyMutation, EmptySubscription).finish(); // Start The Server....
}
```

## TypeDefs 和解析器

在`main`功能之上(甚至在另一个模块中！)，我们将为每个服务定义`typeDefs`和`resolvers`的 rust 当量。

`async-graphql`箱提供了宏，我们可以用它来定义类型定义、实体、扩展等等。先分解一下，然后我一起给你看。

## 定义查询

在联邦 api 中，我们需要扩展已定义的查询。

```
// Node
extend type Query {
  getDog: () => {
    return {
      color: "Tri Color",
      name: "Oakley",
      age: 3,
      id: 1,
      owner: { id: 1, __typename: "User" }
    }
  }
}// Rust
struct Query#[Object(extends)]
impl Query {
  async fn get_dog(&self) -> Dog {
    Dog {
      color: "Tri Color".to_string(),
      name: "Oakley".to_string(),
      age: 3,
      id: 1,
      owner: User { id: 1 },
    }
  }
}
```

## 创建类型

```
// Node
type User {
  name: String!
  id: Int!
}// Rust
#[derive(SimpleObject)]
struct User {
    name: String,
    id: i32,
}
```

# 启用联盟

就像在 Node 中一样，我们需要扩展类型，以便在整个联邦图中使用它们。

## **扩展类型和@外部指令**

`#[Object(extends)]`宏可用于扩展 Rust 中的一个类型。`@key`指令现在被定义为一个实现的功能，它也利用了`#[graphql(external)]`宏。

```
// The Dogs Subgraph Extends a User// Node
extend type User @key(fields: "id") {
  id: Int! @external
  dogs: [Dog!]!
}// Rust
#[Object(extends)]
impl User {
    #[graphql(external)]
    async fn id(&self) -> &i32 {
        &self.id
    } async fn dogs(&self) -> Vec<Dog> {
        // Find and Return Vector of Dogs
    }
} 
```

## 解析实体

为了在联合架构中使用`async-graphql`服务，我们需要创建一个函数来解析`entity.`

```
#[Object(extends)]
impl Query {
    #[graphql(entity)]
    async fn resolve_user(&self, id: i32) -> User {
        User { id }
    }
}
```

# **最终结果**

## 用户子图

**狗子图**

# 下一步和一些阅读

## 启动服务器

我们可以简单地通过运行`cargo run`来启动服务器。确保在不同的端口启动每台服务器。

## 启动网关

本文并不打算介绍如何创建一个 apollo 联邦网关——但是它非常简单！查看我关于如何创建网关的文章——也有一个完整的网关回购，如果你愿意，你可以克隆并使用它。

[](/build-or-clone-a-federated-api-pt-1-the-gateway-ad75dad21459) [## 构建或克隆—一个联合 API — Pt。1:网关++

### 构建它或克隆 Repo——一个具有全局授权上下文和静态文件服务的 Apollo 网关

blog.devgenius.io](/build-or-clone-a-federated-api-pt-1-the-gateway-ad75dad21459) 

## 查看更多示例

我上面展示的很多内容都来自于通读源代码示例、文档和 github 问题！以下是一些对我有帮助的资料。

1.  帮助我运行这个的最好的联邦例子实际上来自于由`async-graphql` — [GraphGate](https://github.com/async-graphql/graphgate) 的维护者创建的一个 repo
2.  另一个精彩演讲和联邦回购示例—[https://www.youtube.com/watch?v=hMIL12Mj7Pw](https://www.youtube.com/watch?v=hMIL12Mj7Pw)
3.  当然，检查出板条箱文件和`async-graphql`书

嘿——谢谢你今天能来！对我来说，这是一个非常有趣的周末项目，可以学到更多关于 rust 的知识。我想我在让联邦方面工作时比 rust 语法更容易出错，这感觉很好。不管怎样，希望你会觉得有趣，如果你有任何 rust graphql 的提示和技巧想分享，请告诉我。