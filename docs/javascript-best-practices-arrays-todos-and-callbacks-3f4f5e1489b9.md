# JavaScript 最佳实践——数组、待办事项和回调

> 原文：<https://blog.devgenius.io/javascript-best-practices-arrays-todos-and-callbacks-3f4f5e1489b9?source=collection_archive---------23----------------------->

![](img/fd8b65d0c9323b77940ceb9ea3a44f80.png)

照片由 [Marius Masalar](https://unsplash.com/@marius?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 向待办事项注释添加过期条件

如果我们必须做注释，我们可以给它添加过期条件，这样 ESLint 就会选择它们，如果它们过期了，就会抛出一个错误。

例如，我们可以写:

```
// TODO [2019-11-15]: fix this
```

我们有 ISO 8601 格式的日期和任务描述。

# 显式比较值的`length`属性

显式比较 value 的`length`属性比不比较清楚。

例如，不写:

```
if (string.length) {}
if (array.length) {}
```

我们写道:

```
if (string.length > 0) {}
if (array.length > 0) {}
```

显式地将它们与 0 进行比较。

# 文件名的大小写样式

文件名的大小写样式应该一致。

例如，我们可以用烤肉串做任何事情:

```
foo-bar.js
```

或者我们可以使用骆驼案例。

例如，我们可以写:

```
fooBar.js
```

或者我们可以用蛇的例子:

```
foo_bar.js
```

或者我们可以用帕斯卡的例子:

```
FooBar.js
```

# 使用`.`导入索引文件

如果我们正在导入一个`index.js`文件，我们不需要显式地写文件名。

例如，不写:

```
const module = require('./index');
```

或者:

```
const module= require('./');
```

我们写道:

```
const m = require('.');
```

或者:

```
const m = require('./foo');
```

# 除了`String`、`Number`和`Boolean`之外，所有内置构造器都使用`new`

几乎所有的构造函数都需要`new`关键字，除了`String`、`Number`和`Boolean`，

我们不应该使用构造函数来创建原始值

相反，我们不用`new`来使用它们:

```
const str = String(123);
```

但是我们写道:

```
const list = new Array(10);
```

这是因为使用带有`String`、`Number`或`Boolean`的`new`创建对象，而不是原始值。

# `Use Array.isArray()`而不是`instanceof Array`

`Array.isArray`比`instanceof Array`更健壮。

`instanceof Array`不能跨领域或跨背景工作。

它不能在浏览器中不同的框架或窗口之间工作，也不能在 Node 中的`vm`模块之间工作。

例如，不写:

```
array instanceof Array;
```

我们写道:

```
Array.isArray(array);
```

# `console.log`参数之间没有前导空格

我们不需要`console.log`参数之间的前导空格。

我们只是把尾随空格。

所以与其写:

```
console.log('abc' , 'def');
```

我们写道:

```
console.log('abc', 'def');
```

# 将函数引用直接传递给迭代器方法

我们应该创建自己的回调，而不是直接传递函数引用。

例如，不写:

```
[1, 2, 3].map(fn);
```

我们写道:

```
[1, 2, 3].map(c => fn(c)));
```

这样，我们可以控制传递给`fn`的参数，而不是总是将数组条目作为第一个参数，将索引作为第二个参数，将数组作为第三个参数。

这也适用于任何其他接受回调的函数调用。

我们希望通过创建自己的函数来控制传入的参数，

# 不要使用可以用`for-of`线圈代替的`for`线圈

如果我们可以使用 for-of 循环来遍历一个对象，那么我们应该这样做。

例如，不写:

```
for (let index = 0; index < arr.length; index++) {
  const element = arr[index];
  console.log(element);
}
```

我们写道:

```
for (const element of array) {
  console.log(element);
}
```

干净多了。

# 使用 Unicode 转义而不是十六进制转义

为了清晰和一致，我们应该使用 Unicode 转义而不是十六进制转义。

例如，不写:

```
const foo = '\x1B';
```

我们写道:

```
const foo = '\u001B';
```

# 没有嵌套的三元表达式

嵌套的三元表达式很难读懂，所以我们不应该有。

例如，不写:

```
const foo = i > 39 ? i < 100 ? true : true: false;
```

我们写道:

```
const foo = (x === 1) ? 'foo' : 'bar';
```

![](img/4910a79f988d18251e666b09967bf783.png)

[大卫·克洛德](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们应该在正则表达式中使用 Unicode 转义而不是十六进制转义。

三元表达式很难读，我们不应该有。

ESLint 可以检查 todo 注释的截止日期。

文件名的大小写应该一致。

我们应该定义自己的回调，而不是传入函数引用。

对于遍历可迭代对象，for-of 循环比 for 循环好得多。