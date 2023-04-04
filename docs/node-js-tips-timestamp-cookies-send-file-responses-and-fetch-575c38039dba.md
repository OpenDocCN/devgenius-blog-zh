# Node.js 提示—时间戳、Cookies、发送文件响应和获取

> 原文：<https://blog.devgenius.io/node-js-tips-timestamp-cookies-send-file-responses-and-fetch-575c38039dba?source=collection_archive---------18----------------------->

![](img/2cac4a1d20482f51e400f8c5393c9d8a.png)

[解析眼](https://unsplash.com/@parsingeye?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 修复“ReferenceError: fetch 未定义”错误

节点中未实现 Fetch API。

如果我们想使用它，我们可以使用节点获取库。

要安装它，我们运行:

```
npm i node-fetch --save
```

然后我们可以通过写来使用它:

```
const fetch = require("node-fetch");
```

还有`cross-fetch`图书馆。

我们可以通过运行以下命令来安装它:

```
npm install --save cross-fetch
```

然后我们可以写:

```
import fetch from 'cross-fetch';fetch('https//example.com')
  .then(res => {
    if (res.status >= 400) {
      throw new Error("error");
    }
  })
```

# 避免在 Node.js 中长时间嵌套异步函数

我们可以通过将嵌套的异步函数重写为多个函数来重新组织它。

例如，我们可以写:

```
http.createServer((req, res) => {
  getSomeData(client, (someData) => {
    getMoreData(client, function(moreData) => {
         //
      });
   });
});
```

收件人:

```
const moreDataParser = (moreData) => {
   // date parsing logic
};const dataParser = (data) => {
  getMoreData(client, moreDataParser);
};const callback = (req, res) => {  
  getSomeData(client, dataParser);
};http.createServer(callback);
```

我们将匿名函数移到它们自己的命名函数中，这样我们就可以一个一个地调用它们。

# 的使用”。/bin/www "在 Express 4.x 中

`./bin/www`是以快递 app 为入口点的文件。

在启动脚本中，我们有:

```
"scripts": {
  "start": "node ./bin/www",
}
```

启动应用程序。

# 使用 Node.js HTTP Server 获取和设置单个 Cookie

如果我们使用`http`模块创建一个服务器，我们可以通过分割字符串来解析 cookie。

例如，我们可以写:

```
const http = require('http');const parseCookies = (cookieStr) => {
  const list = {};
  const cookies = cookieStr && cookieStr.split(';');
  for (const cookie of cookies) {
    const [key, value] = cookie.split('=');
    list[key.trim()] = decodeURI(value);
  });
  return list;
}http.createServer((request, response) => {
  const cookies = parseCookies(request.headers.cookie);
  //...
}).listen(8888);
```

我们用`request.headers.cookie`得到 cookie 字符串。

然后我们将它传递给`parseCookie`函数。

它用分号分隔 cookie 字符串。

然后我们循环遍历它们，通过`=`符号来分割部分。

左边是键，右边是值。

要设置 cookie，我们可以编写:

```
const http = require('http');http.createServer((request, response) => {
   response.writeHead(200, {
    'Set-Cookie': 'foo=bar',
    'Content-Type': 'text/plain'
  });
  response.end('hello\n');
}).listen(8888);
```

我们使用`writeHead`来设置标题。

`'Set-Cookie'`设置响应 cookie。

`'Content-Type'`设置响应的内容类型。

然后我们用`response.end`返回响应体。

# 使用 Express req 对象获取请求路径

我们可以使用`req.originalUrl`属性来获取 Express `req`对象的请求路径。

# 递归复制文件夹

我们可以用`node-fs-extra`包递归复制一个文件夹。

它的`copy`方法让我们递归地进行复制。

例如，我们可以写:

```
fs.copy('/tmp/oldDir', '/tmp/newDir', (err) => {
  if (err) {
    console.error(err);
  } else {
    console.log("success");
  }
});
```

参数是旧的目录路径。

第二个是新的目录路径。

最后一个参数是复制操作结束时运行的回调。

`err`有错误。

# 用 Moment.js 返回当前时间戳

我们可以用一些方法返回 Moment.js 中的当前时间戳。

我们可以写:

```
const timestamp = moment();
```

或者:

```
const timestamp = moment().format();
```

或者:

```
const timestamp = moment().unix();
```

或者:

```
const timestamp = moment().valueOf();
```

来获取时间戳。

我们也可以将它转换成一个`Date`实例并调用`getTime`:

```
const timestamp = moment().toDate().getTime();
```

# 修复“TypeError:路径必须是绝对路径或指定 res.sendFile 的根目录”错误

如果我们传递一个绝对路径到`res.sendFile`，这个错误可以被修复。

为此，我们写道:

```
res.sendFile('index.html', { root: __dirname });
```

或者:

```
const path = require('path');
res.sendFile(path.join(__dirname, '/index.html'));
```

无论哪种方式，我们都会得到`res.sendFile`所需要的绝对路径。

![](img/4449e21aa475df6760e73daadc0df54c.png)

照片由 [redcharlie](https://unsplash.com/@redcharlie?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

如果我们使用`http`创建我们的服务器，我们必须手动解析 cookies。

节点的标准库不包含获取 API。

`res.sendFile`只走绝对路径。

我们可以通过将嵌套回调重新组织到它们自己的函数中来重写它们。

Moment 用几个函数返回当前时间戳。