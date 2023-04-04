# Node.js 提示—非模块库、Blobs、setTimeout 等等

> 原文：<https://blog.devgenius.io/node-js-tips-non-module-libraries-blobs-settimeout-and-more-6f9591d2c18e?source=collection_archive---------27----------------------->

![](img/a44fd085ced224f62c7e77696f6275ce.png)

照片由[ярославалексеенко](https://unsplash.com/@webaliser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# setTimeout 和 Node.js

`setTimeout`运行前等待最少毫秒数。

这不是一个保证。

传递 0，非负数将导致它在节点中等待最少毫秒数。

例如，如果我们有:

```
setTimeout(() => {
  console.log('hello');
}, 100)
```

然后，在运行回调之前，我们至少等待 100ms。

# 文件和文件夹的 Node.js 项目命名约定

一个典型的节点项目有一个用于脚本、助手和二进制文件的`/bin`文件夹。

`/lib`为 app。

`/config`为配置。

`/public`为公共文件。

以及用于测试的`/test`。

# 包括 Node.js 中另一个文件的 JavaScript 类定义

我们可以导出该类，并将其导入到另一个文件中。

例如，我们可以写:

`person.js`

```
class Person {
  //...
}module.exports = Person;
```

然后在另一个文件中，我们写:

`app.js`

```
const Person = require('./person');
const person = new Person();
```

我们使用`require`来请求`person`模块，然后实例化我们导入的类。

同样，我们可以对 ES 模块做同样的事情。

例如，我们可以写:

`person.js`

```
export default class Person {}
```

`app.js`

```
import Person from './person';
const person = new Person();
```

我们导入`person`模块，并以同样的方式使用它。

导出必须是一个`default`导出，这样我们就可以导入没有花括号的类。

# 运行节点脚本时，更改当前 Shell 上下文中的工作目录

要更改节点脚本中的工作目录，我们可以使用`process.chdir`方法。

例如，我们可以写:

```
const process = require('process');
process.chdir('../');
```

向上移动一个目录。

# 将普通 Javascript 库加载到 Node.js 中

如果我们想加载不是模块的 JavaScript 库，我们可以使用`vm`模块。

例如，我们可以写:

```
const vm = require("vm");
const fs = require("fs");
module.exports = (path, context) => {
  context = context || {};
  const data = fs.readFileSync(path);
  vm.runInNewContext(data, context, path);
  return context;
}
```

我们用要导出的函数读入脚本文件，并用`vm.runInNewContext`运行它。

`data`有脚本文件。`path`是文件路径。

`context`是脚本运行的上下文，是全局对象。

然后我们可以使用`execfile`来运行文件:

```
const execfile = require("execfile");
const context = execfile("example.js", { globalVar: 42 });
console.log(context.foo());
context.globalVar = 100;
```

`example.js`是一个包含以下内容的文件:

```
function foo() {
  return 'bar';
}
```

我们应该从控制台日志中获取`'bar'`，因为`foo`是一个全局函数。

`globalVar`是一个全局变量。

# Node.js 中的 ES6 变量导入名称

我们可以用 import by name 来写:

```
const import = async () => {
  try {
    const module = await import('./path/module');
  }  catch (error) {
    console.error('import failed');
  }
}
```

我们使用返回承诺的`import`函数。

它可以用来通过我们传入的路径字符串导入 ES 模块。

如果成功，它将解析到模块。

# 用 JavaScript 将 Blob 转换成文件

我们可以用几种方法在 JavaScript 中将 blob 转换成文件。

我们可以将 blob 传递给`File`构造函数。

例如，我们可以写:

```
const file = new File([blob], "name");
```

其中`blob`是 blob 实例，`'name'`是文件名。

我们可以传入更多的参数:

```
const file = new File([blob], "name", { lastModified: 1534584790000 });
```

我们用第三个参数中的时间戳设置`lastModfiied`日期。

我们还可以设置 blob 的`lastModifiedDate`属性。

例如，我们可以写:

```
const blobToFile = (blob, fileName) => {
  blob.lastModifiedDate = new Date();
  blob.name = fileName;
  return blob;
}
```

我们在`blob`上设置了`lastModifiedDate`，这是一个`Blob`实例，用于设置最后修改日期，

`blob.name`设置文件名。

![](img/da4f4000ee501cb5b128baa4c782b879.png)

Jacques Bopp 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们以各种方式设置 blob 或文件的修改日期。

`setTimeout`不保证延迟时间。

我们可以用模块导出类。

在节点应用中加载非模块 JavaScript 库需要一些努力。