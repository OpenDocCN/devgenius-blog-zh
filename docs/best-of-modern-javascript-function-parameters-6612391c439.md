# 现代 JavaScript 的精华—函数参数

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-function-parameters-6612391c439?source=collection_archive---------5----------------------->

![](img/7ea66ce0b8fe2b7c35c63e141b91e601.png)

照片由[利奥波德·坎普](https://unsplash.com/@leopold_k?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# 参数默认值

使用 ES6，我们可以为函数参数添加默认值。

这很好，因为我们不用再担心参数未定义了。

旧的方法是使用`||`操作符返回一个默认值，并将其赋给参数:

```
function foo(x, y) {
  x = x || 0;
  y = y || 0;
}
```

使用默认参数，我们可以编写:

```
function foo(x = 0, y = 0) {
  //···
}
```

它短得多，也干净得多。

只有当参数值为`undefined`时，才会分配默认参数值。

而使用`||`操作符，任何 falsy 值都将返回默认值，这可能不是我们想要的。

# 命名参数

在 ES6 之前，命名参数很难处理。

例如，一种方法是再次使用`||`操作符:

```
function calcVolume(dimensions) {
  var length = dimensions.length || 1;
  var width = dimensions.width || 1;
  var height = dimensions.height || 1;
  //...
}
```

我们从`dimensions`对象中获取每个属性，并将它们赋给变量。

如果属性是 falsy，我们使用`||`操作符返回默认值。

借助 ES6，我们可以更容易地创建命名参数，这要归功于析构语法。

我们可以写:

```
function calcVolume({
  length = 1,
  width = 1,
  height = 1
}) {
  //...
}
```

我们有一个对象参数，它的`length`、`width`和`height`属性被析构。

这样，我们可以命名属性，而不用担心它们被调用的顺序。

我们还为每个属性分配了一个默认值。

# 使参数可选

我们可以在 ES6 之前用`||`操作符使参数可选。

例如，我们可以写:

```
function calcVolume(dimensions) {
  var dimensions = dimensions || {};
  var length = dimensions.length || 1;
  var width = dimensions.width || 1;
  var height = dimensions.height || 1;
  //...
}
```

但是有了 ES6，我们再也不用写这么多行代码了。

我们可以写:

```
function calcVolume({
  length = 1,
  width = 1,
  height = 1
} = {}) {
  //...
}
```

我们刚刚给我们要析构的对象参数赋了一个数组，给对象参数赋了一个默认值。

# `arguments`为休息参数

`arguments`是 ES6 之前从函数调用中获取参数的老方法。

它是一个可迭代的对象，但不是一个数组。

所以我们可以用数组做的大多数事情都是不可用的。

另一方面，rest 参数作为数组返回，所以我们可以在数组中获取参数。

而不是写:

```
function logArgs() {
  for (var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}
```

使用 ES5 或更早的语法，我们编写:

```
function logArgs(...args) {
  for (const arg of args) {
    console.log(arg);
  }
}
```

我们获取`args`数组，并用 for-of 循环遍历它。

![](img/7627e9df665372946f8606443d217e3a.png)

奥其尔-额尔德尼·奥云梅格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

有了默认的参数值和 rest 参数，我们可以更容易地处理函数参数。