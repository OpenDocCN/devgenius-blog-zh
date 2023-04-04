# Node.js 提示—表达错误、URL、参数和编写文件

> 原文：<https://blog.devgenius.io/node-js-tips-express-errors-urls-parameters-and-writing-files-f75657b8e08e?source=collection_archive---------23----------------------->

![](img/0e524ed1fc644317dcf6c15a72c05525.png)

照片由 [Xenia Bogarova](https://unsplash.com/@xeniabogarova?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 找不到页面时出现渲染错误

我们可以用`res.render`呈现一个错误消息。

例如，我们可以写:

```
app.use((req, res, next) => {
  res.render('404', { status: 404, url: req.url });
});
```

来显示 404 错误。

`'404'`是模板名。

这个对象包含了我们想要在`404`模板上显示的内容。

我们也可以通过编写来制作一个通用的错误处理中间件；

```
app.use((err, req, res, next) => {
  res.render('500', {
    status: err.status || 500,
    error: err
  });
});
```

我们将`err`作为第一个参数。

我们可以用`err.status`得到错误状态代码，然后我们可以将对象传递给`500`页面模板。

# 在 Node.js 中写入文件

我们可以用 Node.js 和`writeFile`、`writeFileSync`或者一个写流来写文件。

例如，我们可以写:

```
const fs = require('fs');fs.writeFile("/tmp/foo.txt", "hello world", (err) => {
  if (err) {
    return console.log(err);
  }
  console.log("The file was saved!");
});
```

或者我们可以写:

```
const fs = require('fs');fs.writeFileSync('/tmp/foo.txt', 'hello world');
```

它们都将文件路径和内容作为前两个参数。

`writeFile`也接受一个用`err`调用的回调(如果存在的话)。

要创建写流，我们可以写:

```
const fs = require('fs');
const  stream = fs.createWriteStream("/tmp/foo.txt");
stream.once('open', (fd) => {
  stream.write("hello\n");
  stream.write("world\n");
  stream.end();
});
```

我们用要写入的文件路径调用`createWriteStream` to。

然后我们用`stream.once`打开流。

然后我们从`stream.write`开始写。

我们必须用`stream.end`关闭流。

# 在 Express 中获取查询字符串变量

我们可以用`req.query`属性获得查询字符串变量。

它将查询字符串解析成键值对的对象。

例如，我们可以写:

```
const express = require('express');
const app = express();app.get('/', (req, res) => {
  res.send(req.query.id);
});app.listen(3000);
```

如果我们有一个类似于`?id=10`的查询字符串，那么`req.query.id`就是 10。

# 在 Express 应用程序中检索发布查询参数

我们可以用`body-parser`包获得 POST 查询参数。

要使用它，我们必须通过运行以下命令来安装它:

```
npm install --save body-parser
```

然后我们可以写:

```
const bodyParser = require('body-parser');
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ 
  extended: true
}));
```

我们导入模块，以便我们可以使用它。

然后我们调用`bodyParser.json()`，它返回一个中间件来使用这个中间件。

`urlencoded`解析 URL 编码体。

`extended`设置为`true`意味着我们可以用它解析 URL 编码字符串中的 JSON 主体。

# 将 CORS 标题添加到选项路线

我们可以用自己的中间件轻松完成所有的连线工作。

例如，我们可以写:

```
const allowCrossDomain = (req, res, next) => {
  res.header('Access-Control-Allow-Origin', 'example.com');
  res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,DELETE');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  next();
}
```

`Access-Control-Allow-Origin`头让浏览器知道哪些域可以访问 API。

`Access-Control-Allow-Methods`告诉浏览器允许调用哪种请求。

`Access-Control-Allow-Headers`告诉浏览器可以发送什么样的内容。

更简单的方法是使用`cors`中间件。

要使用它，我们可以运行:

```
npm install cors --save
```

来安装它。

然后我们可以写:

```
const cors = require('cors');
const express = require('express');
const app = express();
app.use(cors());
app.options('*', cors());
```

我们在整个应用程序上使用`cors`中间件:

```
app.use(cors());
```

我们允许使用以下方式调用选项路线:

```
app.options('*', cors());
```

# 如何在 Express 中获得完整的 URL

我们可以使用`req.protocol`属性来获取协议，也就是`http`或`https`部分。

同样，我们可以用`req.get('host')`得到主机名。

我们得到了带有`req.originalUrl`的完整 URL。

为了解析合并成完整的 URL，我们可以使用`url`模块。

例如，我们可以写:

```
const requrl = url.format({
  protocol: req.protocol,
  host: req.get('host'),
  pathname: req.originalUrl,
});
```

我们可以在路由处理器内部或任何其他地方这样做。

![](img/a6972ff67062143d7f6abf10d738830e.png)

照片由 [Jonatan Pie](https://unsplash.com/@r3dmax?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以通过传入一个模板来呈现一个错误页面，并获得我们想要显示的状态或错误。

`url`模块可以将 URL 部分组合在一起。

写文件有很多种方法。

CORS 可以通过设置一些响应头或使用带有 Express 的`cors`中间件来完成。