# 构建或克隆—一个联合 API — Pt。1:网关++

> 原文：<https://blog.devgenius.io/build-or-clone-a-federated-api-pt-1-the-gateway-ad75dad21459?source=collection_archive---------6----------------------->

大家好，感谢加入我所说的“构建或克隆”所以你可以和我一起建，或者克隆完成的回购！我们正在创建一个全功能的阿波罗联合 API。这是第 1 部分——Gateway++。

今天，我想分享我在过去几个月里一直在做的一个项目的一部分——一个比一般的教程/入门回购更有吸引力的 Apollo 网关。

除了将一般的 graphql 请求定向到子图之外，网关还有多种职责。无论您是选择自己构建还是克隆最终结果作为起点，这个网关都履行了在创建自己的网关时可能会被忽略的职责。

因此，如果你有兴趣学习一点关于回购、网关的责任或者如何编写网关代码，请继续阅读！

对于那些只想克隆功能完整的最终结果回购的人，请跳到底部了解如何操作。我制作它是为了让人们可以把它作为他们自己的门户的一个开端，或者只是按原样运行！

让我们启动网关。

![](img/341f7847622939e17364d94ec9a69778.png)

# 阿波罗之门——我们正在建造的

网关是联合架构中微服务 API 的入口点。所有对 API 的请求都需要通过网关，这意味着这个小服务有很多责任。

为了从用户的微服务中获取用户，客户机将把请求发送给网关，然后网关将找到到达用户服务的方法来完成请求。

## 好处

与直接查询外部服务相反，使用网关提供了相当多的好处。

1.  单一端点—无论有多少服务，客户端只需知道一个单一端点和服务器位置。
2.  全局上下文—可以在网关级别将全局上下文添加到所有请求中。键入的自定义数据现在可以传递给所有微服务。
3.  全局授权——现在可以在网关级别全局解析授权，而不必在每个微服务上解析授权。

## 责任

这意味着网关也将有相当多的推断出的*职责*。

1.  将请求传递给正确的子图
2.  类型检查传入的 GraphQL 请求
3.  处理授权
4.  将上下文附加到请求
5.  处理文件上传请求(为什么现代应用程序不接受文件上传？)
6.  提供上传的文件/静态资产

希望你明白了！建立一个网关来做我们需要它为现代 API 做的事情是很重要的。

因此，我们将为一个 API 构建一个网关，来处理上述的 6 个*责任*和 3 个*利益*。

# 正在设置

## 创建和初始化

为项目创建网关目录，并用 npm 初始化。

```
mkdir gateway-tutorial
cd gateway-tutorial
npm init -y
```

下面的例子将使用 typescript，但是设置它超出了本文的范围。确保按照您喜欢的方式设置您的 tsconfig。

完成后，让我们创建`src`文件和入口点。

```
mkdir src
touch server.ts
```

# 入口点— Server.ts

## 快速设置

网关将需要能够接受 GraphQL 请求以及对静态资产的请求(RESTful 请求)。

为了处理快速设置，我们需要几个包。

```
npm i express cors
```

Express 将非常适合处理静态资产的服务。首先为一个简单的 express 应用程序创建样板文件。

```
// server.tsimport { express } from "express";
import cors from "cors";const app = express();
app.use(cors());
app.use(express.json());const port = 5000;app.listen(port, () => console.log(`GATEWAY ====> UP ON PORT ${port}`));
```

## 创建网关

我们将能够用`@apollo/gateway`包创建网关。

这个`@the-devoyage/micro-auth-helpers`包提供了助手函数来生成上下文、传递消息头、解析授权状态等等。从 GitHub 注册表安装这个包，如下所示，或者查看[微认证助手文档](https://github.com/The-Devoyage/micro-auth-helpers/packages/1244493)。

*务必将范围添加到您的* `*.npmrc*` *中，并登录 GitHub 注册表。*

```
## Login to GitHub registry
npm login --registry=https://npm.pkg.github.com## Tell NPM where to find `@the-devoyage` packages. 
echo [@the](http://twitter.com/the)-devoyage:registry=[https://npm.pkg.github.com](https://npm.pkg.github.com) >> .npmrc## Install The Packages
npm i @apollo/gateway @the-devoyage/micro-auth-helpers
```

然后创建网关

```
// server.tsimport { ApolloGateway } from "@apollo/gateway";
import { Helpers } from "@the-devoyage/micro-auth-helpers";
import { readFileSync } from "fs";
// ...imports// Express Setup...const supergraphSdl = readFileSync("./supergraph.graphql").toString();const gateway = new ApolloGateway({
  supergraphSdl: supergraphSdl ?? "",
  buildService({ url }) {
    const dataSource = new Helpers.Gateway.ContextDataSource({ url });
    return dataSource;
  },
});
```

`supergraphSdl`是整个 API 中所有类型的模式。从根本上说，它定义了在您的网关中可以或不可以发生什么。这将在接下来的步骤中使用 Rover CLI 生成，所以现在不要担心实际的文件——只需确保像上面一样阅读它。

在新的`ApolloGateway`实例中，`buildService`函数允许我们将数据添加到请求的 outgoing to 子图中。这意味着我们可以生成全局`context`——源自网关但被传递给所有连接的子图的数据。

您可以从包`@apollo/gateway`中扩展`RemoteGraphQLDataSource`，以创建您自己的全局上下文——或者—`@the-devoyage/micro-auth-helpers`包通过助手`ContextDataSource`帮助我们完成这项工作，如上面的代码示例所示。

除了帮助我们创建全局上下文，`ContextDataSource`助手还使用`graphql-upload`包通过 GraphQL 请求将文件上传功能扩展到外部 API。这个我们下面就讲！

## 创建并启动 Apollo 服务器

是时候创建 Apollo 服务器了，我们需要另外几个包来完成这个任务。

```
npm i apollo-server-express graphql-upload dotenv
```

并创建服务器:

```
// server.tsimport { Helpers } from "@the-devoyage/micro-auth-helpers";
import { ApolloServer } from "apollo-server-express";
import { graphqlUploadExpress } from "graphql-upload";
import dotenv from "dotenv";
// ...imports// ...Express Setup// ...Gateway Setupdotenv.config(); // Get Environment Variables// Create and Start Apollo Serverlet apolloServer;async function startServer() {
  apolloServer = new ApolloServer({
    gateway,
    context: ({ req }) =>
      Helpers.Gateway.GenerateContext({
        headers: ["Authorization"],
        req,
        secretOrPublicKey: process.env.JWT_ENCRYPTION_KEY,
      }),
  });
  await apolloServer.start();
  app.use(graphqlUploadExpress());
  apolloServer.applyMiddleware({ app });
}startServer();// Start Express Serverapp.listen(port, () => console.log(`GATEWAY ====> UP ON PORT ${port}`));
```

异步功能允许我们等待`apolloServer`的开始，应用`graphql-upload`中间件，最后将快速应用应用到`apolloServer`。

`GenerateContext`助手将允许我们为服务器创建一个上下文，更重要的是，auth context。

用这个助手生成上下文的第一种方法是挑选传入请求的头，用作网关服务器中的上下文。这非常有用，因为我们将需要检查 JWT 的`Authorization`头，以向服务器授权。要选择一个头作为上下文，将它添加到`headers`数组中，就像我对上面的`Authorization`所做的那样。

助手的巧妙之处在于它做的比它引导的多一点。在我们的例子中，因为我们使用了一个 JWT 和一个`Authorization`头，它还将解码 JWT 有效载荷并将结果添加到上下文中。我们只需提供`JWT_ENCRYPTION_KEY`，作为函数的参数。

因此，简单地通过使用`GenerateContext`和`ContextDataSource`，我们能够为我们的 API 创建一个全局`auth`上下文。

```
// Global Contextexport interface Context extends Record<string, any> {
  auth: AuthContext;
  // ...other custom context!
}export interface AuthContext {
  payload: Payload;
  isAuth: boolean;
  error?: string;
}
```

检查包本身:

[](https://github.com/The-Devoyage/micro-auth-helpers/packages/1244493) [## 将微身份验证助手打包到-devo yage/微身份验证助手

### 这是一组函数，可以简化在联邦环境中处理身份验证和授权的过程…

github.com](https://github.com/The-Devoyage/micro-auth-helpers/packages/1244493) 

这是我写的一篇文章，概述了这个包的特性:

[](/apollo-federation-how-do-request-travel-through-a-federated-architecture-e4a4da54f46d) [## Apollo 联邦:请求如何通过联邦架构？

### 通过联合 Apollo 架构的网络请求之旅非常有趣——了解流程，同时…

blog.devgenius.io](/apollo-federation-how-do-request-travel-through-a-federated-architecture-e4a4da54f46d) 

## 处理静态文件请求

接下来，我们需要确保 restful 请求仍然可以通过网关服务器，即使客户端请求的是图像之类的文件。

网关将需要接收请求，并将其代理到将处理该请求的适当服务器。

首先，安装代理软件:

```
npm i http-proxy-middleware 
```

创建处理代理的路由:

```
// server.ts
import { createProxyMiddleware } from "http-proxy-middleware";
// ...imports// Express Setup...app.use(
    process.env.MEDIA_SERVING_ROUTE ?? "/public",
    createProxyMiddleware({
      target: process.env.MEDIA_SERVER_URL,
      changeOrigin: false,
    })
  );
```

我们可以使用环境变量，而不是硬编码路由和服务器位置。这将允许您在生产和开发环境之间切换路由或服务器 URL。

现在，发往环境变量中指定的端点的所有网关请求都将被重定向到指定的服务器，在那里我们可以处理图像的查找和提供。

# 启动服务器

用 nodemon 启动服务器，或者如果愿意的话，通过创建一个 npm 脚本来启动服务器，或者尝试一下传统的方法——如果使用 typescript，不要忘记首先编译它。

```
node server.js
```

我们应该看到服务器活跃起来，网关现在可以开始接收请求了！

```
GATEWAY ====> UP ON PORT 5000
```

网关已经启动，但还没那么有用，因为没有超级图来启用类型检查或路由。

从教程的这一部分开始，您将需要一些联邦子图来继续运行。如果你手头缺少子图，那就抓紧了——这是 5 部分系列的第 1 部分！下周我们将创建一个账户子图。如果你感兴趣，一定要跟着去。

# 安装漫游者并生成一个超级图

首先，从 [Apollo Docs](https://www.apollographql.com/docs/rover/getting-started) 中选择您的安装方法来安装 Rover CLI。下面我用 curl 来做:

```
curl -sSL https://rover.apollo.dev/nix/latest | sh
```

现在创建一个配置文件，保存到项目的根目录。这个文件将告诉网关关于每个服务的信息。我把我的叫做`supergraph-config.yaml`。

```
subgraphs:
  accounts:
    routing_url: [http://localhost:5001](http://localhost:5001) 
    schema:
      subgraph_url: [http://localhost:5001](http://localhost:5001) 
  users:
    routing_url: [http://localhost:5002](http://localhost:5002)
    schema:
      subgraph_url: [http://localhost:5002](http://localhost:5002)
  meida:
    routing_url: [http://localhost:5006/graphql](http://localhost:5006/graphql)
    schema:
      subgraph_url: [http://localhost:5006/graphql](http://localhost:5006/graphql)
  mailer:
    routing_url: [http://mailer:5008/graphql](http://mailer:5008/graphql)
    schema:
      subgraph_url: [http://localhost:5008/graphql](http://localhost:5008/graphql)
```

当所有子图都在运行时，我们现在可以使用 Rover CLI 来生成一个超级图。

```
rover supergraph compose --config ./supergraph-config.yaml > supergraph.graphql
```

如果所有的类型检查都成功，它将输出保存到我们上面定义的文件中，`supergraph.graphql.`

## 重启网关并庆祝

就这样，我们就要完成了。每当子图类型改变时，都需要重新生成超图，并且每当超图重新生成时，都需要重新启动网关。

一旦网关重启，您应该能够查询子图了！

# 网关—存储库

嘿！您刚刚构建了一个健壮的网关来处理各种请求场景…或者等等，您是那个跳到底层的人吗！不管怎样，看看这个网关！

*   类型检查所有子图
*   使用 Rover CLI/supergraph 将 graphql 请求路由到正确的子图
*   从各种来源生成网关上下文，如请求头、jwt 或自定义数据
*   全局上下文—上下文被发送到所有外部微服务
*   为每个请求自动创建一个类型化的授权上下文
*   代理往来于外部微服务器的 restful 请求，从文件存储微服务请求静态资产。
*   它将 graphql 请求中的文件传递给子图——用于`graphql-upload`

## 克隆存储库

我把这个库做成了双重用途——把它作为一个入门模板来定制你自己的网关，或者启动它并把它作为你下一个项目的现成网关，它将满足大多数需求。

[购买权限](https://basetools.io/checkout/XGUVNNGr) —我使用 Basetools 为我的 git 回购协议添加了一个付费墙！一旦您的交易通过，您将收到一封电子邮件，要求您作为合作者加入回购。这意味着你可以随心所欲地克隆它，随心所欲地使用它。您还可以访问所有未来版本！

不想购买？没关系——成为关注者并给我发消息，我也会将你添加为合作者以获得这种类型的支持！

感谢您阅读这篇关于创建 Apollo 网关的教程。我希望它能帮助你开始你的下一个项目！

请继续关注，因为 4 graphql federated micro services 还有 4 个教程/回购即将推出！我们将建立一个帐户微服务，一个用户微服务，一个文件上传微服务，最后是一个自动化的电子邮件微服务！