# Node.js 技巧——HTTP 代理、Mongoose 模式、读取文件等等

> 原文：<https://blog.devgenius.io/node-js-tips-http-proxies-mongoose-schemas-read-files-and-more-c41aa8b83ddb?source=collection_archive---------14----------------------->

![](img/18544a9c67e17db1928f9220bee331a6.png)

亨特·汤普森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 在 Node.js 中创建简单的 HTTP 代理

要用 Node.js 创建一个简单的 HTTP 代理，我们可以使用`node-http-proxy`库。

我们可以通过运行以下命令来安装它:

```
npm install http-proxy --save
```

然后我们可以写:

```
const http = require('http');
const httpProxy = require('http-proxy');
const proxy = httpProxy.createProxyServer({});

http.createServer((req, res) => {
  proxy.web(req, res, { target: 'http://www.example.com' });
}).listen(3000);
```

我们调用`createProxyServer`来创建代理服务器。

然后我们调用`proxy.web`将我们的请求重定向到`http://example.com`。

# Node.js 中的 fs.createReadStream 与 fs.readFile

我们应该使用`fs.createReadStream`一次读取一个文件的一部分。

这意味着我们可以读取大文件，而无需将所有内容加载到内存中。

`fs.readFile`适合读取整个文件。这意味着它适合阅读小文件。

读流可以接受数据，而不会使主机系统不堪重负。

# 在 Express.js 中获取发出请求的域

我们可以用 Express 获得发起请求的域。

我们所要做的就是使用`req.get('host')`来获取`HOST`头。

这适用于非跨来源请求。

对于跨来源请求，我们可以使用`req.get('origin')`来代替。

同样，我们可以使用`req.headers.host`和`req.headers.origin`来做同样的事情。

如果我们想获得一个客户端的 IP 地址，我们可以使用`req.socket.remoteAddress`。

# 用对象 id 数组创建 Mongoose 模式

要创建一个包含对象 id 数组的 Mongoose 模式，我们只需向数组传递一个构造函数或模式。

例如，我们可以写:

```
const userSchema = mongoose.Schema({
  lists: [listSchema],
  friends: [{ type : ObjectId, ref: 'User' }]
});exports.User = mongoose.model('User', userSchema);
```

我们传入`listSchema`来使用一个现有的模式作为一个带有数组的模式。

`listSchema`是现有的猫鼬模式。

我们也可以像处理`friends`一样传入一个对象。

我们让`friends`引用自身。

# 监听 Node.js 中所有发出的事件

为了监听所有发出的事件，我们可以使用通配符。

我们可以用`eventemitter2`包做到这一点。

它支持命名空间和通配符。

例如，我们可以写:

```
const EventEmitter2 = require('eventemitter2');
const emitter = new EventEmitter2({
  wildcard: false,
  delimiter: '.',
  newListener: false,
  removeListener: false,
  maxListeners: 10,
  verboseMemoryLeak: false,
  ignoreErrors: false
});emitter.on('bar.*', (val) => {
  console.log(this.event, valu);
});
```

我们收听名称以`bar.`开头的所有事件

此外，我们可以监听多个事件和用符号表示标识符的事件。

# 在客户端 JavaScript 中访问 Express.js 局部变量

我们可以在客户端 JavaScript 中访问 Express 局部变量。

我们要做的就是传入一个对象作为`res.render`的第二个参数。

然后我们可以通过键名来访问变量。

例如，我们可以写:

```
res.render('index', {
  title: 'page title',
  urls: JSON.stringify(urls),
});
```

然后我们可以通过书写来访问它们:

```
const urls = !{urls};
```

在我们的 Jad 模板中

我们必须使用 stringy`urls`,因为变量是通过字符串插值添加的。

# 如何检查邮件头是否已经用快递寄出

我们可以检查是否已经用`res.headerSent`属性发送了报头。

例如，我们可以写:

```
if (res.headersSent) { 
  //...
}
```

# 将时间戳转换为人类日期

我们可以通过使用`Date`构造函数将时间戳转换为人类的日期。

例如，我们可以写:

```
const date = new Date(1591399037536);
```

然后我们可以调用`toDateString`、`toTimeString`或`toLocaleDateString`来按照我们喜欢的方式格式化字符串。

# 测量节点应用程序中代码的计时

要测量节点应用程序的计时，我们可以使用`performance.now()`方法。

例如，我们可以写:

```
const {
  performance
} = require('perf_hooks');

console.log(performance.now());
```

我们只是从`perf_hooks`导入`performance`对象，然后在其上调用`now`方法。

![](img/82291449ab14a1cd6a0c312549d1a611.png)

里卡多·莫拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用`performance.now()`方法来衡量绩效。

要创建 HTTP 代理，我们可以使用第三方包来完成。

Mongoose 模式中可以有数组。

我们可以从 Express 的头中获得请求的主机名。

读流适合读大文件，`readFile`适合读小文件。