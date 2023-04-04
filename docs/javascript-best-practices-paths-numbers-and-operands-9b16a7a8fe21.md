# JavaScript 最佳实践—路径、数字和操作数

> 原文：<https://blog.devgenius.io/javascript-best-practices-paths-numbers-and-operands-9b16a7a8fe21?source=collection_archive---------7----------------------->

![](img/f724d8159d059ab7943278e1d1a54548.png)

照片由[自由使用声音](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral)上的 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 在`in`表达式中不否定左操作数

忘记否定整个`in`表达式是错误的。

对左操作数求反不会对其他操作数求反。

例如，不要写:

```
if (!key in object) {
  //...
}
```

我们写道:

```
if (!(key in object)) {
  //...
}
```

来否定整个表达式。

# 没有嵌套的三元表达式

嵌套的三元表达式很难阅读，所以我们不应该使用它们/

例如，我们不应该写:

```
const foo = bar ? baz : qux === quxx ? abc : baz;
```

我们应该只写`if`语句来代替它:

```
if (foo) {
  thing = bar;
} else if (baz === qux) {
  thing = abc;
} else {
  thing = baz;
}
```

# 没有使用新的副作用

我们不应该用`new`产生副作用。

所以我们不应该写:

```
new Animal();
```

相反，我们写道:

```
const animal = new Animal();
```

我们将返回值赋给一个变量，这样我们就可以使用它了。

# 没有函数构造函数

`Function`构造函数接受字符串参数来创建函数。

因此很难优化代码，我们不想让任何人向它传递字符串。

而不是写:

```
const add = new Function("a", "b", "return a + b");
```

我们写道:

```
const x = (a, b) => {
  return a + b;
};
```

# `No Object`构造函数

不应该使用构造函数。

它们没有带来任何好处，而且写起来比较长。

例如，不写:

```
const obj = new Object();
```

我们写道:

```
const obj = {};
```

# 没有新要求

我们不应该一起使用`new`和`require`。

相反，我们将它们分开，以消除以下两者之间的混淆:

```
const foo = new require('foo');
```

和

```
const foo= new require('foo');
```

所以我们不写这些，而是写:

```
const Foo = require('foo');
const foo = new Foo();
```

# 没有符号构造函数

`Symbol`函数不是一个构造函数。

我们直接称之为创造一个符号。

所以与其写:

```
const foo = new Symbol("foo");
```

我们写道:

```
const foo = ymbol("foo");
```

# 没有原始包装实例

我们不应该使用原始包装器来创建原始值。

它们有对象类型，而不是通常的基本类型。

使用它们没有任何好处。

如果原始值需要包装到对象中，那么它会暂时自动完成。

所以与其写:

```
const stringObject = new String("hello");
```

我们写道:

```
const str = "hello";
```

# 不将全局对象属性作为函数调用

我们不应该把全局对象属性称为函数，因为它们不是函数。

例如，我们不应该有这样的代码:

```
const math = Math();const newMath = new Math();const json = JSON();const newJSON = new JSON();const reflect = Reflect();
```

相反，使用它们的属性:

```
const obj = JSON.parse("{}");const value = Reflect.get({ x: 1, y: 2 }, "x");const pi =  Math.PI;
```

# 没有八进制文字

我们不应该有以零开头的数字。

它们被视为八进制文字。

例如，不写:

```
const num = 071; 
```

实际上是十进制的 57。

所以我们应该写:

```
const num = 71;
```

相反。

# 字符串中没有八进制转义序列

ES5 不赞成字符串中的八进制转义序列，所以不应该使用它们。

所以我们不应该写:

```
const foo = "hello \251";
```

相反，我们写道:

```
const foo = "hello \xA9";
```

# 没有重新分配函数参数

我们不应该给函数参数重新赋值。

它们改变了`argumenrts`对象，如果它们是对象的话，还改变了变量本身。

所以与其写作；

```
function foo(bar) {
  bar = 100;
}
```

我们写道:

```
function foo(bar) {
  let baz = 13;
}
```

# 使用`__dirname`和`__filename`时没有字符串连接

我们不应该将这些表达式与路径的其余部分连接起来，因为它们不能在所有平台上正常工作。

所以与其写:

```
const fullPath = __dirname + "/foo.js";const filePath = __filename + "/foo.js";
```

我们写道:

```
const fullPath = path.join(__dirname, "foo.js");const fullPath = path.join(__filename, "foo.js");
```

![](img/38bb15d671d57db81fe4211a2fef865c.png)

照片由[奥拉夫·特维特](https://unsplash.com/@olav_tvedt?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们不应该把路径连接在一起。

相反，我们使用`path`模块方法来实现。

嵌套的三元表达式很难读懂。

应删除不推荐使用的表达式。

永远不要使用八进制文字