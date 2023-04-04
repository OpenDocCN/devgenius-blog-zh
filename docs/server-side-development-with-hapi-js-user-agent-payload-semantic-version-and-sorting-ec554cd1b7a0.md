# 使用 Hapi.js 进行服务器端开发——用户代理、有效负载、语义版本和排序

> 原文：<https://blog.devgenius.io/server-side-development-with-hapi-js-user-agent-payload-semantic-version-and-sorting-ec554cd1b7a0?source=collection_archive---------1----------------------->

![](img/68b616305bfbe60006d69e50a27b6c2c.png)

照片由[布鲁克·卡吉尔](https://unsplash.com/@brookecagle?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Hapi.js 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将看看如何用 Hapi.js 创建后端应用程序。

# 获取用户代理信息

我们可以用`@hapi/scooter`模块从请求中获取用户代理信息。

为了使用它，我们写:

```
const Hapi = require('@hapi/hapi');
const Scooter = require('@hapi/scooter');const init = async () => {
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); server.route({
    method: 'GET',
    path: '/',
    config: {
      handler(request, h) {
        return request.plugins.scooter;
      },
    }
  });
  await server.register(Scooter);
  await server.start();  
  console.log('Server running at:', server.info.uri);
};process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});init();
```

我们有`await server.register(Scooter);`来注册 Scooter 插件。

我们使用`request.plugins.scooter`来返回用户代理对象。

# 语义版本控制

我们可以使用`@hapi/somever`模块来处理语义版本号。

例如，我们可以写:

```
const Hapi = require('@hapi/hapi');
const somever = require('@hapi/somever');const init = async () => {
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); server.route({
    method: 'GET',
    path: '/',
    config: {
      handler(request, h) {
        return somever.compare('1.2.3', '1.2.5');
      },
    }
  });
  await server.start();
  console.log('Server running at:', server.info.uri);
};process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});init();
```

用`somever.compare`法比较 2 个版本号。

然后我们得到-1，因为第一个版本号比第二个版本号早。

此外，我们可以使用`match`方法比较版本:

```
const Hapi = require('@hapi/hapi');
const somever = require('@hapi/somever');const init = async () => {
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); server.route({
    method: 'GET',
    path: '/',
    config: {
      handler(request, h) {
        return somever.match('1.2.x', '1.2.5');
      },
    }
  });
  await server.start();
  console.log('Server running at:', server.info.uri);
};process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});init();
```

因为`'1.2.x'`包含`'1.2.5'`，所以我们得到`true`。

# 解析 HTTP 负载

我们可以用`@hapi/subtext`模块解析 HTTP 负载。

例如，我们可以写:

```
const Http = require('http');
const Subtext = require('@hapi/subtext');Http.createServer(async (request, response) => {
  const { payload, mime } = await Subtext.parse(request, null, { parse: true, output: 'data' });
  response.writeHead(200, { 'Content-Type': 'text/plain' });
  response.end(`Payload contains: ${JSON.stringify(payload)}`);}).listen(1337, '0.0.0.0');console.log('Server running/');
```

我们用`request`对象调用`Subtext.parse`来解析它。

它返回一个带有带有`payload`和`mime`属性的对象的承诺。

`payload`有效载荷已发送。

`mime`具有 MIME 类型。

# 排序文本

我们可以用`@hapi/topo`模块对文本进行排序。

为了使用它，我们写:

```
const Hapi = require('@hapi/hapi');
const Topo = require('@hapi/topo');const morning = new Topo.Sorter();morning.add('Nap', { after: ['breakfast', 'prep'] });
morning.add([
  'Make toast',
  'Pour juice'
], { before: 'breakfast', group: 'prep' });
morning.add('Eat breakfast', { group: 'breakfast' });const init = async () => {
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  });
  server.route({
    method: 'GET',
    path: '/',
    config: {
      handler(request, h) {
        return morning.nodes;
      },
    }
  });
  await server.start();
  console.log('Server running at:', server.info.uri);
};
process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});
init();
```

我们用`Topo.Sorter`构造函数创建了`morning`对象。

然后我们调用`morning.add`来添加我们想要排序的项目。

属性指定了第一个参数中项目之后的项目。

`group`让我们将物品分组。

然后我们可以用`morning.nodes`属性得到排序后的项目。

# 结论

我们可以用`@hapi/topo`模块对物品进行分类。

`@hapi/subtext`模块让我们解析请求负载。

并且`@hapi/scooter`返回用户代理信息。

让我们比较语义版本字符串。