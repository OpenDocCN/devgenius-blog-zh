# 函数式 JavaScript —函数式编程和数组

> 原文：<https://blog.devgenius.io/functional-javascript-functional-programming-and-arrays-bfa9b2d6a069?source=collection_archive---------4----------------------->

![](img/983f3f93c535f49fb4a9a152c621cf93.png)

照片由 [Richard T](https://unsplash.com/@newhighmediagroup?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是一种函数式语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将看看如何创建我们自己的数组方法。

# 函数式处理数组

数组函数对于操作数组很有用。

像`map`和`forEach`这样的内置数组方法还有很多。

他们中的许多人采取回调，让我们做一些与数组条目。

# 地图

`map`方法让我们将数组条目从一种东西转换成另一种东西。

为了实现它，我们可以遍历每个条目，并调用我们传入的函数。

该函数的返回值将被推入一个新数组并返回。

例如，我们可以写:

```
const map = (array, fn) => {
  const results = []
  for (const a of array) {
    results.push(fn(a));
  }
  return results;
}
```

`map`函数采用一个`array`和`fn`函数。

我们使用 for…of 循环遍历条目。

`fn`用于将一个值映射到另一个值。

然后我们在`results`中推送返回的项目。

我们返回`results`数组。

然后我们可以通过写来使用它:

```
const arr = map([1, 2, 3], (x) => x ** 3);
```

那么`arr`就是:

```
[1, 8, 27]
```

# 过滤器

我们可以实现自己的`filter`函数，让我们返回一个新的数组，其中包含满足给定条件的条目。

条件在回调中。

例如，我们可以写:

```
const filter = (array, fn) => {
 let results = [];
  for (const a of array) {
    if (fn(a)) {
      results.push(a)
    }
  }
  return results;
}
```

我们用 for…of 循环遍历数组。

然后我们用带有参数`a`的`fn`函数检查它是否返回`true`。

如果它返回`true`，那么我们将项目`a`推入`results`。

一旦我们遍历了所有的条目，我们就返回`results`。

然后我们可以通过写来使用它:

```
const arr = filter([1, 2, 3], a => a % 2 === 1);
```

然后我们得到:

```
[1, 3]
```

作为`arr`的值。

# 链接操作

我们可以将这两个操作链接起来，因为它们都是纯函数。

例如，我们可以过滤我们的项目，然后将它们映射到我们选择的值:

```
const map = (array, fn) => {
  const results = []
  for (const a of array) {
    results.push(fn(a));
  }
  return results;
}const filter = (array, fn) => {
  let results = [];
  for (const a of array) {
    if (fn(a)) {
      results.push(a)
    }
  }
  return results;
}const arr = map(filter([1, 2, 3], a => a % 2 === 1), a => a ** 3);
```

我们首先调用`filtet`函数来返回一个奇数数组。

然后我们对所有的奇数求立方，并返回立方奇数的数组。

所以`arr`是:

```
[1, 27]
```

我们可以用一种更简洁的方式重写它:

```
const odd = filter([1, 2, 3], a => a % 2 === 1);
const arr = map(odd, a => a ** 3);
```

![](img/fbf508aa1098581f168cc5475aa92203.png)

由[凯尔·格伦](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以创建自己的`map`和`filter`函数来映射数组条目并返回一个过滤后的数组。