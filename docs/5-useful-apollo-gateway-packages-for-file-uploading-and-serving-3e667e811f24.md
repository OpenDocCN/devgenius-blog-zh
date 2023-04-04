# 用于文件上传和服务的 5 个有用的 Apollo 网关包

> 原文：<https://blog.devgenius.io/5-useful-apollo-gateway-packages-for-file-uploading-and-serving-3e667e811f24?source=collection_archive---------17----------------------->

大家好！

感谢您的参与——今天，我很乐意分享一份简短的软件包、工具和存储库列表，它们可以让您的 Apollo Federated API *在文件上传和从外部微服务提供服务方面非常好用。*

![](img/4c0b1ae833e78276187cfda47c287ec9.png)

等等，我不会随便列出包的。让我们找点乐子，用用例来组织这个列表。

## **通过 GraphQL 网关从联合微服务安全地提供静态文件**

包/工具:

*   `express`
*   `apollo-server-express`
*   `http-proxy-middleware`

我喜欢 GraphQL，但是像许多其他开发人员一样，我的第一个后端框架是`express`。有一些用例场景可以更好地匹配 express 的功能——包括静态文件服务。

```
// Federated Microserviceconst app = express()app.use("/public", express.static("./public"))
```

通过联邦，您可以将逻辑分离到微服务中，比如媒体/文件上传服务。媒体服务可以负责保存文件、提供文件以及将关于文件的信息保存到数据库中以供将来参考。

问题是媒体服务不应该负责用户认证，那是网关的工作。也就是说，您不应该直接向公众公开外部服务。

那么，如何从外部服务安全地提供文件呢？当然是通过入口！

一种选择是通过网关代理去往/来自微服务的请求和响应。这样，任何请求都不会直接发送到外部微服务，而是通过网关。

```
// Gateway
import { createProxyMiddleware } from "http-proxy-middleware";
import {isAuthMiddleware} from './middleware';
import express from 'express';const app = express();app.use(
    "/public",
    isAuthMiddleware,
    createProxyMiddleware({
      target: http://media_server:5000,
      changeOrigin: false,
    })
  );
```

现在，客户端将能够使用`/public`路由通过网关从外部微服务请求文件。添加自定义身份验证和授权中间件，您的 GraphQL 网关现在可以安全地提供来自外部微服务的静态文件。

## **通过网关上传文件**

包和工具:

*   `graphql-upload`
*   `@profusion/apollo-federation-upload`

文件上传与文件服务有点不同。如上所述，你永远不应该将上传微服务的文件直接暴露给公众——因此，一个选择是将文件发送到网关，并让它处理路由。这一次，我们将在 GraphQL 请求中发送文件，而不是使用 express。

听起来比实际更复杂！

`@profusion/apollo-federation-upload`包提供了一个`RemoteDataSource`类，称为`FileUploadDataSource`。这个类的重要任务是将传入的文件传递给外部文件上传微服务。

当然，除了文件，我们还希望能够发送请求的授权状态。这可以通过扩展`FileUploadDataSource`来包含一个自定义的`AuthenticationDataSource`来实现。

基本上，在我们将文件发送到远程服务器之前，我们会将授权状态添加到请求中。

```
export class AuthenticatedDataSource extends FileUploadDataSource {
  willSendRequest({
    request,
    context,
  }: {
    request: GraphQLRequest;
    context: Context;
  }) {
    request.http?.headers.set("token", JSON.stringify(context.token));
    request.http?.headers.set("isauth", JSON.stringify(context.isAuth));
  }
}
```

现在，在创建网关时使用新的`AuthenticatedDataSource`类。

```
const gateway = new ApolloGateway({
  supergraphSdl,
  buildService({ url }) {
    const dataSource = new AuthenticatedDataSource({ url });
    return dataSource;
  },
});
```

用`graphql-upload`包作为中间件启动服务器。

```
async function startServer() {
  apolloServer = new ApolloServer({
    gateway,
    context: ({ req }) => {
      return isAuth(req);
    },
  });
  await apolloServer.start();
  app.use(graphqlUploadExpress());
  apolloServer.applyMiddleware({ app });
}
```

注意上面，我们没有在网关级别拒绝访问，这是非常可能的，也是完全有效的。相反，我们只是简单地传递授权状态。状态由`isAuth`函数确定，并添加到上下文中，如上所示。

这样，可以在解析器级别批准或拒绝请求。如果外部服务中的一些解析器需要授权，而其他解析器不需要，这就变得很有用。

这很神奇——文件现在与请求头结合在一起，可以从 Apollo 网关找到正确的外部联邦服务和解析器。

# 启动您的网关

网关的工作相当重要——用户授权和路由。大多数时候，我们只需要打开网关，让它完成自己的工作。

这就是为什么我创建了一个存储库，您可以使用它来启动您的网关，提供用户授权、文件服务和上传功能，就像我们上面讨论的那些功能一样。

回购是一个私人仓库，但你只需支付 50 美元就可以立即进入。我使用 [Basetools](https://basetools.io) 为私有回购协议添加付费墙。这是一个非常酷的服务，可以让你轻松地出售你的代码。这是我可以证明遛狗和约会夜之间长时间编码是合理的一种方式！

感谢您的支持！

*   [购买访问我的](https://basetools.io/checkout/XGUVNNGr) `[@the-devoyage/graphql-gateway](https://basetools.io/checkout/XGUVNNGr)` [存储库](https://basetools.io/checkout/XGUVNNGr)
*   阅读更多关于`@the-devoayge/graphql-gateway`的信息——即将推出

嘿，我叫尼克！感谢您花时间阅读上面的文章！如果你正在跟随，享受内容，或者认为它可能帮助某人，你可以做三件事来帮助我——鼓掌——跟随——回应！

正如你可能从阅读我以前文章的最后想法中知道的——我要做前交叉韧带手术。本来应该已经发生了，但是德克萨斯州反常的冰风暴导致了一点延迟。现在疼痛开始于本周五，11 号…

虽然我可能无法享受正常的徒步旅行和遛狗，但我仍然会呆在电脑前——创建微服务和编写代码！

如果你有一个 API 微服务需要创建，请在下面回复！我将寻找新的项目想法，而我正在沙发上冲浪这个手术的恢复！我很乐意为你和其他人创造一个新的服务！

再次感谢，

缺口