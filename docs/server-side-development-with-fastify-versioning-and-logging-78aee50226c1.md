# 使用 Fastify 进行服务器端开发—版本控制和日志记录

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-versioning-and-logging-78aee50226c1?source=collection_archive---------3----------------------->

![](img/8bfa9ae643efaddc5cf8b60523362434.png)

照片由 [Goh Rhy Yan](https://unsplash.com/@gohrhyyan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 版本

我们可以用`version`属性设置接受的版本。

例如，我们可以写:

```
const fastify = require('fastify')()fastify.route({
  method: 'GET',
  url: '/',
  version: '1.2.0',
  handler (request, reply) {
    reply.send({ hello: 'world' })
  }
})
const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

然后我们的`/`路由将检查请求中的`Accept-Version`报头，看它是否与我们所拥有的相匹配。

如果我们在`Accept-Version`设置为`1.x`、`1.2.0`或`1.2.x`的情况下发出请求，那么我们会看到:

```
{ hello: 'world' }
```

在回应中。

否则，我们得到 404。

# 记录

默认情况下，日志记录是禁用的。

我们可以通过设置`logger`属性来启用日志记录。

例如，我们可以写:

```
const fastify = require('fastify')({ logger: true })fastify.get('/', (request, reply) => {
  request.log.info('Some info')
  reply.send({ hello: 'world' })
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

在`logger`设置为`true`的情况下启用记录。

然后我们调用`request.log.info`来记录一些东西到控制台。

我们应该会看到类似。

```
{"level":30,"time":1604268389762,"pid":737,"hostname":"9cc07d2eed9d","reqId":2,"msg":"Some info"}
```

记录在案。

我们还可以将一些选项传递给记录器。

例如，我们可以写:

```
const fastify = require('fastify')({ 
  logger: {
    level: 'info',
    file: './log.txt'
  }
})fastify.get('/', (request, reply) => {
  request.log.info('Some info')
  reply.send({ hello: 'world' })
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

然后我们将输出记录到`log.txt`。

我们可以向 Pino 实例传递一个自定义流。

为此，我们写道:

```
const split = require('split2')
const stream = split(JSON.parse)const fastify = require('fastify')({
  logger: {
    level: 'info',
    stream: stream
  }
})
fastify.get('/', (request, reply) => {
  request.log.info('Some info')
  reply.send({ hello: 'world' })
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

若要传入使用创建的自定流:

```
const stream = split(JSON.parse)
```

此外，我们可以定制用`serializers`属性记录的内容。

我们可以写:

```
const fastify = require('fastify')({
  logger: {
    serializers: {
      req (request) {
        return { url: request.url }
      }
    }
  }
})fastify.get('/', (request, reply) => {
  request.log.info('Some info')
  reply.send({ hello: 'world' })
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

添加具有`req`函数的`serializers`属性，以返回一个包含我们希望从请求中记录的数据的对象。

# 结论

我们可以设置 API 的版本，并使用 Fastify 添加日志。