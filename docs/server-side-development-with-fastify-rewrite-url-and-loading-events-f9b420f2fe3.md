# 使用 Fastify 进行服务器端开发—重写 URL 和加载事件

> 原文：<https://blog.devgenius.io/server-side-development-with-fastify-rewrite-url-and-loading-events-f9b420f2fe3?source=collection_archive---------5----------------------->

![](img/eb83d01c0e45c769f604591eaa3a930a.png)

Marc-Olivier Jodoin 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Fastify 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将了解如何使用 Fastify 创建后端应用程序。

# 重写 URL

我们可以用`rewriteUrl`选项重写 URL。

例如，我们可以写:

```
const fastify = require('fastify')({
  rewriteUrl (req) { 
    return req.url === '/hi' ? '/hello' : req.url;
  }
})fastify.get('/hello', (request, reply) => {  
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

然后，当我们到达`/hi`时，就会调用`/hello`路线。

# 服务器方法

`fastify.server`是`Fastify`工厂函数返回的节点核心服务器对象。

`after`方法让我们在当前插件和注册在其中的所有插件完成加载后运行代码。

例如，我们可以写:

```
const fastify = require('fastify')()
fastify
  .register((instance, opts, done) => {
    console.log('Current plugin')
    done()
  })
  .after(err => {
    console.log('After current plugin')
  })
  .register((instance, opts, done) => {
    console.log('Next plugin')
    done()
  })
  .ready(err => {
    console.log('Everything has been loaded')
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

然后我们看到:

```
Current plugin
After current plugin
Next plugin
Everything has been loaded
```

已登录控制台。

如果在没有函数的情况下调用`after`，那么它返回一个承诺。

例如，我们可以写:

```
(async ()=>{
  const fastify = require('fastify')()
  fastify.get('/', (request, reply) => {  
    reply.send('hello world')
  }) await fastify.register(async (instance, opts) => {
    console.log('Current plugin')
  }) await fastify.after()
  console.log('After current plugin') await fastify.register(async (instance, opts) => {
    console.log('Next plugin')
  }) await fastify.ready() try {
    await fastify.listen(3000, '0.0.0.0')
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
})()
```

我们用一个`async`函数调用`fastify.register`来加载我们的插件。

然后我们用`await`调用`fastify.after`在插件加载后运行代码。

我们可以调用`fastify.ready`来观察服务器何时准备好处理请求。

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
    process.exit(1)
  }
}
start()
```

向其添加回调。

如果我们没有传入回调，我们也可以让它返回一个承诺:

```
const fastify = require('fastify')()fastify.get('/', (request, reply) => {  
  reply.send('hello world')
})fastify.ready(err => {
  if (err) throw err
})const start = async () => {
  try {
    await fastify.listen(3000, '0.0.0.0')
    await fastify.ready()
    console.log('successfully booted!')
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()
```

# 倾听请求

我们可以用`fastify.listen`方法监听请求。

例如，我们写道:

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
    process.exit(1)
  }
}
start()
```

使用`fastify.listen`开始监听请求。

第一个参数是要监听的端口，第二个参数是要监听的 IP 地址。

# 结论

我们可以用 Fastify 重写 URL，收听各种事件。