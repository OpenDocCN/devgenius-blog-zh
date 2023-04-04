# 使用 Fastify 进行服务器端开发—自定义日志记录和路由配置

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-custom-logging-and-route-config-c052acef9432?source=collection_archive---------2----------------------->

![](img/ef110dc85921249759eadf6c7472db36.png)

由[卢卡·坎皮奥尼](https://unsplash.com/@nexgenfx?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 自定义日志级别

我们可以更改 Fastify 应用程序的日志级别。

例如，我们可以写:

`index.js`

```
const fastify = require('fastify')({
  logger: true 
})fastify.register(require('./routes/v1/users'), { prefix: '/v1' })
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

`routes/v2/user.js`

```
module.exports = function (fastify, opts, done) {
  fastify.get('/user', async () => 'v2')
  done()
}
```

我们将`logger`设置为`true`来启用日志记录。

我们还可以为单个路由设置日志记录级别。

例如，我们可以写:

```
const fastify = require('fastify')()fastify.get('/', { logLevel: 'warn' }, (request, reply) => {
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

我们在`/`路线上设置`logLevel`到`'warn'`。

# 自定义日志序列化程序

我们可以定制日志序列化程序。

例如，我们可以写:

```
const fastify = require('fastify')({
  logger: {
    level: 'info',
    serializers: {
      user (req) {
        return {
          method: req.method,
          url: req.url,
          headers: req.headers,
          hostname: req.hostname,
          remoteAddress: req.ip,
          remotePort: req.socket.remotePort
        }
      }
    }
  }
})fastify.register(context1, {
  logSerializers: {
    user: value => `My serializer father - ${value}`
  }
})async function context1 (fastify, opts) {
  fastify.get('/', (req, reply) => {
    req.log.info({ user: 'call father serializer', key: 'another key' })
    reply.send({})
  })
}
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

添加`logSerializers`属性。

当我们在第一行调用 Fastify 工厂函数时，我们配置了日志记录器。

我们设置`level`来设置日志级别。

`serializers`有一个带有`user`函数的对象，它从请求中返回信息。

我们分别返回请求方法、URL、头、主机名、IP 和远程端口。

然后，当我们发出请求时，我们会在控制台中看到输出。

# 路线配置

我们可以在定义路由时传入一个配置对象。

例如，我们可以写:

```
const fastify = require('fastify')()function handler (req, reply) {
  reply.send(reply.context.config.output)
}fastify.get('/en', { config: { output: 'hello world!' } }, handler)
fastify.get('/fr', { config: { output: 'bonjour' } }, handler)const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')  
  } catch (err) {
    fastify.log.error(err)    
    process.exit(1)
  }
}
start()
```

我们在第二个参数中传入一个属性为`config`的对象。

然后我们可以从`reply.context`属性中获取该值。

# 结论

我们可以使用 Fastify 设置自定义日志和路由配置。