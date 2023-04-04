# Node.js 提示—承诺、Socket.io 和在 Express 中传递数据

> 原文：<https://blog.devgenius.io/node-js-tips-promises-socket-io-and-passing-data-in-express-db98d4b14105?source=collection_archive---------9----------------------->

![](img/a4675deb089d632c0d0b58b726a6c7c7.png)

戴夫·弗朗西斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 在 NodeJS 模块中共享常量

我们可以通过导出一个冻结的对象来共享节点模块中的常量。

例如，我们可以写:

`constants.js`

```
module.exports = Object.freeze({
  FOO: 'foo',
  BAR: '123'
});
```

我们调用`Object.freeze`方法来冻结带有常量的对象。

然后在另一个模块中，我们可以通过编写来要求:

```
const constants = require('./constants');
```

# 使用 Socket.io 向除发送方以外的所有客户端发送响应

有多种方法可以用 Socket.io 向除发送方以外的所有客户端发送响应。

哟只发给发件人，我们可以写:

```
socket.emit('message', "hello");
```

要发送给除发送者之外的所有客户端，我们可以写:

```
socket.broadcast.emit('message', "hello");
```

要发送给房间中除发送者之外的所有客户端，我们可以写:

```
socket.broadcast.to('chatRoom').emit('message', 'hello');
```

要发送给包括发件人在内的所有客户，我们写:

```
io.emit('message', "hello");
```

要发送给房间中的所有客户，包括发送者，我们可以写:

```
io.in('chatRoom').emit('message', 'hello');
```

要发送给只在一个房间里的发送者，我们可以写:

```
socket.to('chatRoom').emit('message', 'hello');
```

要发送给名称空间中的所有客户端，我们可以编写:

```
io.of('namespace').emit('message', 'hello');
```

要发送一个套接字 ID，我们可以写:

```
socket.broadcast.to(socketId).emit('message', 'hello');
```

# 使用像 Q 或蓝鸟这样的 Promise 库的理由

我们可以使用像 Q 或 Bluebird 这样的 promise 库将现有的异步函数转换为返回 promise。

例如，我们可以写:

```
const Promise = require('bluebird');
const fs = Promise.promisifyAll(require('fs'));fs.readFileAsync('foo.text').then((data) => {
   //...
});
```

我们使用蓝鸟将`fs`模块中的一切转换成承诺。

所以我们可以使用`readFileAsync`这样的方法来异步读取文件。

还有一些方法是蓝鸟独有的，ES6 `Promise`构造函数没有这些方法。

`Promise.promisify()`将节点回调转换为承诺。

将数组映射到承诺。

`Promise.reduce()`让我们将值映射到承诺，然后依次调用它们，并将解析后的值缩减为一个值。

`Promise.mapSeries()`获取值，将它们映射到承诺，并连续运行它们。

最后我们得到它们的值。

让我们在给定的毫秒数内完成一个承诺。

# Node.js 全局变量

我们可以给`global`添加一个属性来添加一个全局变量。

例如，我们可以写:

```
global._ = require('underscore');
```

# Node.js 获取文件扩展名

我们可以用`extname`方法得到一个文件名的文件扩展名。

例如，我们可以写:

```
const path = require('path');

path.extname('index.html');
```

# 从命令行运行脚本中的函数

我们可以用`node -e`从命令行运行脚本中的函数。

如果我们有一个模块，我们可以写:

`hi.js`

```
module.exports.hello = () => {
  console.log('hello');
};
```

例如，我们可以写:

```
node -e 'require("./hi").hello()'
```

# Express.js 中 res.send 和 res.json 的区别

当数组或对象被传入时，`res.send`和`res.json`是相同的。

然而，如果传入的是非对象，那么`res.json`也会将这些值转换成可以返回的值。

`res.json`最后调用`res.send`。

# 使用 Express.js 中的 Next()将变量传递给下一个中间件

我们向`req`变量添加一个属性，将数据传递给下一个中间件。

例如，我们可以写:

```
req.foo = 'bar';
```

然后我们可以在下一个中间件中访问`req.foo`。

同样，我们可以向`res.locals`属性添加一个属性来做同样的事情。

# 使用 Node.js 加密创建 HMAC-SHA1 散列

我们可以使用节点加密模块来创建 HMAC-SHA1 散列。

例如，我们可以写:

```
const crypto = require('crypto')

const text = 'hello world';
const key = 'secret';

crypto.createHmac('sha1', key)
  .update(text)
  .digest('hex')
```

我们调用`createHmac`方法从`key`创建一个散列。

然后我们用`update`把`text`和它散列，用`update`把它转换成十六进制。

![](img/7aef631ca32d901ed5d3144a9539a7a2.png)

照片由[朱迪思·普林斯](https://unsplash.com/@judithprins?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以使用`crypto`模块来散列文本。

用 socket.io 发送数据有很多种方法。

我们可以用`path`模块得到一个文件的扩展名。

Promise 库还是有用的。

`global`是节点中的全局对象。