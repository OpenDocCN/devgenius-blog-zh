# 可维护的 JavaScript —函数和数组

> 原文：<https://blog.devgenius.io/maintainable-javascript-functions-and-arrays-b1d4a47cb46b?source=collection_archive---------5----------------------->

![](img/5e68c833ded798515429316f92dfbf87.png)

照片由[Louis Hansel @ shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看如何检查函数、数组以及检查对象属性的坏方法来了解创建可维护的 JavaScript 代码的基础。

# 检测功能

函数是 JavaScript 中的引用类型。

它们是从每个函数都是实例的`Function`构造函数中创建的。

有一种不好的方法来检查一个函数。

我们不应该用`instanceof`操作符来检查函数。

例如，我们不应该写:

```
function func() {}console.log(func instanceof Function);
```

检查功能。

这并不好，因为它不能跨帧工作，因为每个帧都有自己的`Function`实例。

相反，我们应该使用`typeof`操作符。

所以我们应该写:

```
function func() {}console.log(typeof func === 'function');
```

`typeof`适用于所有框架。

只有 IE8 或更早的版本对此有限制，所以我们不必担心。

# 检测阵列

数组也是对象，所以我们可以用`instanceof`操作符检查数组。

例如，我们可以写:

```
arr instanceof Array
```

但是就像`instanceof`操作符的其他用途一样，它不能跨帧工作。

道格拉斯·克洛克福特推荐的另一种方法是检查数组方法。

例如，我们可以写:

```
function isArray(value) {
  return typeof value.sort === "function";
}
```

检查`sort`方法是否存在于`value`对象中。

但是任何东西都可以有`sort`方法，所以它可能不是很准确。

另一个建议是对值调用`toString`方法，看看它是否返回`'[object Array]'`。

所以我们可以写:

```
function isArray(value) {
  return Object.prototype.toString.call(value) === "[object Array]";
}
```

这适用于所有帧，只有数组返回这个字符串，所以它可以用来检查数组。

然而，现在有了一个内置的方法来用`Array.isArray`方法检查数组。

例如，我们可以写:

```
function isArray(value) {
  if (typeof Array.isArray === "function") {
    return Array.isArray(value);
  } else {
    return Object.prototype.toString.call(value) === "[object Array]";
  }
}
```

我们通过检查`Array.isArray`是否是函数来检查它是否存在。

然后我们可以调用它来检查它是否是一个数组。

`Array.isArray`适用于所有帧，所以我们可以用它来检查任何地方的数组。

这在 IE9、Firefox 4+、Chrome、Opera 10.5+和 Safari 5+中都有实现。

这意味着我们可以只使用`Array.isArray`而忘记其他的一切。

# 检测属性的坏方法

检测属性是我们可能遇到的另一个棘手的问题。

有几种不好的方法来检查对象的属性。

它们包括:

```
if (object[propertyName]) {
  //...
}

if (object[propertyName] != null) {
  //...
}

if (object[propertyName] != undefined) {
  //...
}
```

它们都检查所有错误的值，所以它们不能很好地满足我们的需要。

Falsy 值包括 0、`null`、`undefined`、`''`(空字符串)m 和`false`。

因此，如果`objec[propertyName]`是这些值中的任何一个，那么这些表达式都将是`true`。

![](img/9baa1c7b19243639a08f9194368ed0ab.png)

照片由 [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用`Array.isArray`来检查数组。

我们不应该检查所有的 falsy 值来检查一个对象属性是否存在。

我们可以用`typeof`操作符来检测函数。