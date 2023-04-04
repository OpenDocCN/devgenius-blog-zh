# 通过 React、Redux 和“无服务器”使用 API 网关 Web Socket API:第 1 部分

> 原文：<https://blog.devgenius.io/using-api-gateway-web-socket-api-with-react-redux-and-serverless-part-01-2a9ec01c8a40?source=collection_archive---------6----------------------->

![](img/9b2a11e1a3ddfb232cbdd1ccf4225266.png)

API gateway 无疑是 AWS 提供的使用最多的服务之一。但是，Web socket APIs 与通常的 REST APIss 有很大的不同，REST API 接收并响应客户端发送的请求。相反，Web Socket API 将创建一个由客户端建立的双向连接**。**

> **由客户端建立**这里指的是双向连接的开始。因为客户端必须通过 Web 套接字服务 URL 触发连接。

这里有很多例子展示了如何使用 Web 套接字创建一个聊天应用程序。所以，让我们来看看有些不同的东西。我们将使用 web 套接字创建一个 Web 推送通知功能。

## 需要网页推送通知吗？

简而言之，它可以用于应用程序中的任何东西。一个例子是当后台任务完成时发出通知。在[的上一篇文章](https://medium.com/@iwiick/create-a-background-job-service-with-dynamodb-streams-and-lambda-functions-with-serverless-60b7bc49fd48)中，我解释了如何用无服务器、DynamoDB 流和 Lambdas 创建你自己的后台“作业服务”。在这个场景中，我们可以使用推送通知来通知作业何时完成。Ant design [Notification](https://ant.design/components/notification/) 是一个可定制的组件，我们可以轻松地将其用于 react 应用程序。

## 路线

正如我前面提到的，Web Socket APIs 的工作方式不同于 REST APIs。“路线”是这里的主要区别之一。 **$connect** 客户端与后端建立稳定连接后，进入路由。 **$disconnect** 客户端断开连接时访问路由。建立连接后，使用任何未指定的路由时，将访问 **$default** 路由。

为了澄清一些事情，这篇文章将分为两部分，第一部分将解释如何设置无服务器后端。下一部分将集中于用 **redux** 和 react **钩子**:)灵活地设置前端代码

# 为 API 网关 Web 套接字设置无服务器后端。

[无服务器文档](https://www.serverless.com/framework/docs/providers/aws/events/websocket)提供了后端所需的设置代码。不过，我们将使用一个 lambda 函数(在下面的代码片段中是 createSocket lambda ),它与所有三个$connect、$disconnect 和$default 路由集成在一起:)

我们还需要如下设置 IAM 权限。

现在，让我们看看“createSocket”处理程序。:)

在这个代码片段中有一些事情需要注意。

> 这里我们不使用 return 语句。相反，我们使用回调

这是因为我们将为来自同一个 lambda 的所有路由保持套接字连接，直到断开路由被访问。如果我们在代码末尾返回，我们将会遇到一个严重的错误。所以，**改用回调:)**

> **switch 语句处理路由。**

这是因为我们使用与所有 3 条路线集成的 lambda Lambda。我们从 switch 访问 **routeKey** ，并基于此执行逻辑。如果需要的话，你也可以为每一条路由设置单独的 Lambda 处理程序。

> **不要从 connect 路由向客户端发送消息**

如果你这样做，它会给你一个错误。这是因为此时尚未建立连接。这就是为什么我们与客户沟通，如果 **$default** 路线。

> **“连接 Id”、“域名”和“阶段”取自 requestContext。**

我们需要“connectionId”将消息发送回客户端。“域名”和“阶段”也需要用于此目的，因为 URL 必须以特定的方式格式化。当然，我们可以使用 HTTP Url，而不用从“域名”和“stage”创建 Url，但是这样就忽略了使用无服务器方法的主要目的。让我们看一下消息传递给套接字客户端的代码片段，以及所有这些是如何连接的:)

在使用 **API 网关管理 API** 时，我们需要遵循一些特定的代码语法。如您所见，这个函数有 3 个参数。

“ **url** ”是我们需要从“域名”和“stage”中获取格式化后的 url 的地方。" **ConnectionId** "是通过该套接字连接的客户端的唯一 Id。“有效负载”是我们将发送回客户端的数据对象。它可以是任何东西，只要我们把它串起来(T21)

现在，所有这些都已经过去了，让我们来看看完整的 Lambda 函数。

我们在第 22 行格式化 URL。我还将传递回 **connectionId** 和 **CallbackUrl** 作为有效负载。部署这个服务，您将获得一个 Web Socket 服务 URL。我们需要这个来建立与客户端的连接。

这基本上就是设置后端的工作。使用 connectionId 和 callbackUrl 的内容和方式完全由您决定。只要您能够访问它们，就可以使用它们从应用程序的后端向客户端发送消息。在我的例子中，当使用我的自定义“[作业服务](https://medium.com/@iwiick/create-a-background-job-service-with-dynamodb-streams-and-lambda-functions-with-serverless-60b7bc49fd48)”实现完成一个后台作业时，我用它来创建一个推送通知特性:)

文章的第二部分是[这里](https://medium.com/@iwiick/using-api-gateway-web-socket-api-with-react-redux-and-serverless-part-02-da407a47d070):)

*经* ***鹤山***