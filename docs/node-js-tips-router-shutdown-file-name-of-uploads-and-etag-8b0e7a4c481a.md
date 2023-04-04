# Node.js 提示—路由器、关机、上传的文件名和 etag

> 原文：<https://blog.devgenius.io/node-js-tips-router-shutdown-file-name-of-uploads-and-etag-8b0e7a4c481a?source=collection_archive---------10----------------------->

![](img/02de982263653f588fe6f96fc0b9db1b.png)

照片由[埃德森·托雷斯](https://unsplash.com/@edsonfotopet?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 优雅关闭 Express 应用程序的简单方法

为了让优雅地关闭 Express 应用程序更加容易，我们可以使用[@ moebius/http-graceful-shut down](https://github.com/moebius-mlm/http-graceful-shutdown)模块来让关闭连接更加容易。

要安装它，我们运行:

```
npm i -S @moebius/http-graceful-shutdown
```

然后我们可以写:“

```
const express = require('express');
const GracefulShutdownManager = require('@moebius/http-graceful-shutdown').GracefulShutdownManager;const app = express();const server = app.listen(8080);const shutdownManager = new GracefulShutdownManager(server);process.on('SIGTERM', () => {
  shutdownManager.terminate(() => {
    console.log('Server is terminated');
  });
});
```

我们只是在 Express 服务器上使用包的`GracefulShutdownManager`构造函数。

然后我们可以观察`SIGTERM`信号，并对其调用`terminate`方法。

当终止完成时，将调用`terminate`中的回调。

# 错误:大多数中间件(如 json)不再与 Express 捆绑在一起，必须单独安装。

在 Express 4 中，我们必须安装大部分以前单独包含的软件包。

我们必须安装像主体解析器和模板引擎这样的包。

例如，为了安装 body-parser，我们运行:

```
npm i body-parser
```

然后我们可以通过写来使用它:

```
const http = require('http');
const express = require('express');
const bodyParser = require('body-parser');
const mysql = require('mysql');
const ejs = require('ejs');const app = express();app.use(bodyParser.urlencoded({
    extended: true
}));app.use(bodyParser.json());
```

# 用 Multer 存储文件扩展名为的文件

要存储文件扩展名为 Multer 的文件，我们可以在文件名后添加扩展名。

例如，我们可以写:

```
const multer = require('multer');const storage = multer.diskStorage({
  destination(req, file, cb) {
    cb(null, 'uploads/')
  },
  filename(req, file, cb) {
    cb(null, `${Date.now()}.png`)
  }
})const upload = multer({ storage });
```

我们用一个有几个方法的对象来调用`multer.diskStorage`方法。

`destination`方法指定上传文件的文件夹。

在`filename`方法中，我们用文件名和扩展名调用`cb`回调。

我们还可以在`filename`方法中从 MIME 类型中获取扩展。

例如，我们可以写:

```
const multer = require('multer');
import * as mime from 'mime-types';const storage = multer.diskStorage({
  destination(req, file, cb) {
    cb(null, 'uploads/')
  },
  filename(req, file, cb) {
    const ext = mime.extension(file.mimetype);
    cb(null, `${Date.now()}.${ext}`);
  }
})const upload = multer({ storage });
```

我们使用 mime-types 包来帮助我们获取上传文件的 mime 类型，我们将使用它作为扩展名。

`mime.extension`让我们从 MIME 类型中获取扩展。

`file`是文件对象。`file.mimetype`是 MIME-type。

然后，我们将扩展名插入回调中的文件名字符串。

# 在 Express 中设置默认路径(路由前缀)

我们可以使用 Express 的`Router`构造函数来创建一个路由器对象。

这是我们可以向路由添加路由前缀的地方。

例如，我们可以写:

```
const router = express.Router();
router.use('/posts', post);app.use('/api/v1', router);
```

首先，我们通过调用`express.Router()`来创建`router`。

然后我们调用`router.use`来创建一条路径为`posts`的路由。

然后我们调用`app.use`将`router` 作为第二个参数传递给它。

我们将前缀`/api/v1`指定为`app.use`的第一个参数。

此外，我们可以通过编写以下内容将它们链接在一起:

```
app.route('/post')
  .get((req, res) => {
    res.send('get post')
  })
  .post((req, res) => {
    res.send('add post')
  })
  .put((req, res) => {
    res.send('update post')
  });
```

我们有 GET、POST 和 PUT 路由使用的`post`路径。

# 在 Express 中禁用 etag 标题

我们可以通过调用`app.set`禁用带有 Express 的`etag`标题。

例如，我们可以写:

```
app.set('etag', false);
```

关闭`etag`割台。

我们也可以写:

```
app.disable('etag')
```

做同样的事情。

![](img/498367b0f15202959c1c85950a14e0a8.png)

照片由 [Dieny Portinanni](https://unsplash.com/@dienyportinanni?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用一个包优雅地关闭一个 Express 服务器。

此外，我们必须安装以前包含在 Express ourselves 中的大多数中间件。

我们可以从上传的文件中获取扩展名，并将其附加到上传文件的文件名上。

快递的路由器是可以连锁的。

可以禁用`etag`割台。