# 更多 Node.js 应用程序最佳实践

> 原文：<https://blog.devgenius.io/more-node-js-app-best-practices-a68f3f978a78?source=collection_archive---------37----------------------->

![](img/322ade6f99e0503e7307b62bb891b02d.png)

Lukasz Szmigiel 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，节点应用程序也必须编写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些节点最佳实践。

# 没有不推荐使用的 API

有许多我们应该避免使用的不推荐使用的 API。

例如，`assert`模块有一些方法，如`deepEqual`、`equal`等。不推荐使用的。

`crypto`也有一些弃用的模块。

`fs`也有一些不赞成使用的方法。

还有很多没有在这里列出。

因此，我们应该避免使用它们，因为它们将来会被删除。

# 回调返回

每当回调在主函数体之外被触发时，回调都应该有一个`return`语句。

否则，我们可能会多次完成某个动作。

例如，我们应该写:

```
const doWork = (err, callback) => {
  if (err) {
    return callback(err);
  }
  callback();
}
```

然后当`err`被定义时，我们停止运行函数。

现在最后一行只在`err`未定义时运行。

# 导出样式

我们应该一致地使用`module.exports`或`exports`。

这样，我们就不会困惑了。

例如，我们应该坚持:

```
module.exports = {
  foo: 'bar'
}
```

或者:

```
exports.bar = 'baz';
```

很容易混淆这两者，因为它们的行为不同，我们很容易出错。

# 导入中的文件扩展名

当我们导入一个模块时，我们不需要添加文件扩展名。

因此，与其写:

```
import foo from "./path/to/a/file.js" 
export * from "./path/to/a/file.js"
```

我们写道:

```
import foo from "./path/to/a/file" 
export * from "./path/to/a/file"
```

它将解析为一个`.js`或`.json`文件。

# 全局需求

我们应该在顶级模块范围内进行`require`调用。

这样，我们可以在模块的任何地方使用它们。

所以与其写:

```
if (condition) {
  const fs = require("fs");
}
```

我们写道:

```
const fs = require("fs");
```

ES6 模块必须在顶部导入，所以我们不会有这个问题。

# 无混合要求

我们应该在同一行中有多个`require`。

将`require`和赋值放在不同的行会更清楚。

例如，不写:

```
const fs = require('fs'),        
    async = require('async'), 
    foo = require('./foo'),   
    bar = require(name()),  
    baz = 42,                  
    bam;
```

我们在同一行中既有 require 语句又有变量赋值。

这对于一些人来说肯定是困惑的。

相反，我们应该写:

```
const fs = require('fs');
const async = require('async');
const foo = require('./foo');
const bar = require(name()); 
const baz = 42;
let bam;
```

# 过程环境

我们可能希望在整个代码中停止使用`proecess.env`。

相反，我们把环境变量都放在一个地方，然后引用它。

例如，我们可以写:

```
const config = require("./config");if(config.env === "development") {
    //...
}
```

从文件中导入`config`，这样我们就不必使用`process.env`。

然而，一些像`dotenv` 这样的图书馆可能会使用`process.env`，所以这个规则可能不适用。

# 限制进口

我们可能想要限制不想让人们使用的模块的进口。

这样，我们只能使用允许的库。

我们可以用 eslint-plugin-node 插件做到这一点。

# 限制要求

像导入一样，我们可以用 eslint-plugin-node 插件来限制需求。

# 不使用同步功能

如果我们正在编写应用程序，那么我们不应该使用同步函数。

这是因为一个同步操作会阻止程序的其余部分运行。

那么我们的应用程序的性能就会受到影响。

对于脚本和命令行实用程序来说，使用同步函数是可以的。

但是对于其他任何东西，我们都不应该使用它们。

例如，如果我们正在编写一个 web 应用程序，那么我们不应该使用像`fs.existsSync()`这样的方法。

# 使用全局缓冲或需要缓冲模块

我们要么使用`Buffer`全局变量，要么始终需要`buffer`模块。

所以我们要么处处用`Buffer`，要么处处用`require(“buffer”).Buffer`。

# 使用全局控制台对象或需要控制台模块

节点有`console`模块和`console`全局对象。

我们可以坚持其中任何一个，并坚持它的一致性。

例如，我们要么写:

```
console
```

或者:

```
const console = require("console");
```

# 使用全局流程对象或需要流程模块

像`console`一样，`process`也作为一个全局对象或模块出现。

例如，我们可以写:

```
process
```

或者:

```
const process = require("process");
```

![](img/5297be7f76fd0a35f07898aee37ed284.png)

照片由 [Marita Kavelashvili](https://unsplash.com/@maritafox?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们应该在我们的节点应用程序中坚持一种做事方式。

此外，同步方法只适用于脚本和命令行程序。