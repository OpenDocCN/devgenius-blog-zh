# 使用 Fastify 进行服务器端开发——事件和错误挂钩

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-event-and-error-hooks-19852a848aed?source=collection_archive---------1----------------------->

![](img/517ffc9b31efef29e83945c88041e06e.png)

[产品学校](https://unsplash.com/@productschool?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 翁森

我们可以用`onSend`钩子改变发送的响应负载。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('onSend', (request, reply, payload, done) => {
  const err = null;
  const newPayload = payload.replace('some-text', 'some-new-text')
  done(err, newPayload)
})
const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

我们用`'some-new-text'`代替`'some-text'`响应。

然后，当我们向`/`发出 GET 请求时，我们看到`'some-new-text’`被返回。

同样，我们可以用一个`async`函数做同样的事情:

```
const fastify = require('fastify')({})fastify.addHook('onSend', async (request, reply, payload) => {
  const newPayload = payload.replace('some-text', 'some-new-text')
  return newPayload
})const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

我们还可以使用以下命令删除响应负载:

```
const fastify = require('fastify')({})fastify.addHook('onSend', (request, reply, payload, done) => {
  reply.code(304)
  const newPayload = null
  done(null, newPayload)
})const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

# 响应时

当一个响应被发送时,`onResponse`钩子让我们运行代码。

这是一个收集统计数据的有用地方。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('onResponse', (request, reply, done) => {  
  console.log(reply)
  done()
})const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

来记录`reply`。

我们也可以写:

```
const fastify = require('fastify')({})fastify.addHook('onResponse', async (request, reply) => {
  console.log(reply)  
  return
})const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

做同样的事情。

# 准时结束

`onTimeout`钩子让我们监控请求超时。

例如，我们可以这样使用它:

```
const fastify = require('fastify')({})fastify.addHook('onTimeout', (request, reply, done) => {
  console.log('timed out')
  done()
})const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

或者:

```
const fastify = require('fastify')({})fastify.addHook('onTimeout', async (request, reply) => {
  console.log('timed out')
  return
})const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

记录任何请求超时。

# 管理钩子中的错误

我们可以在钩子中抛出错误，如果有任何错误，就返回错误响应。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('preHandler', (request, reply, done) => {
  reply.code(400)
  done(new Error('Some error'))
})const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

添加`preHandler`钩子并返回 400 个错误。

我们还可以在`done`函数中传递一个`Error`实例。

此外，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('onResponse', async (request, reply) => {
  throw new Error('Some error')
})const start = async () => {
  try {
    fastify.get('/', function (request, reply) {
      reply.send('some-text')
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

用`async`功能做同样的事情。

# 结论

我们可以用 Fastify 添加各种钩子来处理事件和错误。