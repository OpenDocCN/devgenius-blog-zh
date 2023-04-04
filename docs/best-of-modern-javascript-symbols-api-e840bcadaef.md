# 现代 JavaScript 的精华——符号 API

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-symbols-api-e840bcadaef?source=collection_archive---------4----------------------->

![](img/9116b8a558736b4bad20ac4aa3d40c38.png)

托马斯·塔克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 符号。

# 用符号穿越领域

符号不能很好地跨领域传播。

由于不同的帧中有不同的全局对象，我们不能跨帧使用它们。

像`Symbol.iterator`这样的特殊符号应该可以跨不同的领域使用。

为了获得跨不同领域的符号，我们可以使用`Symbol.for`方法获得带有字符串的符号。

例如，我们可以写:

```
const sym = Symbol.for('foo');
```

然后我们可以调用`Symbol.keyFor`来获取我们传递给`Symbol`的字符串，以创建符号:

```
Symbol.keyFor(sym)
```

像`Symbol.iterator`这样的特殊符号，如果我们用`Symbol.keyFor`调用它，就会返回`undefined`:

```
Symbol.keyFor(Symbol.iterator)
```

# 符号是原语还是对象？

符号是原始值。

符号类似于字符串，因为它们可以用作属性键。

但它们就像物品一样，每个符号都有自己的身份。

它们是不可变的，可以用作属性键。

然而，它没有像原型和包装器这样的对象的许多属性。

符号也不能被像`instance`或`Object.keys`这样的操作符和方法检查。

# 符号为什么有用？

符号有助于避免标识符冲突。

这很简单，因为我们不能创建同一个符号两次。

# JavaScript 符号像 Ruby 的符号吗？

JavaScript 和 Ruby 符号不一样。

Ruby 符号是用于创建值的文字。

例如，如果我们有:

```
:foo == :foo
```

然后表达式返回`true`。

`Symbol(‘foo’) !== Symbol(‘foo’)`返回`true`。

所以没有两个符号是相同的，即使我们传入了同一个参数。

# 著名符号的拼写

众所周知的符号以这种方式拼写，因为它们被用作普通的属性键。

# 符号 API

`Symbol`是一个以字符串为描述的函数。

所以我们可以通过写来使用它:

```
const sym = Symbol('foo');
```

得到一个新的符号。

# 符号方法

符号中唯一有用的方法是`toString`方法。

# 著名的符号

有几个众所周知的可能对我们有用。

`Symbol.hasInstance`方法让一个对象定制`instanceof`操作符的行为。

`Symbol.toPrimitive`是一种让我们自定义如何将其转换为原始值的方法。

这是第一次陡降，每当某物被强制转换为原始值时。

`Symbol.toStringTag`是由`Object.prototype.toString`调用的方法，返回对象的字符串描述。

因此，我们可以覆盖它来提供我们自己的字符串描述。

`Symbol.unscopables`是一个用`with`语句隐藏一些属性的方法。

`Symbol.iterator`是一个让我们定义自己的 iterable 来创建 iterable 对象的方法。

一旦我们添加了这个方法，我们就可以用 for-of 操作符迭代它。

# 字符串方法

字符串方法被转发给具有给定符号的方法。

它们的转发方式如下:

*   `String.prototype.match(str, ...)`被转发给`str[Symbol.match]()`。
*   `String.prototype.replace(str, ...)`转发至`str[Symbol.replace]()`。
*   `String.prototype.search(str, ...)`转发至`str[Symbol.search](···)`。
*   `String.prototype.split(str, ...)`转发至`str[Symbol.split]()`。

# 其他特殊符号

`Symbol.species`是配置内置方法来创建类似`this`的对象的方法。

`Symbol.isConcatSpreadable`是一个布尔值，用于配置`Array.prototype.concat`是否添加索引元素作为结果。

![](img/87d3af73c1d1cbed3fc430a99e8326b3.png)

梅丽莎·沃克·霍恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用许多特殊符号来覆盖对象的行为。