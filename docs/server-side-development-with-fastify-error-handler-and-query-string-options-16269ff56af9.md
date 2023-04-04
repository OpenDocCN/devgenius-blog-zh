# 使用 Fastify 进行服务器端开发—错误处理程序和查询字符串选项

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-error-handler-and-query-string-options-16269ff56af9?source=collection_archive---------4----------------------->

![](img/e5b8721a1753983964558e42b4b650f1.png)

安德斯·吉尔登在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 错误处理程序和查询字符串选项

Fastify 有很多我们可以配置的服务器选项。

`trustProxy`选项让 Fastify 知道它在代理后面。

我们可以将其设置为代理服务器的 IP 地址或布尔值。

此外，我们可以创建一个函数来设置代理 IP 的信任回报。

我们可以访问`[request](https://www.fastify.io/docs/latest/Request)`对象上的`ip`、`ips`、`hostname`和`protocol`值:

```
let i = 0
const fastify = require('fastify')({
  genReqId (req) { return i++ }
})fastify.get('/', (request, reply) => {  
  console.log(request.ip)
  console.log(request.ips)
  console.log(request.hostname)
  console.log(request.protocol)
  reply.send('hello world')
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()
```

`ip`拥有 IP 地址。`hostname`有主机名，`protocol`有协议。

`querystringParser`选项让我们设置想要使用的查询字符串解析器。

例如，我们可以写:

```
const qs = require('qs')
const fastify = require('fastify')({
  querystringParser: str => qs.parse(str)
})
fastify.get('/', (request, reply) => {  
  reply.send('hello world')
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()
```

使用`qs.parse`方法解析对象的查询字符串。

`versioning`选项让我们用永远版本化来版本化我们的路线。

我们可以通过编写以下内容来设置此选项:

```
const versioning = {
  storage () {
    let versions = {}
    return {
      get: (version) => { return versions[version] || null },
      set: (version, store) => { versions[version] = store },
      del: (version) => { delete versions[version] },
      empty: () => { versions = {} }
    }
  },
  deriveVersion: (req, ctx) => {
    return req.headers['accept']
  }
}const fastify = require('fastify')({
  versioning
})fastify.get('/', (request, reply) => {  
  reply.send('hello world')
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()
```

我们添加了`get`、`set`和`del`方法来添加操作`versions`对象来处理版本。

`frameworkErrors`让我们为应用程序遇到的错误提供错误处理程序。

例如，我们可以写:

```
const fastify = require('fastify')({
  frameworkErrors (error, req, res) {
    if (error instanceof FST_ERR_BAD_URL) {
      res.code(400)
      return res.send("Provided url is not valid")
    } else {
      res.send(err)
    }
  }
})fastify.get('/', (request, reply) => {  
  reply.send('hello world')
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()
```

我们为错误的 URL 错误添加了自己的处理程序。

如果遇到这种情况，我们会返回 400 错误。

`clientErrorHandler`让我们监听客户端连接发出的错误事件，并返回 400 错误。

例如，我们可以写:

```
const fastify = require('fastify')({
  clientErrorHandler (err, socket) {
    const body = JSON.stringify({
      error: {
        message: 'Client error',
        code: '400'
      }
    })this.log.trace({ err }, 'client error')
    socket.end(`HTTP/1.1 400 Bad Request\r\nContent-Length: ${body.length}\r\nContent-Type: application/json\r\n\r\n${body}`)
  }
})fastify.get('/', (request, reply) => {  
  reply.send('hello world')
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()
```

添加我们自己的`clientErrorHandler`来添加客户端错误。

# 结论

我们可以向我们的 Fasttify 应用程序添加各种处理程序。