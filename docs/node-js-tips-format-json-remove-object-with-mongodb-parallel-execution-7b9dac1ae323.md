# Node.js 技巧—格式化 JSON、使用 MongoDB 移除对象、并行执行等等

> 原文：<https://blog.devgenius.io/node-js-tips-format-json-remove-object-with-mongodb-parallel-execution-7b9dac1ae323?source=collection_archive---------37----------------------->

![](img/703f322c21cecc6de5f73519b89fb00c.png)

由 [Fermin Rodriguez Penelas](https://unsplash.com/@ferminrp?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 在 Node.js 中编写格式化的 JSON

要在节点应用程序中打印格式化的 JSON，我们可以使用带有一些额外参数的`JSON.stringify`方法。

例如，我们可以写:

```
JSON.stringify(obj, null, 2)
```

添加两个缩进空间。

# 用 MongoDB 从数组中删除对象

我们可以通过使用`update`方法用 MongoDB 从数组中移除一个对象。

例如，我们可以写:

```
db.collection.update(
  {'_id': ObjectId("5150a1199fac0e6910000002")}, 
  { $pull: { "items" : { id: 123 } } },
  false,
  true 
);
```

我们传入查询，这是拥有数组的文档的 ID。

然后第二个参数获取要更新的项目，这是我们想要从`items`数组中移除的项目。

`$pull`删除数组中的项目。

第三个参数意味着我们不想做 upsert。

最后一个参数意味着我们要处理多个文档。

# Node.js 中的 console.log 与 console.info

我们使用`console.log`方法来记录一般消息。

`console.info`专门用于记录信息性消息。

然而，除了名字和它们的预期用途之外，它们几乎做同样的事情。

# 复制到 Node.js 中的剪贴板

要将一些内容复制到剪贴板，我们可以运行一个 shell 命令来将一些内容复制到剪贴板。

例如，在 Linux 中，我们可以写:

```
const exec = require('child_process').exec;const getClipboard = (func) => {
  exec('/usr/bin/xclip -o -selection clipboard', (err, stdout, stderr) => {
    if (err || stderr) { 
      throw new Error(stderr);
    }
    func(null, stdout);
  });
};getClipboard((err, text) => {
  if (err) {
    console.error(err);
  }
  console.log(text);
});
```

我们用命令获取剪贴板的内容并输出到标准输出。

此外，我们可以使用`clipboardy`包来读写剪贴板。

例如，我们可以写:

```
const clipboardy = require('clipboardy');
clipboardy.writeSync('copy me');
```

复制到剪贴板。

要从剪贴板粘贴，我们可以写:

```
const clipboardy = require('clipboardy');
clipboardy.readSync();
```

# 协调 Node.js 中的并行执行

为了在 Noe 应用中协调代码的并行执行，我们可以使用`async`库。

例如，我们可以写:

```
const async = require('async');
const fs = require('fs');
const A = (c) => { fs.readFile('file1', c) };
const B = (c) => { fs.readFile('file2', c) };
const C = (result) => {
  // get all files and use them
}async.parallel([A, B], C);
```

我们有 3 个读文件进程，我们将前 2 个放在一起，这样我们可以并行运行它们。

一旦它们都完成了，我们就运行函数`C`。

然后我们用`parallel`方法完全控制哪些并行运行，哪些串行运行。

# 要求和功能

如果我们要调用`require`来导入一个函数。

然后我们必须将该功能分配给`module.exports`。

例如，如果我们有:

`app/routes.js`

```
module.exports = (app, passport) => {
  // ...
}
```

然后我们可以写:

```
require('./app/routes')(app, passport);
```

要调用 import 函数并立即调用它。

# 带发电机的异步/等待和 ES6 产出

`async`和`await`与发电机关系非常密切。

他们只是总能产生承诺的发电机。

它们被编译成带有巴别塔的生成器。

`async`和`await`总是用`yield`。

它用于将产生的值解包为承诺，并将解析后的值传递给异步函数。

我们应该使用`async`和`await`来链接承诺，因为我们可以链接它们，就好像代码是同步的一样。

但是，它只能回报承诺。

`async`和`await`是构建在生成器之上的一个抽象概念，使得使用它们更加容易。

# 从 Node.js HTTP Get 请求中获取数据

我们可以使用`http`模块的`get`方法发出 GET 请求。

例如，我们可以写:

```
const http = require('http');const options = {
  //...
};http.get(options, (response) => {  
  response.setEncoding('utf8')  
  response.on('data', console.log)  
  response.on('error', console.error)  
})
```

`response`是一个读取流，所以我们必须监听`data`事件来获取数据。

我们通过监听`error`事件来获取错误。

![](img/fc9a859db39c960711ddda9c28cd9780.png)

[谢伊·鲁达](https://unsplash.com/@shrouda?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用`http`模块来发出 GET 请求。

要从 MongoDB 文档的数组中删除一个项目，我们可以使用带有`$pull`命令的`update`方法。

`async`模块让我们处理函数的并行和串行执行。

此外，我们可以使用 shell 命令或第三方库来操作剪贴板。

`async`和`await`是生成器之上的抽象。