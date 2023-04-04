# 基本节点应用最佳实践

> 原文：<https://blog.devgenius.io/basic-node-apps-best-practices-d4ed4ddb04bd?source=collection_archive---------32----------------------->

![](img/4ebfef05c489b57215bcad6f73710edf.png)

照片由 [Henry Be](https://unsplash.com/@henry_be?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，节点应用程序也必须编写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些节点最佳实践。

# 处理回调中的错误

我们应该在回调中处理错误。

异步代码通常有带`err`参数的回调。

在我们的回访中，我们应该写:

```
const loadData = (error, data) => {
  if (error) {
     console.log(error.stack);
  }
  doSomething();
}
```

而不是像这样忽视它:

```
const loadData = (error, data) => {
  doSomething();
}
```

即使我们记录下来，也比什么都不做好。

# 回调文字

当一个函数被命名为`cb`或`callback`时，那么它应该用`undefined`、`null`或`Error`实例来调用，这样如果有错误就传入一个错误。

例如，我们写道

```
cb(undefined);
```

或者:

```
callback(new Error('error'));
```

而不是传入一个字符串或其他东西。

# 出口中没有分配

我们不应该在一个`exports` 对象中分配任何东西。

如果我们这样做，它就不会像预期的那样工作。

例如，不写:

```
exports = {
  foo: 'bar'
}
```

我们应该写:

```
exports.bar = 1;
```

或者:

```
module.exports.foo = 1
```

或者:

```
module.exports = {}
```

这样，成员实际上是导出的。

# 没有外来的进口

我们应该进口任何我们不需要的东西。

因此，我们应该清除它们。

# 没有额外要求

如果不需要 require 调用，那么我们应该删除它们，就像导入一样。

# 没有遗漏导入

如果我们忘记导入一个模块，那么我们应该修复它。

例如，如果我们有:

```
import typoFile from "./typo-file";
import typoModule from "typo-module"; 
```

相反，我们修复了错别字:

```
import existingFile from "./existing-file";
import existingModule from "existing-module";
```

# 没有遗漏要求

像导入一样，我们应该修复指向不存在的模块的 require 调用。

例如，如果我们有:

```
const foo = require("./foo");
```

如果`foo`不存在，那么我们会得到一个错误。

相反，我们应该写:

```
const existingFile = require("./existing-file");
const existingModule = require("existing-module");
```

# 没有新要求

我们不应该用`require`来形容`new`。把它们结合在一起会让人感到困惑

我们可能会混淆:

```
const bar = new require('foo');
```

使用:

```
const bar = new (require('foo'));
```

因此，我们应该将它赋给一个变量，然后在需要时使用`new`:

```
const Foo = new require('foo');
const foo = new Foo();
```

# 没有路径串联

我们不应该把路径连接在一起。

这是因为不同平台之间的路径分隔符可能不同。

相反，我们应该`path.join`来组合路径。

它可以在所有支持的操作系统中正常工作

因此，与其写:

```
const fullPath = __dirname + "/foo.js";
```

或者:

```
const fullPath = `${__dirname}/foo.js`;
```

我们写道:

```
const fullPath = path.join(__dirname, "foo.js");
```

# 没有进程退出

`process.exit()`是一个危险的调用方法，因为它可以在任何时候停止程序。

这意味着程序在退出前可能没有时间处理错误。

相反，我们可以抛出一个错误。

所以与其写作；

```
if (errorHappened) {
  process.exit(1);
}
```

我们写道:

```
if (errorHappened) {
  throw new Error("error!");
}
```

# 没有未发布的框

如果我们没有为我们的项目发布一个包，我们应该在我们的`package.json`中有`bin`属性。

# 没有不支持的 ES 功能

我们不应该使用不受支持的 ES 特性，因为没有 transpilation 它们就不能运行。

既然 JavaScript 的改进已经被整合到 Node.js 中，在没有官方支持的情况下，我们就没有那么多需要使用的新特性了。

因此，我们应该只使用受支持的那些。

这包括语法和方法。

这些方法可能有库或 polyfill。

语法相当成熟，所以我们不需要使用新的不支持的语法。

# 事情

我们添加了一个 shebang，这样我们就可以运行我们的节点程序，而不需要显式地输入`node`。

例如，我们可以写:

```
#!/usr/bin/env node
```

命令行将选择 shebang 并处理 Node。

![](img/7a19f29996ad9312029f58e8c6c0da6f.png)

照片由[梅里达勒](https://unsplash.com/@meric?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 没有不推荐使用的 API

有许多我们应该避免使用的不推荐使用的 API。

例如，`assert`模块有一些方法，如`deepEqual`、`equal`等。不推荐使用的。

`crypto`也有一些弃用的模块。

因此，我们应该避免使用它们，因为它们将来会被删除。

# 结论

在编写节点应用程序时，我们应该考虑一些最佳实践。

它们非常简单，我们可以快速修复。

像连接路径这样的事情是 Node 的内置特性，我们可以用它来解决这个问题。