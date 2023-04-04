# Node.js 提示—异步函数、从 S3 读取文件和关闭服务器

> 原文：<https://blog.devgenius.io/node-js-tips-async-functions-read-files-from-s3-and-closing-servers-56d9ca08aa8f?source=collection_archive---------14----------------------->

![](img/47af0251a2eb2df26d2ee3b60769c1d4.png)

照片由 [Ayla Verschueren](https://unsplash.com/@moob?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 避免在新的 Promise()构造函数反模式中使用 async/await

我们应该避免在`Promise`构造函数反模式中使用`asyhc`和`await`。

例如，不写:

```
const createPromise = () => {
  return new Promise(async (resolve, reject) => {
    const val = await Promise.resolve(100);
    //..
  });
}
```

我们不需要在承诺中包含一个`async`函数，因为我们不应该在`Promise`构造函数中包含承诺。

`async`函数已经返回承诺，所以我们不需要`Promise`构造函数将代码转换成承诺。

我们只是将`async`函数从`Promise`中移除，并原样使用。

# 使用 Socket.io 更新所有客户端

我们可以通过使用`emit`方法用 socket.io 更新所有客户端。

例如，我们可以写:

```
socket.emit('message', 'hello');
```

我们向所有客户端发出内容为`'hello'`的`message`事件。

我们还可以使用`io.sockets.emit`方法向所有套接字发出。

例如，我们可以写:

```
io.sockets.emit('message', 'hello');
```

我们可以使用`broadcast`属性来做同样的事情。

它向除了发送事件的套接字之外的所有人发送消息:

```
socket.broadcast.emit('message', 'hello');
```

# 从 Node.js 中的 S3 getObject 获得响应

我们可以调用`getObject`方法来获得我们想要的项目，如下所示:

```
const aws = require('aws-sdk');
const s3 = new aws.S3();const getParams = {
  Bucket: 'abc',
  Key: 'abc.txt'
}s3.getObject(getParams, (err, data) => { 
  if (err) {
    return err;
  } const objectData = data.Body.toString('utf-8');
});
```

我们使用`aws.S3`构造函数来创建一个新的客户端。

然后我们设置设置桶和键的参数，也就是文件路径。

接下来，我们调用`getObject`来获取桶中给定路径的文件。

`err`是错误对象。

`data`是文件的内容。

我们用`toString`和正确的编码将它转换成一个字符串。

# 正确关闭 Express 服务器

我们在`http`服务器实例上调用`close`，而不是`app`实例。

例如，我们可以写:

```
app.get('/foo', (req, res) => {
  res.redirect('/');
  setTimeout(() => {
    server.close();
  }, 3000)
});const server = app.listen(3000);
```

我们在`server`上调用`close`，这是`http`服务器实例。

`app.listen`返回`http`服务器实例。

# 使用 Node.js 调用 JSON API

我们可以使用`http`模块的`get`方法向服务器发出 GET 请求。

例如，我们可以写:

```
const url = 'https://api.agify.io/?name=michael';http.get(url, (res) => {
  let body = '';
  res.on('data', (chunk) => {
    body += chunk;
  }); res.on('end', () => {
    const response = JSON.parse(body);
    console.log(response.name);
  });
})
.on('error', (e) => {
  console.log(e);
});
```

我们通过传入回调来调用`get`。

回调监听获取数据的`data`事件。

当流发送完数据后，我们监听`end`事件来解析 JSON。

在`end`回调中，我们用`JSON.parse`解析 JSON。

我们还监听`error`事件来记录任何可能存在的错误。

# 为 MongoDB 中填充的文档选择字段

我们可以调用`populate`方法来填充我们想要的字段并返回它。

例如，我们可以写:

```
Model
.findOne({ _id: '...' })
.populate('age', 'name')
```

我们通过`_id`获取项目，填充`age`并返回`name`。

# 检查函数是否是异步的

我们可以通过使用`Symbol.toStringTag`属性来检查一个函数是否是异步的。

例如，我们可以写:

```
asyncFn[Symbol.toStringTag] === 'AsyncFunction'
```

我们还可以检查它是否是带有`instanceof`的`AsyncFunction`的实例。

例如，我们可以写:

```
const AsyncFunction = (async () => {}).constructor;
const isAsyncFunc = asyncFn instanceof AsyncFunction;
```

我们获得了带有`constructor`属性的异步函数的构造函数。

然后我们使用`instanceof`操作符进行检查。

![](img/e5ce69a1838755239183c5233daa720f.png)

罗伯特·格拉姆纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们不应该将异步函数放在`Promise`构造函数中。

我们可以用`getObject`方法从 S3 桶中读取一个文件。

此外，我们可以用`http`模块发出 HTTP 请求。

我们可以在`app.listen`返回的`http`服务器实例上调用`close`。

有几种方法可以检查一个函数是否是异步的。