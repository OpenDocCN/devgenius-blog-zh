# Nuxt.js —插件和模块

> 原文：<https://blog.devgenius.io/nuxt-js-plugins-and-modules-b0364dab5611?source=collection_archive---------4----------------------->

![](img/8603b615fb41c44401b6059b1afb39e8.png)

照片由[晨酿](https://unsplash.com/@morningbrew?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Nuxt.js 是一个基于 Vue.js 的应用框架。

我们可以用它来创建服务器端渲染应用和静态站点。

在本文中，我们将了解如何在客户端和服务器端环境中使用插件并创建模块。

# 客户端或服务器端插件

我们可以将插件配置为只在客户端或服务器端可用。

一种方法是在文件名中添加`client.js`来创建一个客户端专用插件。

我们可以在文件名中添加`server.js`来创建一个服务器端的插件。

为此，在`nuxt.config.js`中，我们可以这样写:

```
export default {
  plugins: [
    '~/plugins/foo.client.js',
    '~/plugins/bar.server.js',
    '~/plugins/baz.js'
  ]
}
```

如果没有后缀，那么插件在所有环境中都可用。

我们可以用对象语法做同样的事情。

例如，我们可以写:

```
export default {
  plugins: [
    { src: '~/plugins/both-sides.js' },
    { src: '~/plugins/client-only.js', mode: 'client' },
    { src: '~/plugins/server-only.js', mode: 'server' }
  ]
}
```

可以将`mode`属性设置为`'client'`，使插件在客户端可用。

为了使插件在服务器端可用，我们可以将`mode`设置为`'server'`。

对于只在服务器端可用的插件，我们可以在运行代码之前检查插件代码中的`process.server`是否为`true`。

同样，在静态页面上运行插件代码之前，我们可以检查`process.static`是否是`true`。

# Nuxt.js 模块

js 附带了一些模块，我们可以用它们来扩展 Nuxt 的核心功能。

`@nuxt/http`用于发出 HTTP 请求。

`@nuxt/content`用于通过类似 MongoDB 的 API 编写内容和获取 Markdown、JSON、YAML 和 CSV 文件。

`@nuxtjs/axios`是一个模块，用于 Axios 集成以发出 HTTP 请求。

`@nuxtjs/pwa`用于创建 PWAs。

`@nuxtjs/auth`用于添加认证。

# 编写一个模块

我们可以创建自己的模块。

要添加一个，我们可以在`modules`文件夹中创建一个文件。

例如，我们可以创建一个`modules/simple.js`文件，并编写:

```
export default function SimpleModule(moduleOptions) {
  // ...
}
```

然后我们可以将该模块添加到`nuxt.config.js`中，以便我们可以使用它:

```
modules: [
  ['~/modules/simple', { token: '123' }]
],
```

然后将第二个条目中的对象作为参数传递给`SimpleModule`函数。

模块可以是异步的。

# 仅生成模块

我们可以创建只构建模块，并将它们放在`nuxt.config.js`的`buildModules`数组中。

例如，我们可以写:

`modules/async.js`

```
import fse from 'fs-extra'export default async function asyncModule() {
  const pages = await fse.readJson('./pages.json')
  console.log(pages);
}
```

我们添加了`fs-extra`模块来读取文件。

该函数是异步的，所以它返回一个承诺，解析后的值就是我们返回的值。

在`nuxt.config.js`中，我们加上:

```
buildModules: [
  '~/modules/async'
],
```

添加我们的模块。

该模块将在我们运行开发服务器或构建时加载。

# 结论

我们可以用 Nuxt 创建在客户端或服务器端可用的模块和插件。