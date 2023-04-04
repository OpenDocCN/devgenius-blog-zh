# Node.js 提示—解压缩文件、存储密码和回复

> 原文：<https://blog.devgenius.io/node-js-tips-unzipping-files-storing-passwords-and-repls-85f2f10054a0?source=collection_archive---------12----------------------->

![](img/b10bf14af044f49469481aa1fb8e6c3c.png)

里卡多·莫拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 使用 Node.js 和 MongoDB 存储密码

我们可以用`bcrypt`库将密码存储在 MongoDB 文档中。

比如说。我们可以写:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
const bcrypt = require('bcrypt');
const SALT_WORK_FACTOR = 10;

const UserSchema = new Schema({
  username: { 
    type: String, 
    required: true, 
    index: { 
      unique: true 
    } 
  },
  password: { type: String, required: true }
});

module.exports = mongoose.model('User', UserSchema);
```

我们用`Schema`构造函数创建了`UserSchema`。

它有`username`和`password`字符串字段。

然后我们可以使用`pre`方法在保存密码之前对其进行哈希和加盐处理。

为此，我们写道:

```
UserSchema.pre('save', function(next) {
  const user = this;
  if (!user.isModified('password')){
    return next();
  }

  bcrypt.genSalt(SALT_WORK_FACTOR, (err, salt) => {
    if (err) {
       return next(err);
    }

    bcrypt.hash(user.password, salt, (err, hash) => {
      if (err) {
        return next(err);
      }
      user.password = hash;
      next();
    });
  });
});
```

我们用散列和加盐密码覆盖纯文本密码/

为此，我们使用了`genSalt`方法来创建盐。

然后我们根据密码创建散列。

我们将生成的`salt`传递给了`hashmethod.`

然后我们将`password`属性设置为`hash`。

最后，我们调用`next`继续保存用户数据。

# 不支持修复协议“https:”。预期的“http:”错误

我们可以修复这个错误，它发生在我们试图用`http`发出 HTTPS 请求的时候。

我们必须使用`https.get`而不是`http.get`来请求一个以`https`开头的 URL。

# Mongoose 连接错误回调

我们可以在传递给`connect`的回调中发现错误。

例如，我们可以写:

```
mongoose.connect('mongodb://localhost/db', (err) => {
  if (err) throw err;
});
```

`err`有错误时设置的错误对象。

# 运行一些代码，然后进入节点 REPL

我们可以使用`repl`模块来启动 REPL。

例如，我们可以写:

```
const repl = require("repl");
const r = repl.start("node> ");
```

我们用自己选择的命令提示符调用`start`REPL。

# 在运行时获取 Node.js 版本

我们可以在运行时用`process.version`属性获得节点版本。

要获得更多的版本信息，我们可以使用`process.versions`属性。

然后我们会得到这样的结果:

```
{
  node: '12.16.3',
  v8: '7.8.279.23-node.35',
  uv: '1.34.2',
  zlib: '1.2.11',
  brotli: '1.0.7',
  ares: '1.16.0',
  modules: '72',
  nghttp2: '1.40.0',
  napi: '5',
  llhttp: '2.0.4',
  http_parser: '2.9.3',
  openssl: '1.1.1g',
  cldr: '36.0',
  icu: '65.1',
  tz: '2019c',
  unicode: '12.1'
}
```

# 解压缩 NodeJS 请求的模块 gzip 响应体

我们可以通过使用`zlib`库来解压缩 gzipped 文件。

例如，我们可以写:

```
const http = require("http"),
const zlib = require("zlib");const getGzipped = (url, callback) => {
  const buffer = [];
  http.get(url, (res) => {
    const gunzip = zlib.createGunzip();            
    res.pipe(gunzip);

    gunzip.on('data', (data) => {
      buffer.push(data.toString())
    })
    .on("end", () => {
      callback(null, buffer.join(""));
    })
    .on("error", (e) => {
      callback(e);
    })
  })
  .on('error', (e) => {
    callback(e);
  });
}getGzipped(url, (err, data) => {
   console.log(data);
});
```

我们通过使用`http.get`方法发出获取压缩文件的 GET 请求来创建`getGzipped`函数。

然后调用`createGunzip`来创建`gunzip`对象，我们可以通过管道将`res`流对象传递给它。

然后我们可以监听`data`事件来获取数据。

我们在`buffer`对象上调用`push`来收集数据。

然后当发出`end`事件时，我们调用`callback`将数据发送给回调。

当`error`事件从`gunzip`或`http.get`方法发出时，我们将调用`callback`来发送错误。

然后，我们可以使用该函数来获取错误或解压缩文件的数据。

![](img/027960b3223cff3df86f96e659d804a3.png)

由[Toimetaja tülkebüroo](https://unsplash.com/@toimetaja?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以得到带有`process`对象的版本。

为了解压从`http.get`方法获得的文件，我们可以用`zlib`解压。

如果我们想安全地存储密码，我们必须手动散列和加盐。

我们可以使用`repl`模块访问 REPL。