# 使用 Hapi.js 进行服务器端开发—呈现模板和 HTTP 请求

> 原文：<https://blog.devgenius.io/server-side-development-with-hapi-js-rendering-templates-and-http-requests-1fe37c8506bc?source=collection_archive---------10----------------------->

![](img/3d0e2e1fcf9bfc9ebc4e6d771acd28e0.png)

照片由[诺亚·西利曼](https://unsplash.com/@noahsilliman?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Hapi.js 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将看看如何用 Hapi.js 创建后端应用程序。

# 渲染模板

我们可以用`@hapi/vision`插件在我们的哈比神应用程序中渲染 Nunjucks 模板。

为此，我们写道:

`index.js`

```
const Nunjucks = require('nunjucks');
const Hapi = require('@hapi/hapi');
const Vision = require('@hapi/vision');const server = Hapi.Server({ port: 3000 });const rootHandler = (request, h) => {
  return h.view('index', {
    title: request.server.version,
    message: 'Hello!'
  });
};const init = async () => {
  await server.register(Vision);server.views({
    engines: {
      html: {
        compile: (src, options) => {
          const template = Nunjucks.compile(src, options.environment);
          return (context) => {
            return template.render(context);
          };
        },
        prepare: (options, next) => {
          options.compileOptions.environment = Nunjucks.configure(options.path, { watch : false });
          return next();
        }
      }
    },
    relativeTo: __dirname,
    path: 'templates'
  });server.route({ method: 'GET', path: '/', handler: rootHandler });await server.start();
  console.log('Server running at:', server.info.uri);
};init();
```

`templates/index.html`

```
{{message}}
```

我们需要`Nunjucks`模块。

然后我们用`src`参数调用`Nunjucks.compile`来将源代码编译成模板。

然后我们返回一个用`context`调用`template.render`的函数来呈现模板。

`context`将`h.view`的第二个参数作为其值。

在`compile`方法之前调用`prepare`方法，它设置`environment`选项设置特定于环境的设置。

`index.html`有模板内容。

`@hapi/vision`也支持渲染树枝模板。

为此，我们写道:

```
const Twig = require('twig');
const Hapi = require('@hapi/hapi');
const Vision = require('@hapi/vision');const server = Hapi.Server({ port: 3000 });const rootHandler = (request, h) => {
  return h.view('index', {
    title: request.server.version,
    message: 'Hello!'
  });
};const init = async () => {
  await server.register(Vision); server.views({
    engines: {
      twig: {
        compile: (src, options) => {
          const template = Twig.twig({ id: options.filename, data: src });
          return (context) => {
            return template.render(context);
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

`templates/index.twig`

```
{{ message }}
```

我们将`engines.twig.compile`属性设置为一个函数。

在其中，我们调用`Twig.twig`来设置模板的`id`和`data`。

我们返回一个带`context`参数的函数，并对其调用`template.render`。

有我们作为第二个参数传入的对象。

`index.twig`是我们的小枝模板。

# 发出 HTTP 请求

我们可以用`@hapi/wreck`模块在我们的节点应用程序中发出 HTTP 请求。

例如，我们可以写:

```
const Wreck = require('@hapi/wreck');const makeRequest = async () => {
  const { res, payload } = await Wreck.get('http://example.com');
  console.log(payload.toString());
};try {
  makeRequest();
}
catch (ex) {
  console.error(ex);
}
```

我们需要`@hapi/wreck`模块。

然后我们调用`Wreck.get`发出 GET 请求。

然后我们得到带有`payload`属性的响应。

我们用`toString`方法把它转换成一个字符串。

# 结论

我们可以用`@hapi/vision`模块渲染各种模板。

此外，我们可以在节点应用程序中使用`@hapi/wreck`模块发出 HTTP 请求。