# 使用 Hapi.js 进行服务器端开发—会话和高级 HTTP 请求

> 原文：<https://blog.devgenius.io/server-side-development-with-hapi-js-sessions-and-advanced-http-requests-be0abc5114d5?source=collection_archive---------0----------------------->

![](img/ca5e346bff533ad8cd1ab64a0d4e5362.png)

图片由 [nine koepfer](https://unsplash.com/@enka80?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Hapi.js 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将看看如何用 Hapi.js 创建后端应用程序。

# 高级 HTTP 请求

我们可以用`@hapi/wreck`模块发出复杂的 HTTP 请求。

例如，我们可以写:

```
const Wreck = require('@hapi/wreck');
const Https = require('https')
const Http = require('http')const method = 'GET';
const uri = '/';
const readableStream = Wreck.toReadableStream('foo=bar');const wreck = Wreck.defaults({
  headers: { 'x-foo-bar': 123 },
  agents: {
    https: new Https.Agent({ maxSockets: 100 }),
    http: new Http.Agent({ maxSockets: 1000 }),
    httpsAllowUnauthorized: new Https.Agent({ maxSockets: 100, rejectUnauthorized: false })
  }
});const wreckWithTimeout = wreck.defaults({
  timeout: 5
});const options = {
  baseUrl: 'https://www.example.com',
  payload: readableStream || 'foo=bar' || Buffer.from('foo=bar'),
  headers: {},
  redirects: 3,
  beforeRedirect: (redirectMethod, statusCode, location, resHeaders, redirectOptions, next) => next(),
  redirected: function (statusCode, location, req) {},
  timeout: 1000,
  maxBytes: 1048576,
  rejectUnauthorized: false,
  agent: null,  
};const example = async () => {
  const promise = wreck.request(method, uri, options);
  try {
    const res = await promise;
    const body = await Wreck.read(res, options);
    console.log(body.toString());
  }
  catch (err) {
    console.log(err)
  }
};example()
```

属性有请求头。

`timeout`具有以毫秒为单位的请求超时。

`maxBytes`具有请求的最大大小。

`agent`拥有用户代理。

我们可以添加的`secureProtocol`是要使用的 SSL 方法。

并且可以添加`ciphers`属性来选择 TLS 密码。

然后我们使用`Wreck.read`来读取响应。

我们也可以调用`promise.req.abort()`来中止请求。

# 会话处理

我们可以用`@hapi/yar`模块处理会话数据。

例如，我们可以写:

```
const Hapi = require('@hapi/hapi');const server = Hapi.Server({ port: 3000 });
const options = {
  storeBlank: false,
  cookieOptions: {
    password: 'the-password-must-be-at-least-32-characters-long'    
  }
};const init = async () => {  
  await server.register({
    plugin: require('@hapi/yar'),
    options
  }); server.route({ 
    method: 'GET', 
    path: '/', 
    handler: (request, h) => {
      request.yar.set('example', { key: 'value' });
      return 'hello';
    }
  }); server.route({ 
    method: 'GET', 
    path: '/session', 
    handler: (request, h) => {
      const example = request.yar.get('example');      
      return example;
    }
  }); await server.start();
  console.log('Server running at:', server.info.uri);
};
init();
```

注册`@hapi/yar`插件:

```
await server.register({
  plugin: require('@hapi/yar'),
  options
});
```

`options`属性具有`cookieOptions.password`属性，用于设置加密会话的密码。

然后在`/`路由中，我们调用`request.yar.set`方法将键和值添加到会话中。

然后在`/session`路径中，我们用想要得到的键调用`request.yar.get`方法来获取数据。

我们应该得到:

```
{
  "key": "value"
}
```

作为回应。

# 结论

我们可以使用`@hapi/wreck`模块发出高级 HTTP 请求。

`@hapi/yar`模块让我们可以管理应用程序中的会话数据。