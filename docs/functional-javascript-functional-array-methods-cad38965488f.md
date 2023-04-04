# 函数 JavaScript —函数数组方法

> 原文：<https://blog.devgenius.io/functional-javascript-functional-array-methods-cad38965488f?source=collection_archive---------4----------------------->

![](img/e1ca261ce6bac44f1a5fe5b1ef79d794.png)

照片由 [Manja Vitolic](https://unsplash.com/@madhatterzone?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

JavaScript 部分是一种函数式语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将看看如何创建我们自己的数组方法。

# concatAll

我们可以创建自己的`concatAll`方法，将所有嵌套的数组连接成一个大数组。

例如，我们可以写:

```
const concatAll = (arrays) => {
  let results = []
  for (const array of arrays) {
    results = [...results, ...array];
  }
  return results;
}
```

我们只是将所有数组的数组条目展开，然后返回结果数组。

然后我们可以用它来取消嵌套数组。

例如，我们可以写:

```
const arr = concatAll([
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]);
```

那么`arr`就是:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

# 缩减功能

Reduce 是一个函数，它允许我们将数组中的条目合并成一个值。

要创建我们自己的`reduce`函数，我们可以写:

```
const reduce = (array, fn) => {
  let accumlator = 0;
  for (const a of array) {
    accumlator = fn(accumlator, a)
  }
  return accumlator;
}
```

`reduce`函数采用一个`array`和一个`fn`函数。

`array`是我们循环遍历的数组，用于组合来自`array`的值，并将其指定为`accumulator`的值。

`fn`返回一个值，通过一些运算将`accumulator`和`a`值合并为一个值。

一旦我们遍历了数组，我们就返回`accumulator`值。

然后我们可以用它来添加数组中的数字，方法是:

```
const sum = reduce([1, 2, 3, 4, 5], (acc, val) => acc + val)
```

我们传入一个数字数组和一个回调函数来将数组的条目组合在一起。

那么`sum`就是 15，因为我们把所有的数字加在了一起。

通过接受`accumulator`的初始值，我们可以使`reduce`函数更加健壮。

例如，我们可以写:

```
const reduce = (array, fn, initialValue) => {
  let accumlator = initialValue;
  for (const a of array) {
    accumlator = fn(accumlator, a)
  }
  return accumlator;
}
```

我们将`initialValue`指定为`accumulator`的初始值。

这样，我们不需要假设我们总是在处理数字数组。

# 拉链阵列

我们可以将多个数组压缩成一个。

我们用自己的函数为每个数组创建条目。

然后把它放入我们返回的数组中。

我们只循环到最短数组的长度，所以返回的数组也将与最短数组的长度相同。

例如，我们可以写:

```
const zip = (leftArr, rightArr, fn) => {
  let index, results = [],
    length = Math.min(leftArr.length, rightArr.length);
  for (index = 0; index < length; index++) {
    results.push(fn(leftArr[index], rightArr[index]));
  }
  return results;
}
```

我们用`leftArr`、`rightArr`和`fn`参数创建了一个`zip`函数。

`leftArr`和`rightArr`是数组，`fn`是函数。

我们循环通过最短的长度，也就是`length`。

在循环体中，我们将压缩的条目推送到`results`数组。

一旦我们这样做了，我们就返回`results`。

然后我们可以用它来写:

```
const zipped = zip([1, 2, 3, 4], ['foo', 'bar', 'baz'], (a, b) => `${a} - ${b}`)
```

我们有一个回调函数将左右两边的条目组合成一个字符串。

那么`zipped`就是:

```
["1 - foo", "2 - bar", "3 - baz"]
```

![](img/1e640ed545142c7460d497ecb0f2f220.png)

Benjamin Sow 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以取消嵌套数组并将它们与我们自己的函数压缩在一起。