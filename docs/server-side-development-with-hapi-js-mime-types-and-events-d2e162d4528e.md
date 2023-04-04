# 使用 Hapi.js 进行服务器端开发— MIME 类型和事件

> 原文：<https://blog.devgenius.io/server-side-development-with-hapi-js-mime-types-and-events-d2e162d4528e?source=collection_archive---------9----------------------->

![](img/3b5d2f75c6e9051fe690b3c54f205be7.png)

[恒谷](https://unsplash.com/@tsuneya?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hapi.js 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将看看如何用 Hapi.js 创建后端应用程序。

# MIME 类型

我们可以通过模块获得各种 MIME 类型的信息。

例如，我们可以写:

```
const Hapi = require('@hapi/hapi');
const Mimos = require('@hapi/mimos');const options = {
  override: {
    'node/module': {
      source: 'iana',
      compressible: true,
      extensions: ['node', 'module', 'npm'],
      type: 'node/module'
    },
    'application/javascript': {
      source: 'iana',
      charset: 'UTF-8',
      compressible: true,
      extensions: ['js', 'javascript'],
      type: 'text/javascript'
    },
    'text/html': {
      predicate(mime) {
        mime.foo = 'test';
        return mime;
      }
    }
  }
}const mimos = new Mimos(options);const init = async () => {
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); server.route({
    method: 'GET',
    path: '/',
    config: {
      handler(request, h) {
        return mimos.type('text/html');
      },
    }
  }); await server.start();
  console.log('Server running at:', server.info.uri);
};process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});
init();
```

我们添加带有`override`属性的`options`对象来为给定的 MIME 类型设置数据。

然后我们用我们想要查找的 MIME 类型调用`mimos.type`。

然后我们得到:

```
{"source":"iana","compressible":true,"extensions":["html","htm","shtml"],"type":"text/html","foo":"test"}
```

作为返回值。

# 收集服务器运营数据

我们可以通过`@hapi/oppsy`模块轻松收集服务器运营数据。

要使用它，我们可以写:

```
const Hapi = require('@hapi/hapi');
const Oppsy = require('@hapi/oppsy');const init = async () => {
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); const oppsy = new Oppsy(server);
  oppsy.on('ops', (data) => {
    console.log(data);
  }); server.route({
    method: 'GET',
    path: '/',
    config: {
      handler(request, h) {
        return 'hello'
      },
    }
  }); await server.start();
  oppsy.start(1000);
  console.log('Server running at:', server.info.uri);
};process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});
init();
```

我们用`Oppsy`构造函数创建了`oppsy`对象。

然后我们用`oppsy.on`方法监听`ops`事件。

在事件处理程序中，我们记录来自服务器的`data`。

它包括请求、CPU 使用、响应时间、内存使用等数据。

# 事件

我们可以用`@hapi/podium`模块创建自己的事件总线。

例如，我们可以写:

```
const Hapi = require('@hapi/hapi');
const Podium = require('@hapi/podium');const emitter = new Podium()
const context = { count: 0 }emitter.registerEvent({
  name: 'event',
  channels: ['ch1', 'ch2']
})const handler1 = function () {
  ++this.count
  console.log(this.count)
};const handler2 = function () {
  this.count = this.count + 2
  console.log(this.count)
}emitter.on({
  name: 'event',
  channels: ['ch1']
}, handler1, context);emitter.on({
  name: 'event',
  channels: ['ch2']
}, handler2, context)emitter.emit({
  name: 'event',
  channel: 'ch1'
})emitter.emit({
  name: 'event',
  channel: 'ch2'
})emitter.hasListeners('event') 
emitter.removeAllListeners('event') 
const init = async () => {
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); server.route({
    method: 'GET',
    path: '/',
    config: {
      handler(request, h) {
        return 'hello'
      },
    }
  }); await server.start();
  console.log('Server running at:', server.info.uri);
};process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});
init();
```

我们导入模块，然后用它创建`emitter`对象。

然后我们用`emitter.registerEvent`方法注册事件。

我们可以将事件分成各自的通道。

`context`是事件处理程序中`this`的值。

因此`this.count`将被更新并记录在事件处理程序中。

# 结论

我们可以用`@hapi/podium`创建自己的事件总线，用`@hapi/mimos`模块处理 MIME 类型。