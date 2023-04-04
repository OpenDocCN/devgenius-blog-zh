# Angular 项目的 Express.js 服务器

> 原文：<https://blog.devgenius.io/express-js-server-for-angular-project-2ab62f85a82c?source=collection_archive---------0----------------------->

Express.js 是 Node.js web 应用程序的轻量级框架。它创建了托管和管理 web/移动应用程序的服务器，具有快速和强大的性能。

它可用作 NPM 模块。这使得 Node.js 的安装成为使用 Express.js 的先决条件。

![](img/ac1814a16cc7430332ed63fbcc8c03f0.png)

马修·施瓦茨在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

要安装 express.js，请导航到 angular 项目目录(此处为'*演示应用程序【T5])，并通过 CLI 键入以下命令:*

```
npm install express
```

上面的代码行将在当前项目中安装 express (js)模块。使用 *-g* ( *-global* )标志全局安装模块。

创建一个 *server.js* 脚本文件，导入 express 模块。指定一个端口并调用 express app 变量上的 *listen()* 来激活您的服务器。

```
const express = require('express');
const app = express();
const port = 3000; ......//server logic app.listen(port, () => {
    console.log(`My Demo App listening on ${port} port`
});
```

服务器监听请求和响应。这些请求是 HTTP 请求。我们可以为所有 HTTP 请求创建回调。每个 HTTP 请求主要有三个参数供开发人员使用:

*   统一资源定位器
*   请求变量
*   响应变量

```
app.get('/demo-url1', (req, res) => {
  res.send('Get Request')
});app.post('/demo-url2', (req, res) => {
  res.send('Post Request')
});
```

对于所有与任何 URL 都不匹配的请求，express 返回一个 **404 Not Found** 错误。

服务器是完整的，可以通过运行服务器文件来使用。

```
node server.js
```

在 Angular 中，当我们创建一个项目时，会创建一个名为' *demo-app* '的父目录。它包含了 *src* 子目录，其中包含了我们应用程序的实际程序文件。在构建应用程序时，会在父目录中创建另一个子目录 *dist* 。 *dist* 目录是生产模式文件的容器。使用 *ng build* 构建 angular 项目。

```
ng build
```

我们的 angular 项目的单页应用程序可在*demo-app/dist/demo-app/index . html*获得。这是需要通过 HTTP 请求返回给客户端的网页。

由于 URL 路由由 angular 应用负责，express 服务器可以跳过这一步，将所有请求返回给 index.html 页面。“ *** ”用作“*任何 URL* ”的通配符。

响应参数变量有一个 *sendFile()* 方法，可用于在 HTTP 请求中返回文件。 *path* 模块导入解析响应中要发送的文件路径。

在上面的 *server.js* 文件中， *app.get* 监听所有 get 请求(这里' *** 代表'*任意 URL* ')，并将 *dist/demo-app/index.html* 文件返回给它们所有人。进一步由角度逻辑来处理不同的 URL。

注意:将 *server.js* 文件放在项目的父目录中。

在浏览器中打开 *localhost:3000* 运行中的服务器。

# **托管静态文件**

Angular 将所有静态文件存储在一个子目录中，项目名称创建在 *dist* 子目录下(即在' *dist/demo-app* '内)。和资产(图像、字体、图标、视频、音频等。)在*资产*文件夹下(即' *dist/demo-app/assets* '里面)。

例如，当我们需要访问一个图像文件时，我们必须通过这样的路径:“dist/demo-app/assets/image.png”。所以可以通过使用 *express.static* 中间件来减少这个长路径，提高安全性。因为虚拟路径是为我们的资源创建的，所以实际路径对公众是隐藏的。

```
app.use(express.static('dist/demo-app/'));
```

*完成 server.js*

要了解更多信息，请浏览 express.js 文档。