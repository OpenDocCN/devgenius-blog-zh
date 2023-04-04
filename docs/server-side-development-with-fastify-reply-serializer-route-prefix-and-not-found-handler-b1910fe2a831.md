# 使用 Fastify 的服务器端开发—回复序列化程序、路由前缀和未找到处理程序

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-reply-serializer-route-prefix-and-not-found-handler-b1910fe2a831?source=collection_archive---------3----------------------->

![](img/eedac9df83dd387acfe705751729d156.png)

由[卢卡·坎皮奥尼](https://unsplash.com/@nexgenfx?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 关闭服务器

我们可以调用`fastify.close`来关闭服务器。

例如，我们可以写:

```
const fastify = require('fastify')()fastify.get('/', (request, reply) => {  
  reply.send('hello world')
})fastify.ready(err => {
  if (err) throw err
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)
    await fastify.close()
    process.exit(1)
  }
}
start()
```

无争论地调用`fastify.close`，它将返回一个承诺。

# 路由前缀

我们可以通过`prefix`选项为我们的旅程添加一个路线前缀。

例如，我们可以写:

```
const fastify = require('fastify')()fastify.register(function (instance, opts, done) {
  instance.get('/foo', function (request, reply) {
    request.log.info('prefix: %s', instance.prefix)
    reply.send({ prefix: instance.prefix })
  }) instance.register(function (instance, opts, done) {
    instance.get('/bar', function (request, reply) {
      request.log.info('prefix: %s', instance.prefix)
      reply.send({ prefix: instance.prefix })
    }) done()
  }, { prefix: '/v2' }) done()
}, { prefix: '/v1' })const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()
```

然后当我们去`/v1/v2/bar`时，我们看到:

```
{
  "prefix": "/v1/v2"
}
```

已退回。如果我们去`/v1/foo`，我们会看到:

```
{
  "prefix": "/v1"
}
```

# 设置回复序列化程序

我们可以调用`setReplySerializer`方法为所有路由设置回复序列化程序。

例如，我们可以写:

```
const fastify = require('fastify')()fastify.setReplySerializer(function (payload, statusCode){
  return `status: ${statusCode}, content: ${payload}`
})fastify.get('/', async (request, reply) => {  
  return 'hello world'
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)
    await fastify.close()
    process.exit(1)
  }
}
start()
```

将序列化程序添加到我们的应用程序中。

# 设置未找到处理程序

我们可以为处理器 404 错误设置一个处理器。

例如，我们可以写:

```
const fastify = require('fastify')()fastify.setNotFoundHandler({
  preValidation: (req, reply, done) => {    
    done()
  },
  preHandler: (req, reply, done) => {    
    done()
  }
}, function (request, reply) {
    reply.send('not found')
})fastify.get('/', async (request, reply) => {  
  return 'hello world'
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)
    await fastify.close()
    process.exit(1)
  }
}
start()
```

在服务器实例上调用`setNotFoundHandler`。

我们还可以在一组 URL 上设置 not found 处理程序。

例如，我们可以写:

```
const fastify = require('fastify')()fastify.register(function (instance, options, done) {
  instance.setNotFoundHandler(function (request, reply) {
    return reply.send('not found')
  })
  done()
}, { prefix: '/v1' })fastify.get('/', async (request, reply) => {  
  return 'hello world'
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)
    await fastify.close()
    process.exit(1)
  }
}
start()
```

为以`/v1`开头的路线添加一个未找到的处理程序。

# 结论

我们可以关闭服务器，设置路由前缀，设置 not found 处理程序，并使用 Fastify 设置回复序列化程序。