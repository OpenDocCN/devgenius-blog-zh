# 使用 Fastify 进行服务器端开发——异步、等待和路由前缀

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-async-and-await-and-route-prefixes-d5704eb5206?source=collection_archive---------2----------------------->

![](img/4c13c52356221329929340c959b43c6f.png)

[哈雷戴维森](https://unsplash.com/@harleydavidson?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 异步等待

我们可以在路由处理程序中使用`async`和`await`。

例如，我们可以写:

```
const fastify = require('fastify')()const getData = () => Promise.resolve('foo')
const processData = (data) => Promise.resolve(data)const opts = {
  schema: {
    response: {
      200: {
        type: 'string'
      }
    }
  }
}fastify.get('/', opts, async function (request, reply) {
  const data = await getData()
  const processed = await processData(data)
  return processed
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

我们使用异步函数作为路由处理器。

然后我们可以在处理程序中使用`await`来运行承诺。

我们也可以使用`reply.send`发送回数据。

例如，我们可以写:

```
const fastify = require('fastify')()const getData = () => Promise.resolve('foo')
const processData = (data) => Promise.resolve(data)const opts = {
  schema: {
    response: {
      200: {
        type: 'string'
      }
    }
  }
}fastify.get('/', opts, async function (request, reply) {
  const data = await getData()
  const processed = await processData(data)
  reply.send(processed)
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

打电话给`reply.send`发回回复。

如果我们在异步回调中调用`reply.send`，我们也可以等待我们的回复。

例如，我们可以写:

```
const fastify = require('fastify')()const getData = () => Promise.resolve('foo')
const processData = (data) => Promise.resolve(data)const opts = {
  schema: {
    response: {
      200: {
        type: 'object',
        properties: {
          hello: {
            type: 'string'
          }
        }
      }
    }
  }
}fastify.get('/', opts, async function (request, reply) {
  setImmediate(() => {
    reply.send({ hello: 'world' })
  })
  await reply
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

等待我们的`reply`，因为我们在`setImmediate`回调中调用了`reply.send`。

同时使用`return value`和`reply.send(value)`时，先发生的优先。

如果我们将`async`和`await`与`reply.send`一起使用，那么我们不应该返回任何值。

如果我们使用带有承诺的`async`和`await`，那么我们应该返回值，不要使用`reply.send`或返回`undefined`。

# 路由前缀

我们可以给路由添加前缀。

例如，我们可以写:

`index.js`

```
const fastify = require('fastify')()fastify.register(require('./routes/v1/users'), { prefix: '/v1' })
fastify.register(require('./routes/v2/users'), { prefix: '/v2' })const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

`routes/v1/users.js`

```
module.exports = function (fastify, opts, done) {
  fastify.get('/user', async () => 'v1')
  done()
}
```

`routes/v2/users.js`

```
module.exports = function (fastify, opts, done) {
  fastify.get('/user', async () => 'v2')
  done()
}
```

然后当我们转到/v1/user 时，我们看到了`v1`。

当我们转到/v2/user 时，我们会看到`v2`。

# 结论

我们可以添加异步函数作为路由处理器。

我们可以用 Fastify 添加路由前缀。