# Node.js 提示—测试文件上传、应用程序范围的头、缓存

> 原文：<https://blog.devgenius.io/node-js-tips-test-file-uploads-app-wide-headers-caching-749cd5c07dfd?source=collection_archive---------13----------------------->

![](img/daba0284a4ef823ab4ec36902cf69934.png)

克里斯蒂娜·安妮·科斯特洛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 以编程方式停止并重新启动 Express 服务器以更改端口

我们可以通过写入以下命令来停止并重新启动服务器:

```
server.on('close', () => {
  server.listen(3000);
});server.listen(8000, () => {
  server.close();
});
```

我们为服务器关闭时运行的 close 事件添加了一个侦听器。

然后我们用端口调用`listen`。

我们有一个回调函数，它调用触发关闭事件的`close`方法。

然后我们传递给`server.on`的回调将会运行。

# 自动为每个“呈现”响应添加标题

我们可以通过创建自己的中间件来添加响应头，从而为每个`render`响应添加一个头。

例如，我们可以写:

```
app.get('/*', (req, res, next) => {
  res.header('X-XSS-Protection' , 0);
  next();
});
```

我们添加了值为 0 的`X-XSS-Protection`自定义响应头。

我们可以通过编写以下代码使中间件在应用程序范围内运行:

```
app.use((req, res, next) => {
  res.set('X-XSS-Protection', 0);
  next();
});
```

然后我们可以通过运行以下命令安装 Mocha 和 hipper 包来添加一个测试:

```
npm install --save-dev mocha hippie
```

此外，我们可以通过编写以下内容来测试:

```
describe('Response Headers', () => {
  it('responds with header X-XSS-Protection with value 0', (done) => {
    hippie(app)
      .get('/foo')
      .expectHeader('X-XSS-Protection', 0)
      .end(done);
  });
});
```

我们导入前面例子中的`app`对象。

然后我们将它传递给`hippie`并调用一个路由，然后调用`expectHeader`来检查报头是否返回。

然后我们用`done`调用`end`来完成测试。

# 为动态数据快车设置高速缓存控制头

我们可以用自己的值设置`Cache-Control`头。

例如，我们可以写:

```
res.set('Cache-Control', 'public, max-age=86400');
```

它将`Cache-Control`头设置为一天，即 86400 秒。

我们可以通过编写以下代码在中间件中使用它:

```
app.use((req, res, next) => {
  res.set('Cache-Control', 'public, max-age=86400');
  next();
})
```

我们只是把线放进去，调用`next`来调用下一个中间件。

# 相对于父目录的快速服务静态文件夹

我们可以用`path.join`把一个文件夹作为一个相对于目录的静态文件夹。

例如，我们可以写:

```
const path = require('path');
const express = require('express');
const app = express();app.use(express.static(path.join(__dirname, '../public')));
```

我们调用`express.static`将当前文件夹上一级的`public`文件夹设置为服务静态文件的文件夹。

# 在一个路由中发送多个响应块

如果需要分块发送我们的响应，我们可以使用`response.write`方法。

例如，我们可以写:

```
response.write("foo");
response.write("bar");
response.end();
```

我们调用了两次`response.write`来发送响应的不同部分，

然后我们调用`response.end`来完成响应的发送。

# Mocha 中带有文件上传的单元测试

我们可以通过编写以下代码来测试文件上传:

```
const should = require('should');
const supertest = require('supertest');
const request = supertest('localhost:3000');describe('upload', () => {
  it('a file', (done) => {
    request
      .post('/upload')
      .field('foo', 'bar')
      .attach('image', 'pic.jpg')
      .end((err, res) => {
        res.should.have.status(200);
        done();
      });
  });
});
```

我们向`supertest`发出 POST 请求，并调用`attach`以给定的名称附加文件。

`field`有文本字段，我们想提交的文件上传。

然后我们检查回调的响应是 200。

我们可以附加多个字段或文件附件。

# 在 Express 应用程序中监听 socket.io 错误

我们可以通过将`listen`返回的服务器对象传入`socket.io`函数来监听 Express app 中的 socket.io。

例如，我们可以写:

```
const express = require('express');
const app = express();
const server = app.listen(port);
const io = require('socket.io')(server);
```

我们将`server`传递给`socket.io`包返回的函数。

此外，我们可以写:

```
const http = require('http');
const express = require('express');
const app = express();const server = http.createServer(app);
consr io = require('socket.io').listen(server);
server.listen(80);
```

我们将从`http`模块返回的`server`传递给`listen`方法。

![](img/ae89745f38cff556fb28f7b624e54c9b.png)

[乔·史密斯](https://unsplash.com/@joepix?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以使用`res.header`方法设置表头。

有多种方法可以在 Express 应用程序中监听来自 socket.io 的错误。

我们也可以用 Supertest 测试文件上传。