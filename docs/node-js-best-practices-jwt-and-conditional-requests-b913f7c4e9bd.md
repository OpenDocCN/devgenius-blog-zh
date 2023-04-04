# Node.js 最佳实践— JWT 和条件请求

> 原文：<https://blog.devgenius.io/node-js-best-practices-jwt-and-conditional-requests-b913f7c4e9bd?source=collection_archive---------6----------------------->

![](img/345536b2314ee7d1bd6bc7c96018b1da.png)

尼克·卡沃尼斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用基于 JWT 的无状态身份验证

我们可以使用 JSON web 令牌进行身份验证。

JSON web 令牌由 3 部分组成。

它们包括:

*   带有令牌类型和哈希算法的标头
*   有效载荷有索赔权
*   签署有效负载的签名。

我们可以很容易地添加一些附件。

例如，我们可以使用 koa-jwt 包来添加令牌。

我们可以写:

```
const koa = require('koa')
const jwt = require('koa-jwt')const app = koa()app.use(jwt({ 
  secret: 'secret' 
}))// Protected middleware
app.use(function *(){
  this.body = {
    foo: 'bar'
  }
})
```

我们只是调用`app.use`来使用`jwt`中间件。

该对象具有签名令牌的秘密。

然后我们添加了一个受保护的中间件。

令牌内容将在`this.state.user`中提供。

JWT 模块不依赖于任何数据库层。

都是自己验证的。

它们还可以包含生存时间值。

为了确保我们的通信是安全的，我们仍然必须确保 API 端点通过 HTTPS 连接可用。

# 使用条件请求

条件请求是 HTTP 请求，根据请求头值的不同，其结果也不同。

标头检查存储在服务器上的资源版本是否与同一资源的给定版本相匹配。

报头可以是自上次修改以来的时间戳。

它也可以是一个实体标签，每个版本都不同。

标题是:

*   `Last-Modified`指示资源上次修改的时间
*   `Etag`哪个是实体标签
*   `If-Modified-Since`与`Last-Modifed`割台一起使用
*   `If-None-Match`与`Etag`割台一起使用。

# 使用速率限制

我们可以限制对 API 的请求数量。

为了告诉我们的 API 客户端还有多少请求，我们可以设置以下响应头:

*   `X-Rate-Limit-Limit`，告诉我们在给定的时间间隔内允许的请求数量
*   `X-Rate-Limit-Remaining`，同一时间间隔内剩余的请求数
*   `X-Rate-Limit-Reset`，告诉我们速率限制将被重置的时间

我们可以添加库来为我们的应用程序添加速率限制功能。

有了 Koa，我们可以使用 [koa-ratelimit](https://github.com/koajs/ratelimit) 包。

# 创建适当的 API 文档

没有任何文档，很难知道如何使用我们的 API。

为了让我们的生活更容易，我们可以使用 [API Blueprint](https://apiblueprint.org/) 或 [Swagger](http://swagger.io/) 来创建我们的文档。

# API 的未来

除了休息还有其他选择。

我们可以使用 GraphQL 来监听 HTTP 请求，但是它有类型检查，我们可以选择性地选择资源。

有了类型检查和有选择地查询资源的能力，我们可以更有效率并减少出错的机会。

![](img/50745fb99b34437bdaf3618a11845a26.png)

丹尼·米勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用 JWT 进行身份验证。

条件请求让我们根据头部发出不同的请求。

还应该考虑 REST APIs 的替代方案。