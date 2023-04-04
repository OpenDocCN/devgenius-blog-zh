# 使用 Fastify 进行服务器端开发— Hooks

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-hooks-cde209d1898d?source=collection_archive---------4----------------------->

![](img/bb19ffc2b50e957286fc70d65e4a3d0c.png)

安德斯·吉尔登在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 准备挂钩

`preParsing`钩子让我们在解析请求有效负载流之前对其进行转换。

如果它返回值，它必须返回一个流。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('preParsing', (request, reply, payload, done) => {    
  done(null, payload)
})const start = async () => {
  try {
    fastify.post('/', function (request, reply) {
      reply.send()
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

将`payload`传递给`done`函数。

此外，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('preParsing', async (request, reply, payload) => {
  return payload
})
const start = async () => {
  try {
    fastify.post('/', function (request, reply) {
      reply.send()
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

向`payload`返回承诺解析。

# 预验证

`preValidation`钩子让我们在验证请求之前运行代码。

它在`preParsing`挂钩后运行。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('preValidation', (request, reply, done) => {
  done()
})const start = async () => {
  try {
    fastify.post('/', function (request, reply) {
      reply.send()
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

我们可以在回调中添加一些检查。

我们也可以写:

```
const fastify = require('fastify')({})fastify.addHook('preValidation', async (request, reply) => {   
  return
})const start = async () => {
  try {
    fastify.post('/', function (request, reply) {
      reply.send()
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

# 前序列化

`preSerialization`挂钩允许在有效载荷序列化之前对其进行更改或替换。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('preSerialization', (request, reply, payload, done) => {
  const err = null
  const newPayload = { wrapped: payload }
  done(err, newPayload)
})const start = async () => {
  try {
    fastify.post('/', function (request, reply) {
      reply.send(request.body)
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

然后，当我们用有效载荷向`/`发出 POST 请求时:

```
{
    "abc": 123
}
```

我们得到:

```
{
    "wrapped": {
        "abc": 123
    }
}
```

作为回应。

等价地，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('preSerialization', async (request, reply, payload) => {
  return { wrapped: payload }
})const start = async () => {
  try {
    fastify.post('/', function (request, reply) {
      reply.send(request.body)
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

得到同样的结果。

如果有效负载是字符串、缓冲区、流或`null`，则不调用这个钩子。

# 错误

钩子让我们捕捉应用程序中出现的错误。

例如，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('onError', (request, reply, error, done) => {
  done()
})const start = async () => {
  try {
    fastify.post('/', function (request, reply) {
      reply.send(request.body)
    })
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

捕捉错误。`error`有错误数据。

同样，我们可以写:

```
const fastify = require('fastify')({})fastify.addHook('onError', async (request, reply, error) => {
  console.log(error)
})const start = async () => {
  try {
    fastify.post('/', function (request, reply) {
      reply.send(request.body)
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

# 结论

我们可以添加钩子，用 Fastify 收听各种事件。