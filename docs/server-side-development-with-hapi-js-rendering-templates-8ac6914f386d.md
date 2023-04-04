# 使用 Hapi.js 进行服务器端开发—渲染模板

> 原文：<https://blog.devgenius.io/server-side-development-with-hapi-js-rendering-templates-8ac6914f386d?source=collection_archive---------3----------------------->

![](img/6e2af20d3c5826d859aba25d46c6dc7c.png)

照片由[海伦娜·洛佩斯](https://unsplash.com/@wildlittlethingsphoto?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Hapi.js 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将看看如何用 Hapi.js 创建后端应用程序。

# 渲染模板

我们可以用`@hapi/vision`模块渲染模板。

例如，我们可以写:

`index.js`

```
const Ejs = require('ejs');
const Hapi = require('@hapi/hapi');
const Vision = require('@hapi/vision');const server = Hapi.Server({ port: 3000 });const rootHandler = (request, h) => {
  return h.view('index', {
    title: request.server.version,
    message: 'Hello Ejs!'
  });
};const init = async () => {
  await server.register(Vision); server.views({
    engines: { ejs: Ejs },
    relativeTo: __dirname,
    path: 'templates'
  }); server.route({ method: 'GET', path: '/', handler: rootHandler }); await server.start();
  console.log('Server running at:', server.info.uri);
};init();
```

`templates/index.ejs`

```
<%= message %>
```

我们有`await server.register(Vision);`来注册`@hapi/vision`插件。

然后我们调用`h.view`来渲染视图。

`title`和`message`将在模板中可用。

我们将`engines.ejs`属性设置为`Ejs`模块，将其用作我们的渲染引擎。

我们可以通过编写以下代码来使用车把视图引擎:

`index.js`

```
const Handlebars = require('handlebars');
const Hapi = require('@hapi/hapi');
const Vision = require('@hapi/vision');const server = Hapi.Server({ port: 3000 });const rootHandler = (request, h) => {
  return h.view('index', {
    title: request.server.version,
    message: 'Hello!'
  });
};const init = async () => {
  await server.register(Vision); server.views({
    engines: { html: Handlebars },
    relativeTo: __dirname,
    path: 'templates'
  }); server.route({ method: 'GET', path: '/', handler: rootHandler }); await server.start();
  console.log('Server running at:', server.info.uri);
};init();
```

`templates/index.html`

```
{{message}}
```

我们将`engines`属性更改为`{ html: Handlebars }`，并要求`handlebars`模块使用车把视图引擎。

要将 Pug 视图引擎用于哈比神，我们编写:

`index.js`

```
const Pug = require('pug');
const Hapi = require('@hapi/hapi');
const Vision = require('@hapi/vision');const server = Hapi.Server({ port: 3000 });const rootHandler = (request, h) => {
  return h.view('index', {
    title: request.server.version,
    message: 'Hello!'
  });
};const init = async () => {
  await server.register(Vision); server.views({
    engines: { pug: Pug },
    relativeTo: __dirname,
    path: 'templates'
  }); server.route({ method: 'GET', path: '/', handler: rootHandler }); await server.start();
  console.log('Server running at:', server.info.uri);
};init();
```

`templates/index.pug`

```
p #{message}
```

为了使用 Mustache 视图引擎，我们编写:

```
const Mustache = require('mustache');
const Hapi = require('@hapi/hapi');
const Vision = require('@hapi/vision');const server = Hapi.Server({ port: 3000 });const rootHandler = (request, h) => {
  return h.view('index', {
    title: request.server.version,
    message: 'Hello!'
  });
};const init = async () => {
  await server.register(Vision); server.views({
    engines: {
      html: {
        compile: (template) => {
          Mustache.parse(template);
          return (context) => {
            return Mustache.render(template, context);
          };
        }
      }
    },
    relativeTo: __dirname,
    path: 'templates'
  }); server.route({ method: 'GET', path: '/', handler: rootHandler }); await server.start();
  console.log('Server running at:', server.info.uri);
};init();
```

`templates/index.html`:

```
{{message}}
```

我们有`engines.html.compile`方法将模板编译成呈现的 HTML。

`context`将我们传递的数据放入`h.view`的第二个参数中。

# 结论

我们可以用`@hapi/vision`插件将各种模板转换成 HTML。