# 使用 Fastify 进行服务器端开发——路由级挂钩和装饰器

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-route-level-hooks-and-decorators-226f283c64fb?source=collection_archive---------1----------------------->

![](img/eefa90f291265090de7f6f2c9bdb75c9.png)

安德斯·吉尔登在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 路由级别挂钩

我们可以在路由级别添加挂钩。

为此，我们可以写:

```
const fastify = require('fastify')({})fastify.route({
  method: 'GET',
  url: '/',
  schema: { },
  onRequest (request, reply, done) {
    done()
  },
  onResponse (request, reply, done) {
    done()
  },
  preParsing (request, reply, done) {
    done()
  },
  preValidation (request, reply, done) {
    done()
  },
  preHandler (request, reply, done) {
    done()
  },
  preSerialization (request, reply, payload, done) {
    done(null, payload)
  },
  handler (request, reply) {
    reply.send({ hello: 'world' })
  }
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

加上钩子。

收到请求时运行`onRequest`。

发送响应时运行`onResponse`。

`preParsing`在解析请求时运行。

`preValidation`在对请求进行验证之前运行。

`preHandler`是共享`preHandler`钩子后运行的。

`preSerialization`在共享的`preSerialization`钩子之后运行，该钩子在响应被序列化之后运行。

`handler`是路由处理程序方法。

# 装修工

我们可以使用 decorators 定制核心 Fastify 对象，比如请求和回复对象。

要添加一个并使用它们，我们可以写:

```
const fastify = require('fastify')({})fastify.decorateRequest('user', '')fastify.addHook('preHandler', (req, reply, done) => {
  req.user = 'jane smith'
  done()
})fastify.get('/', (req, reply) => {
  reply.send(`Hello, ${req.user}!`)
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

我们调用`fastify.decoratorRequest`方法来添加一个`user`属性到`req`对象。

然后我们将`preHandler`钩子中的`req.user`属性设置为`'jane smith'`。

然后在我们的路由处理器中，我们可以访问这个值。

因此，我们得到:

```
Hello, jane smith!
```

作为响应返回。

我们可以使用`fastify.decorate`方法来修饰 Fastify 服务器实例。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.decorate('conf', {
  db: 'some.db',
  port: 3000
})fastify.get('/', (req, reply) => {  
  reply.send(`Hello ${fastify.conf.db}`)
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

向`fastify`对象添加数据。

我们给它添加了`conf`属性。

然后，我们可以在应用程序的任何地方访问该值。

所以我们应该得到:

```
Hello some.db
```

从回应来看。

修饰过的 Fastify 服务器也可以通过路由处理程序中的`this`关键字来访问。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.decorate('conf', {
  db: 'some.db',
  port: 3000
})fastify.get('/', async function (request, reply) {
  reply.send({hello: this.conf.db})
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

然后我们得到:

```
{"hello":"some.db"}
```

从回应来看。

# 结论

我们可以用 Fastify decorators 向 Fastify 实例或请求添加数据。

此外，我们可以在路由级别添加挂钩。