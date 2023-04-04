# Node.js 提示—发出 HTTP 请求、发送响应和文件操作

> 原文：<https://blog.devgenius.io/node-js-tips-making-http-requests-express-packages-and-file-operations-f610500dd304?source=collection_archive---------23----------------------->

![](img/d88cbf8ff1d3a6334cdea4fad3e31627.png)

米切尔·霍兰德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 用 fs 定位父文件夹

我们可以用这两个点去父文件夹。

例如，我们可以写:

```
fs.readFile(path.join(__dirname, '/../../foo.txt'));
```

我们必须上一层楼。

2 会上升两级。

正斜杠也是必需的。

# Node.js 应用程序中如何解释“use strict”

Node 解释`'use strict'`的方式与其他 JavaScript 应用程序相同。

它允许检查未声明的变量，不允许设置常量值，如`undefined`等等。

没有严格模式的静默非法操作被转换为错误。

ES6 模块默认启用严格模式，因此我们不必显式启用它。

# 快速 HTTP GET 请求

要在节点应用程序中发出 GET 请求，我们可以使用`requestify` HTTP 客户端发出请求。

例如，我们可以写:

```
var requestify = require('requestify');requestify.get('http://example.com/')
  .then((response) => {
    response.getBody();
  }
);
```

我们可以在我们的快递应用程序中的任何地方提出我们的请求。

例如，当客户端发出请求时，我们可以在路由处理器或中间件中加入请求。

# 同步和异步编程的区别

同步程序是逐行运行的。

异步程序在不确定的时间内计算结果。

如果异步代码是一个承诺，我们可以在回调中得到结果，或者用`await`赋值。

例如，我们可以写:

```
database.query("SELECT * FROM table", (result) => {
  console.log(result);
});
```

是异步的。当这段代码运行时，它不会阻塞主线程。

当结果准备好时，回调被调用。

同步代码通过编写以下代码来运行:

```
const result = database.query("SELECT * FROM table");
console.log(result);
```

`query`方法返回结果，我们可以在赋值后得到它。

有些任务，如文件系统操作，在不同的进程中运行。

# 使用 socket.io 向特定客户端发送消息

我们可以使用`clients`属性和`send`方法向特定的客户端发送消息。

例如，我们可以写:

```
const io = io.listen(server);
io.clients[clientId].send();
```

我们通过 ID 获取客户端，然后对其调用`send`。

# 用 Express 发送 404 响应

要用 Express 发送 404 响应，我们可以调用`res.status`来发送状态代码。

如果我们想添加一个消息，我们可以在它之后调用`send`。

例如，我们可以写:

```
res.status(404).send('not found');
```

同样，我们可以使用`sendtatus`方法:

```
res.sendStatus(404);
```

# 创建文件夹或使用现有文件夹(如果存在)

为了让我们创建一个文件夹或者使用一个已经存在的文件夹，我们可以使用`mkdirp`包。

要安装它，我们运行:

```
npm install mkdirp
```

然后我们可以写:

```
cpnst mkdirp = require('mkdirp');
mkdirp('/foo', (err) => {
  //...
});
```

`err`遇到错误。

`'/foo'`是目录路径。

在创建目录或目录已经存在后，调用回调。

从节点 10 开始，我们也可以在没有库的情况下做同样的事情。

例如，我们可以写:

```
fs.mkdirSync('/foo', { recursive: true })
```

使用`recursive`选项同步使用或创建文件夹。

此外，我们可以写:

```
await fs.promises.mkdir('/foo', { recursive: true })
```

异步地做同样的事情。

`'/foo'`是小路。

# 如何在 Express 中使用 NODE_ENV

我们可以用`export`命令设置`NODE_ENV`环境变量。

例如，我们可以写:

```
export NODE_ENV=production
```

然后在我们的 Express 应用程序中，我们可以写:

```
app.get('env')
```

获取环境名称。

# 更新特定的节点包

要更新特定的节点包，我们可以使用`npm update`命令。

例如，我们可以运行:

```
npm update express
```

或者我们可以运行:

```
npm install express@4.17.1
```

将 Express 更新到特定版本。

我们还可以通过运行以下命令来更新到最新版本:

```
npm install express@latest
```

![](img/0a3b90e6744cfa84b5834e100ffc2c41.png)

由 [Timothy Muza](https://unsplash.com/@timothymuza?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用`npm update`更新包。

我们可以使用异步代码来运行长期运行的操作。

此外，我们可以向特定的客户端发送消息。

有专为节点应用制作的 HTTP 客户端。