# Node.js 提示—主体解析器错误、静态文件和更新集合

> 原文：<https://blog.devgenius.io/node-js-tips-body-parser-errors-static-files-and-update-collections-ab510c4dce14?source=collection_archive---------14----------------------->

![](img/67579a47a2fca05989cc3eeea552db66.png)

由[马克斯·克莱恩](https://unsplash.com/@hirmin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 注册猫鼬模型

我们可以通过导出它们来注册 Mongoose 模型，然后在我们的 Express 应用程序的入口点中要求它们。

例如，我们可以在`models/User.js`中编写以下内容:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;const userSchema = new Schema({
  name: String
});

mongoose.model('User', userSchema);
```

然后在我们的`app.js`文件中，我们可以写:

```
mongoose.connect("mongodb://localhost/news");
require("./models/User");const users = require('./routes/users');const app = express();
```

要求`./models/User.js`将使它运行代码来注册模型。

# Catch Express 主体解析器错误

我们可以用自己的中间件捕捉明显的`body-parser`错误。

例如，我们可以写:

```
app.use( (error, req, res, next) => {
  if (error instanceof SyntaxError) {
    sendError(res, 'syntax error');
  } else {
    next();
  }
});
```

我们可以检查`error`是否是`SyntaxError`的实例，并对其进行处理。

否则我们调用`next`继续下一个中间件。

# 通过快速应用程序将所有请求发送至 index.html

我们可以通过在一个路由处理器中使用 rth`sendFile`方法将所有请求路由到 Express 应用程序中的`index.html`。

例如，我们可以写:

```
const express = require('express')
const server = express();
const path = require('path');server.use('/public', express.static(path.join(__dirname, '/assets')));server.get('/foo', (req, res) => {
  res.send('ok')
})server.get('/*', (req, res) => {
  res.sendFile(path.join(__dirname, '/index.html'));
})const port = 8000;
server.listen(port)
```

我们将所有请求重定向到`index.html`,最后是一个无所不包的路由。

这样，我们仍然可以有一些路线做别的事情。

`foo`路线响应`ok`。

`express.static`提供静态内容。

# 带有 Node.js 和 Express 的基本 Web 服务器，用于提供 HTML 文件和资源

我们可以使用`express.static`中间件来服务静态资产。

例如，我们可以写:

```
const express = require('express');
const app = express();
const http = require('http');
const path= require('path');
const httpServer = http.Server(app);app.use(express.static(path.join(__dirname, '/assets'));app.get('/', (req, res) => {
  res.sendfile(path.join(__dirname, '/index.html'));
});
app.listen(3000);
```

我们创建一个带有`express.static`中间件的服务器来服务来自`assets`文件夹的静态文件。

然后我们创建一个 GET route 来渲染`index.html`。

我们也可以使用`connect`来做同样的事情:

```
const util = require('util');
const connect = require('connect');
const port = 1234;connect.createServer(connect.static(__dirname)).listen(port);
```

我们调用`createServer`来创建一个 web 服务器。

然后我们使用`connect.static`将当前 app 文件夹作为静态文件夹服务。

# Gzip 快速静态内容

我们可以使用中间件`compression`通过 Gzip 提供静态内容。

例如，我们可以写:

```
const express = require('express');
const compress = require('compression');
const app = express();
app.use(compress());
```

# 在 Node.js 中查找文件的大小

我们可以使用`statSync`方法来获取文件的大小。

例如，我们可以写:

```
const fs = require("fs");
const stats = fs.statSync("foo.txt");
const fileSizeInBytes = stats.size
```

我们调用`statSync`来返回一个包含文件相关数据的对象。

以字节为单位的大小在`size`属性中。

# 自动为每个“呈现”响应添加标题

我们可以使用`res.header`方法为每个响应返回一个头。

例如，我们可以写:

```
app.get('/*', (req, res, next) => {
  res.header('X-XSS-Protection', 0);
  next(); 
});
```

创建一个中间件来设置`X-XSS-Protection`头为 0。

# 用 Mongoose 更新子集合

在回调中更新之后，我们获得更新的集合。

例如，我们可以写:

```
Lists.findById(listId, (err, list) => {
  if (err) {
    ...
  } else {
    list.items.push(task)
    list.save((err, list) => {
      ...
    });
  }
});
```

我们调用`findById`来获取集合。

然后我们在`items`子集合上调用`push`。

最后，我们调用`list.save`保存数据，并在`save`回调的`list`数组中获取最新的列表。

此外，我们可以使用`$push`操作符在回调之外进行推送:

```
Lists.findByIdAndUpdate(listId, {$push: { items: task }}, (err, list) => {
  //...
});
```

我们用`listId`调用`findByIdAndUpdate`来获取列表。

然后我们用我们的`task`对象设置`items`，并使用`$push`来推动它。

然后我们从回调中的`list`参数获取最新的数据。

![](img/4701152c39c3d904958fc1ce3a12c2f6.png)

照片由[威廉·戴格诺特](https://unsplash.com/@williamdaigneault?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以通过运行注册模式的代码来注册 Mongoose 模型。

向子集合添加项的方法不止一种。

我们可以用 Express 或 Connect 提供静态文件。

我们可以捕捉主体解析器错误。