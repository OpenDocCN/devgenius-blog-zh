# 面向对象的 JavaScript —模块

> 原文：<https://blog.devgenius.io/object-oriented-javascript-modules-f657c8dfe697?source=collection_archive---------8----------------------->

![](img/17795544091f94688f2758374633f049.png)

照片由[迈克·多纳](https://unsplash.com/@dorner?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 JavaScript 模块。

# 模块

JavaScript 模块让我们将代码分成多个小块。

JavaScript 中的模块只是普通的 JavaScript 文件。

没有模块关键字。

我们只是使用`export`关键字使模块中的项目对其他模块可用。

例如，我们可以写:

`module.js`

```
export const port = 8080;
export function startServer() {
  //...
}
export class Config {
  //...
}
export function process() {
  //...
}
```

创建一个导出了各种成员的模块。

我们可以导出变量、函数、类等。

然后我们可以通过编写以下内容来导入它:

```
import * from './module'
```

我们从带有星号的模块中导入所有成员。

此外，我们可以通过编写以下内容来导入单个成员:

```
import { Config, process } from "./module";
```

我们从`module`导入`proces`函数和`Config`类。

如果我们只有一个东西要导出，我们可以使用默认的导出语法。

例如，我们可以写:

```
export default class {
  //...
}
```

然后我们在`module.js`中导出类。

然后我们通过写来导入它:

```
import C from "./module";
```

模块有一些特殊的属性。

它们是单例的，这意味着它在另一个模块中只被导入一次。

变量、函数和其他声明对于模块来说是局部的。

只有标有`export`的才可用于`import`。

模块可以导入其他模块。

我们用之前使用的`import`语句来实现。

在示例中，路径是相对的。

但是如果模块在`node_modules`文件夹中，我们可以导入带有模块名的模块。

ES5 支持带有 CommonJS 和异步模块定义(AMD)模块系统的库。

这些不是语言规范的一部分。

它们是第三方系统，因为 ES5 和更早的版本没有本地模块系统而被采用。

CommonJS 是 Node.js 使用的默认模块系统。

# 导出列表

我们可以出口一系列产品。

例如，我们可以写:

```
const port = 8080;function startServer() {
  //...
}
class Config {
  //...
}
function process() {
  //...
}export { port, startServer, Config, process };
```

然后我们导出它上面的所有变量、类和函数。

然后我们可以在另一个模块中用`import`导入它们。

我们用来导入成员的名字可以用关键字`as`来改变。

例如，我们可以写:

```
import { process as processConfig } from "./module";
```

我们用关键字`as`将`process`函数导入到`processConfig`中。

此外，我们可以重命名导出。

例如，我们可以写:

`module.js`

```
const port = 8080;
function startServer() {
  //...
}
class Config {
  //...
}
function process() {
  //...
}export { port, startServer, Config, process as processConfig };
```

然后我们可以通过编写来导入它:

```
import { processConfig } from "./module";
```

我们在导出中使用了`as`来重命名它。

![](img/40060cd95d67fc0c772dd29a827ce516.png)

由 [Alvan Nee](https://unsplash.com/@alvannee?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用模块将代码分成小块。

这样，我们就不必使用全局变量或者编写大量代码。