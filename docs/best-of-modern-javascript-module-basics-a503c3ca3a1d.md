# 现代 JavaScript 精华—模块基础

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-module-basics-a503c3ca3a1d?source=collection_archive---------8----------------------->

![](img/b451d6640720bf36abaa154601a0d60f.png)

[大卫·克拉克](https://unsplash.com/@thethinblackframe?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看如何使用 JavaScript 模块。

# ES6 之前

ES5 或更早的版本没有原生模块系统。

因此，出现了各种模块系统来解决组织代码的问题。

有 CommonHS 模块系统，它是标准的 ib Node.js。

它有一个紧凑的语法，可以同步加载模块，并且可以在服务器端使用。

异步模块定义模块系统是另一种流行的移动系统。

它有更复杂的语法，这让它们无需`eval`或编译步骤就能工作。

# ES6 模块

ES6 模块创建了一个符合 JavaScript 标准的正式模块系统。

它有一个紧凑的语法，并让我们做单一的出口。

此外，它还支持循环依赖。

直接支持异步加载，并且加载是可配置的。

该语法甚至比 ES6 模块语法更紧凑。

并且它支持循环依赖。

这个比 CommonJS 好。

标准模块系统具有用于导入和导出的声明性语法。

它有一个可编程的加载器 API 来配置如何加载模块和有条件地加载模块。

# 指定出口

使用命名导出，我们可以导出一个模块的多个成员。

例如，我们可以写:

`math.js`

```
export const sqrt = Math.sqrt;
export function add(x, y) {
  return x + y;
}
export function subtract(x, y) {
  return x - y;
}
```

创建一个模块，它有几个用`export`关键字导出的函数。

然后，我们可以通过编写以下内容来导入项目:

```
import { add, subtract } from "./math";const sum = add(1, 2);
const difference = subtract(1, 2);
```

我们从`math.js`模块导入项目。

命名的导出在花括号中。

然后我们可以调用我们在它下面导出的函数。

对于 CommonJS，我们使用`module.exports`属性来导出一个模块的多个成员。

例如，我们可以写:

`math.js`

```
const sqrt = Math.sqrt;
function add(x, y) {
  return x + y;
}
function subtract(x, y) {
  return x - y;
}module.exports = {
  sqrt,
  add,
  subtract
};
```

`index.js`

```
const { add, subtract } = require("./math");const sum = add(1, 2);
const difference = subtract(1, 2);
```

我们调用`require`来请求整个模块，然后我们从导入的模块中析构条目。

然后我们可以用同样的方式使用导入的函数。

# 默认导出

默认导出是一种在任何模块中只能发生一次的导出。

当我们导入默认导出时，我们可以给它们起任何名字。

例如，我们可以写:

`math.js`

```
export default function add(x, y) {
  return x + y;
}
```

将`add`功能导出为默认导出。

然后我们可以通过编写以下内容来导入函数:

`index.js`

```
import add from "./math";const sum = add(1, 2);
```

要导出一个类，我们可以写:

`Foo.js`

```
export default class {}
```

语句后面不需要分号。

然后，我们可以使用以下命令导入它:

```
import Foo from "./Foo";const foo = new Foo();
```

我们可以包含或排除默认导出的名称。

所以我们可以写:

```
export default function baz() {}
export default class Bar {}
```

或者

```
export default function() {}
export default class {}
```

![](img/6faf2b8a46d166a38cc9dce42ab26b04.png)

由 [timJ](https://unsplash.com/@the_roaming_platypus?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

在 ES6 之前，没有标准的模块系统。

从那时起，JavaScript 有了一个本地移动系统，我们可以用它来以标准的方式组织我们的代码。