# 现代 JavaScript 的精华——导入和导出风格

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-import-and-export-styles-7d360b779476?source=collection_archive---------6----------------------->

![](img/dd866fd40a3349abd207e0253f6dd6d1.png)

[奥比·奥尼耶德](https://unsplash.com/@thenewmalcolm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看如何使用 JavaScript 模块。

# 命名导出样式

我们可以导出 JavaScript 模块中的任何内容。

我们可以出口的产品包括:

```
export var foo = "...";
export let bar = "...";
export const MY_CONST = "...";export function qux() {
  //...
}export function* gen() {
  //...
}export class Baz {
  //...
}
```

我们导出`var`、`let`、`const`变量。

我们还可以导出各种函数和类。

我们还可以在文件末尾列出所有导出。

例如，我们可以写:

```
var foo = "...";
let bar = "...";
const MY_CONST = "...";function qux() {
  //...
}function* gen() {
  //...
}class Baz {
  //...
}export { MY_CONST, foo, bar, qux, gen, Baz };
```

我们有一个`export`语句，其中包含我们最后导出的所有项目。

# 再出口

我们可以通过编写以下内容来重新导出模块成员:

```
export { foo, bar } from "./foo";
```

鉴于`foo.js`已经:

```
export var foo = "...";
export let bar = "...";
```

这个语法是从`foo.js`导入所有成员并在一行中导出相同名称的简写。

我们还可以使用`as`关键字来导出成员。

比如，我们可以写；

```
export { foo as qux, bar } from "./foo";
```

将`foo`变量重命名为`qux`。

我们可以像默认导出一样做同样的事情。

例如，我们可以写:

```
export { default } from "./foo";
```

鉴于我们有:

```
export default "...";
```

我们还可以将指定的导出作为默认导出重新导出。

例如，我们可以写:

```
export { baz as default } from "./foo";
```

鉴于我们有:

```
export const baz = "...";
```

在`foo.js`里。

我们从`foo.js`导入了`baz`，这是一个作为默认导出的命名导出。

# 在一个模块中既有命名导出又有默认导出

我们可以在一个模块中同时拥有命名导出和默认导出。

例如，我们可以写:

```
var foo = "...";
let bar = "...";
const MY_CONST = "...";function qux() {
  //...
}function* gen() {
  //...
}class Baz {
  //...
}export { MY_CONST, foo, bar, qux, gen, Baz };export default function quux() {}
```

都在一个模块中。

然后我们可以写:

```
import quux, { MY_CONST, foo, bar, qux, gen, Baz } from "./foo";
```

来导入它们。

这类似于我们在 CommonJS 中遇到的情况。

为了避免混淆，避免混合命名导出和默认导出可能是个好主意。

默认导出只是另一个命名导出。

但是，它的名字是`default`，除非我们用`as`重命名，或者用我们想要的名字导入。

例如，我们可以通过编写以下内容来导入默认导出:

```
import { default as foo } from 'lib';
import foo from 'lib';
```

我们可以使用`default`作为导出名，但不能作为变量名。

![](img/c1509fd322bee419ea065668a881710a.png)

[chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

有很多方法可以用我们的代码导出一些东西。

我们还可以将进口产品全部进行再出口。