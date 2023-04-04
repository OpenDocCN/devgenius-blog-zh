# 现代 JavaScript 的精华——错误、字符串和数组

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-errors-strings-and-arrays-8251e8af2d4?source=collection_archive---------6----------------------->

![](img/ec22697465d989fc7d7e76135994b23e.png)

图片由[视觉效果](https://unsplash.com/@visuals?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# 没有更多自定义错误构造函数

在 ES6 或更高版本中，我们不必再编写自己的错误构造函数。

相反，我们只是创建了一个`Error`的子类，这样我们就可以用一致的信息创建错误，比如堆栈跟踪。

例如，不写:

```
function FancyError() {
  var parent = Error.apply(null, arguments);
  copyProperties(this, parent);
}
FancyError.prototype = Object.create(Error.prototype);
FancyError.prototype.constructor = FancyError;function copyProperties(target, source) {
  Object.getOwnPropertyNames(source)
    .forEach(function(propKey) {
      var desc = Object.getOwnPropertyDescriptor(source, propKey);
      Object.defineProperty(target, propKey, desc);
    });
};
```

为了创建`Error`的子构造函数，我们编写:

```
class FancyError extends Error {
}
```

我们没有通过调用带有一些参数的`Error`构造函数来创建`FancyError`构造函数，而是使用`extends`来创建子类。

构造函数没有属性`Error`，所以我们必须像第一个例子一样用`copyProperties`函数复制它们。

我们用`Error`对象获取属性描述符，并调用`Object.defineProperty`来创建属性。

对于第二个例子，我们什么都不用做。

这些属性将被自动继承。

# 对象到地图

在 ES6 或更高版本中，我们可以使用映射来存储键值对。

在 ES6 之前，我们所有的都是普通对象。

在 ES6 或更高版本中，我们可以使用`Map`构造函数通过编写以下内容来创建我们的地图:

```
const map = new Map();
map.set('foo', 'bar');
map.set('count', 1);
```

我们调用`set`方法，将键和值作为第一个和第二个参数来添加或更新它们的键-值对。

# 字符串方法

ES6 提供了我们可以使用的新的字符串方法。

`startsWith`方法让我们检查一个字符串是否以给定的子字符串开始。

例如，我们可以写:

```
str.startsWith('x')
```

同样，字符串有`endsWith`方法让我们检查一个字符串是否以给定的子字符串结尾。

例如，我们可以写:

```
str.endsWith('x')
```

如果以给定的子字符串开始或结束，它们都返回`true`。

`includes`方法让我们检查一个字符串是否包含给定的子字符串。

例如，我们可以写:

```
str.includes('x')
```

我们调用`str`上的`includes`来检查`'x'`是否包含在内。

如果包含，则返回`true`，否则返回`false`。

我们可以使用`repeat`方法来重复一个字符串。

例如，我们可以写:

```
'foo'.repeat(3)
```

重复`'foo'` 3 次。

# 数组方法

ES6 提供了新的数组方法，使得数组操作更加容易。

我们可以使用`findIndex`方法来返回数组条目的索引。

它需要一个回调函数来处理我们正在寻找的条件。

例如，我们可以写:

```
const arr = ['foo', NaN, 'bar'];
arr.findIndex(x => Number.isNaN(x));
```

我们传递了一个回调函数来检查`NaN`是否在`arr`数组中。

这比`indexOf`更有用，因为它不需要回调来检查。

或者 spread 操作符让我们将可迭代对象转换成数组。

在 ES6 之前，我们只有`slice`方法。

而不是写:

```
var arr1 = Array.prototype.slice.call(arguments);
```

在 ES5 或更早的版本中，我们可以写:

```
const args = Array.from(arguments);
```

或者

```
const args = [...arguments];
```

使用 ES6 或更高版本。

![](img/23b13f3a9442ddf050d55337ad9e4525.png)

Photo by [傅甬 华](https://unsplash.com/@hhh13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

ES6 对字符串、数组和错误进行了许多有用的增强。

没有他们很难活下去。