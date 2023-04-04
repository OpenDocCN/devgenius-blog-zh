# 如何获取一个 JavaScript 对象的长度？

> 原文：<https://blog.devgenius.io/how-to-get-the-length-of-a-javascript-object-66644311e40f?source=collection_archive---------4----------------------->

![](img/dc7fbfc80641d368b3e0b33253dafaa7.png)

ahin Sezer diner 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

获取 JavaScript 对象的长度意味着获取对象中属性的数量。

这是我们有时可能会做的事情。

在本文中，我们将了解如何获取 JavaScript 对象的长度。

# `Object.keys`

`Object.keys`让我们以数组的形式获取对象的非继承字符串键。

这意味着我们可以使用返回数组的`length`属性来获取对象的长度。

这个方法从 ES6 开始就有了，所以我们可以在任何地方安全地使用它。

例如，我们可以写:

```
const obj = {
  a: 1,
  b: 2
}
const size = Object.keys(obj).length;
console.log(size)
```

我们用`obj`对象调用`Object.keys`来从`obj`获得一个非继承的字符串键数组。

它不包括不可枚举的属性，这些属性是我们不能用`for-in`循环遍历的。

# `Object.getOwnPropertyNames()`

我们可以使用`Object.getOwnPropertyNames()`方法获得一个对象的所有非继承的字符串键，包括非继承的属性。

例如，我们可以写:

```
const obj = {
  a: 1,
  b: 2
}
const size = Object.getOwnPropertyNames(obj).length;
console.log(size)
```

上一个例子和这个例子没有区别，因为`obj`的所有属性都是可枚举的。

然而，如果为一个数组设置了任何不可枚举的属性，那么我们会得到不同的结果，因为用`Object.getOwnPropertyNames`返回了额外的不可枚举属性。

# 符号键

上面的两个方法不返回任何符号键。

要返回符号键，我们必须使用`Object.getOwnPropertySymbols`方法。

例如，如果我们有一个带有符号键的对象，那么我们可以这样调用这个方法:

```
const obj = {
  [Symbol('a')]: 1,
  [Symbol('b')]: 2
}
const size = Object.getOwnPropertySymbols(obj).length;
console.log(size)
```

我们得到`size`是 2，因为对象中有 2 个符号键。

如果一个对象既有符号键又有字符串键，那么我们可以将`Object.keys`或`Object.getOwnPropertyNames`与`Object.getOwnPropertySymbols`一起使用。

例如，我们可以写:

```
const obj = {
  [Symbol('a')]: 1,
  [Symbol('b')]: 2,
  c: 3
}
const symbolSize = Object.getOwnPropertySymbols(obj).length;
const stringSize = Object.getOwnPropertyNames(obj).length;
const size = symbolSize + stringSize
console.log(size)
```

我们将`symbolSize`和`stringSize`相加，得到`obj`中的按键总数。

所以`size`是 3。

# 结论

要获得一个对象中字符串键的数量，我们可以使用`Object.getOwnPropertyNames`或`Object.keys`。

为了得到一个对象的符号键的数量，我们可以使用`Object.getOwnPropertySymbols`。