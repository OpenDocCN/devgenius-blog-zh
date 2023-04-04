# TypeScript 最佳实践—非空断言、异步和循环

> 原文：<https://blog.devgenius.io/typescript-best-practices-non-null-assertions-async-and-loops-2bb8e7376b18?source=collection_archive---------11----------------------->

![](img/222876f77ebb58fd242bc3cfeb268bdf.png)

照片由[陈彦蓉](https://unsplash.com/@spdumb2025?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 非空断言

我们可以添加一个非空断言操作符来确保 sone 对象永远不会是`null`或`undefined`。

这是用感叹号表示的。

例如，我们可以写:

```
function bar(foo: Foo | undefined) {
  foo!.doSomething();
}
```

我们有一个`foo`参数，可以是`undefined`。

所以为了确保它不是`undefined`，我们使用`!.`操作符来确保它不是`undefined`。

# 没有神奇的数字

我们应该确保我们的代码中没有幻数。

它们很难从代码中理解。

此外，如果我们必须改变它们，我们必须在多个地方进行改变。

因此，与其写:

```
const metersInFeet = meters * 3.28
```

我们写道:

```
const METER_IN_FEET = 3.28;
const metersInFeet = meters * METER_IN_FEET;
```

我们创建一个常数，并使用它，这样每个人都知道它包含什么。

我们可以在任何地方引用它，所以如果需要，我们只需更改它一次。

# 没有参数分配

分配参数不是一个好主意。

如果它是一个对象，它会改变参数，因为它们是通过引用传递的/

所以与其写:

```
function foo(x: number) {
  x = 2;
}
```

我们写道:

```
function foo(x: number) {
  const y = 2;
}
```

# 没有用于引用其他类型脚本文件的引用语法

在 ES 模块并入 TypeScript 之前，我们使用了`reference`语法来引用其他 TypeScript 文件。

然而，现在我们可以使用 ES6 模块来代替。

例如，不写:

```
/// <reference path='./foo'>
```

我们将我们的 TypeScript 类型转换为模块，并编写:

```
import baz from foo;
```

# 不需要 var

有了 TypeScript，我们可以使用`import`语法来要求 CommonJS 模块。

所以与其写:

```
var module = require("module");
```

相反，我们写道:

```
import module = require('module');
```

我们使用`require`，但是左侧有`import`。

这是语言的一部分，TypeScript 会正确地编译它。

# 箭头功能

每当我们的函数中不需要`this`时，我们就应该使用箭头函数。

它们更短，不会绑定到值`this`。

所以与其写:

```
const foo = function() {
  doWork();
};
```

我们写道:

```
const foo = () => {
  doWork();
};
```

# 使用 for-of 循环

for-of 循环是 javaScript 中最通用的一种循环。

它允许我们遍历任何类型的 JavaScript iterable 对象。

它唯一不能做的就是给我们索引。

所以当不需要索引时，我们可以写:

```
for (const a of arr) {
  const(a);
}
```

# 使用带有承诺的异步

返回承诺的函数应该标记为`async`。

通过这种方式，我们确保他们返回一个带有解析值的承诺。

例如，不写:

```
const foo = () => {
  return mypromise
    .then((val) => {
      return val;
    })
}
```

我们写道:

```
const foo = async () => {
  const val = await myPromise;
  return val;
}
```

他们都返回了一个具有确定值`val`的承诺。

# 向我们的代码中添加数据类型定义

我们应该在代码中添加数据类型定义。

它们可以添加到变量、参数中，并作为返回类型。

这将让我们利用数据类型检查，并使它们更容易阅读。

例如，不写:

```
function subtract(x, y) {
  return x - y;
}
```

我们写道:

```
function subtract(x: number, y: number): number {
  return x - y;
}
```

我们指定了参数的数据类型和返回类型。

# 统一签名

如果我们有两个函数签名重载可以用 union 或 rest 参数合并成一个，我们应该这样做。

例如，如果我们有:

```
function foo(a: number);function foo(a: string) {
  console.log(a);
}
```

我们可以写:

```
function foo(a: number | string) {
  console.log(a);
}
```

相反。

![](img/a4a6d2434e0c7b4f4b7de6a4441267c1.png)

照片由[菲利普·马西亚斯](https://unsplash.com/@philipmmacias?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以添加非空断言来确保某些东西不是`null`或`undefined`。

此外，我们不应该使用神奇的数字。

我们可以用`async`和`await`、for-of 循环和组合签名来清理我们的代码。