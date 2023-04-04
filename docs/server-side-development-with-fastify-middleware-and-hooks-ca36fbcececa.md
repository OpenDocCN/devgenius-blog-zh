# 使用 Fastify 进行服务器端开发——中间件和钩子

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-middleware-and-hooks-ca36fbcececa?source=collection_archive---------2----------------------->

![](img/1234b1ecd8e20c546d723d095499ac45.png)

照片由[米勒·塞甘·🇨🇦](https://unsplash.com/@emileseguin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 中间件

从版本 3 开始，Fastify 框架就不再支持中间件了。

如果我们想增加对 Express 风格中间件的支持，我们需要安装`fastify-express`或`middle`插件。

例如，我们可以写:

```
const fastify = require('fastify')()const start = async () => {
  try {
    await fastify.register(require('fastify-express'))
    fastify.use(require('cors')()) fastify.get('/', function (request, reply) {
      reply.send({ hello: 'world' })
    }) await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

我们补充:

```
await fastify.register(require('fastify-express'))
fastify.use(require('cors')())
```

用`cors`中间件添加`fastify-express`插件。

为了使用`middle`模块，我们编写:

```
const fastify = require('fastify')()const start = async () => {
  try {
    await fastify.register(require('middie'))
    fastify.use(require('cors')()) fastify.get('/', function (request, reply) {
      reply.send({ hello: 'world' })
    }) await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

添加中间件，添加`cors`中间件。

# 将中间件的执行限制在特定的路径上

我们可以将中间件的执行限制在特定的路径上。

例如，我们可以写:

```
const fastify = require('fastify')({})
const path = require('path')const start = async () => {
  try {
    await fastify.register(require('fastify-express'))
    const serveStatic = require('serve-static') fastify.use('/css', serveStatic(path.join(__dirname, '/assets')))
    fastify.use('/css/(.*)', serveStatic(path.join(__dirname, '/assets')))
    fastify.use(['/css', '/js'], serveStatic(path.join(__dirname, '/assets'))) fastify.get('/', function (request, reply) {
      reply.send({ hello: 'world' })
    }) await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

我们用中间件运行的路径作为第一个参数来调用`fastify.use`。

第二个参数是我们想要运行的中间件。

这不适用于带参数的路径。

Fastify 为最常用的中间件提供了一些替代方案，比如用`[fastify-helmet](https://github.com/fastify/fastify-helmet)`代替`[helmet](https://github.com/helmetjs/helmet)`，用`[fastify-cors](https://github.com/fastify/fastify-cors)`代替`[cors](https://github.com/expressjs/cors)`，用`[fastify-static](https://github.com/fastify/fastify-static)`代替`[serve-static](https://github.com/expressjs/serve-static)`。

# 钩住

我们可以用`fastify.addHook`方法注册钩子。

例如，我们可以写:

```
const fastify = require('fastify')({})
const asyncMethod = () => Promise.resolve('foo')const start = async () => {
  try {
    fastify.addHook('onRequest', async (request, reply) => {
      await asyncMethod()
      return
    }) fastify.get('/', function (request, reply) {
      reply.send({ hello: 'world' })
    }) await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

为`onRequest`事件调用`addHook`。

此外，我们可以写:

```
const fastify = require('fastify')({})
fastify.addHook('onRequest', (request, reply, done) => {
  done()
})
const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send({ hello: 'world' })
    }) await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

做同样的事情。我们调用`done`来指示钩子已经完成运行。

# 结论

我们可以用 Fastify 添加 Express 中间件和钩子。