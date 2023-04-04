# Node.js 提示—使用 exec、移动文件和嵌套模式更改目录

> 原文：<https://blog.devgenius.io/node-js-tips-change-directory-with-exec-move-files-and-nested-schemas-fdae01f8338?source=collection_archive---------2----------------------->

![](img/8deadb5c45762d1afd6589d68d8dbba6.png)

瑞安·斯通在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 将文件移动到 Node.js 中的不同分区或设备

通过指定写入流的目标路径，我们可以将文件移动到不同的分区或设备。

我们将文件读入一个读流，并通过管道将其传输到一个写流。

例如，我们可以写:

```
const fs = require('fs');const rs = fs.createReadStream('source/file');
const ws = fs.createWriteStream('destination/file');rs.pipe(ws);
rs.on('end', () => {
  fs.unlinkSync('source/file');
});
```

我们用源文件路径调用`createReadStream`从流中读取文件。

然后我们调用`createWriteStream`来创建带有目标路径的写流。

我们在读取流上调用`pipe`,将写入流作为参数，将读取流传输到写入流。

这将把文件从源位置复制到目标位置。

一旦复制完成，我们调用`unlinkSync`从源路径中删除文件。

当发出`end`事件时，我们知道复制何时完成。

为了让我们的生活更容易，我们可以使用`fs.extra`模块的`move`方法。

我们通过运行以下命令来安装它:

```
npm install fs.extra
```

然后我们可以通过指定源路径和目的路径来使用`move`方法。

例如，我们可以写:

```
const fs = require('fs.extra');
fs.move('foo.txt', 'bar.txt', (err) => {
  if (err) {
    throw err;
  }
  console.log("success");
});
```

我们用源文件路径作为第一个参数调用了`fs.move`。

第二个参数是目标路径。

最后一个是在移动操作完成时运行的回调。

# 查找弃用警告

当我们运行我们的应用程序时，我们可以通过使用`--trace-deprecation`或`--throw-deprecation`选项找到弃用警告。

例如，我们可以运行:

```
node --trace-deprecation app.js
```

或者:

```
node --throw-deprecation app.js
```

`— trace-deprecation`记录堆栈跟踪，`--throw-deprecation`抛出错误。

我们还应该确保在异步方法中包含一个回调函数，如果我们得到如下结果:

```
(node:4346) DeprecationWarning: Calling an asynchronous function without callback is deprecated.
```

调用没有回调的异步函数是一个很大的不推荐的特性。

例如，我们应该写:

```
const fs = require('fs');
//...fs.writeFile('foo.txt', data, 'utf8', (error) => {
  // ...
});
```

第四个参数是回调函数。

# Node.js exec 不支持“CD”Shell 命令

每个命令都在自己的 shell 中用`exec`运行，所以使用`cd`只会影响运行`cd`的 shell 进程。

如果我们想改变当前的工作目录，我们必须设置`cwd`选项。

例如，我们可以写:

```
exec('git status', {
  cwd: '/repo/folder'
}, (error, stdout, stderr) => {
  if (error) {
    console.error(error);
    return;
  }
  console.log(stdout);
  console.error(stderr);
});
```

我们通过在作为第二个参数传入的对象中设置`cwd`选项来切换到`/repo/folder`目录。

然后我们可以在`/repo/folder`上运行`git status`，而不是应用程序运行的目录。

# 带有 Mongoose 的模式中的模式

我们可以用 Mongoose 在模式中定义一个模式。

这样，我们可以用它存储关系数据。

例如，我们可以写:

```
const UserSchema = new Schema({
  name: String,
  tasks: [{
    type: Schema.ObjectId,
    ref: 'Task'
  }]
});const TaskSchema = new Schema({
  name: String,
  user: {
    type: Schema.ObjectId,
    ref: 'User'
  }
});
```

我们有两个相互引用的模式。

我们用`tasks`数组在`UserSchema`和`TaskSchema`之间创建了一个一对多的关系。

`ref`指定了我们想要访问的模式。

数组表示一对多。

同样，在`TaskSchema`中，我们定义了`user`字段，它创建了与`User`的隶属关系。

我们用`'User’`指定一个对象作为`ref`的值。

然后当我们查询一个用户时，我们可以通过使用`populate`方法获得它的所有任务:

```
User.find({}).populate('tasks').run((err, users) => {
  //...
});
```

我们传入想要访问的子字段，即`tasks`。

![](img/150d629c459477ccd5aa557a26fa636d.png)

西蒙·斯霍尔滕在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用 Mongoose 定义嵌套模式来创建关系。

有几种方法可以移动文件。

我们可以运行带有一些选项的`node`来查找弃用警告。

要用`exec`改变当前工作目录，我们可以设置`cwd`属性。