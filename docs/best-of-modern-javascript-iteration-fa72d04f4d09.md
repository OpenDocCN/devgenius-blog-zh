# 现代 JavaScript 的精华——迭代

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-iteration-fa72d04f4d09?source=collection_archive---------5----------------------->

![](img/627ea8275c18e6aafeb286454f75e3f6.png)

rafael Souza 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 可迭代对象。

# 迭代协议的速度

在创建迭代协议时，已经考虑了它的速度。

管理小对象时，内存管理速度很快。

JavaScript 引擎优化迭代，以便不分配中间对象。

# 重用相同的可迭代对象

我们可以多次使用 iterable。

所以我们可以写:

```
const results = [];
const iterator = [1, 2, 3][Symbol.iterator]();while (!(val = iterator.next()).done) {
  results.push(val);
}
```

我们从数组中获取迭代器。

然后我们用我们的`while`循环调用它来得到结果。

# 循环

JavaScript 迭代协议有规则可循。

`next`方法有一些规则。

只要迭代器返回值`next`，就返回一个带有`value`属性的对象。

而`done`将会是`false`。

所以我们有:

```
{ value: x, done: false }
```

迭代完最后一个值后，`next`应该返回一个属性`done`为`true`的对象。

# 返回迭代器的 Iterables

Iterables 可以返回新的迭代器或者返回相同的迭代器。

如果它们返回新的迭代器，那么每个迭代器都从开始返回值。

所以如果我们有一个数组，我们用:

```
function getIterator(iterable) {
  return iterable[Symbol.iterator]();
}
```

来获取迭代器，然后我们可以比较它们，看它们是否返回相同的迭代器。

例如，如果我们有:

```
const arr = ['a', 'b'];
console.log(getIterator(arr) === getIterator(arr));
```

那么表达式将记录`false`。

这意味着即使数组是相同的，它们也返回相同的迭代器。

其他的可迭代对象，比如生成器，每次都返回相同的迭代器。

如果我们有生成器对象，我们每次调用时都返回同一个生成器:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
}const gen = genFn();
console.log(getIterator(gen) === getIterator(gen));
```

`genFn`是一个生成器函数，返回一个生成器，

当我们从生成器中得到迭代器时，我们从生成器中得到迭代器，并将它们与表达式日志`true`进行比较。

所以同一个生成器有相同的迭代器。

我们可以多次迭代一个新的迭代器。

例如，我们可以写:

```
const arr = ['foo', 'bar', 'baz'];for (const a of arr) {
  console.log(a);
}for (const a of arr) {
  console.log(a);
}
```

在同一个数组中循环两次。

另一方面，如果我们有一个发电机:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
}
const gen = genFn();for (const a of gen) {
  console.log(a);
}for (const a of gen) {
  console.log(a);
}
```

即使我们有两个循环，我们也只循环一次。

# 关闭迭代器

有两种方法可以关闭迭代器。

迭代器可以用穷举或关闭来关闭。

穷举是指迭代器返回所有可迭代的值。

通过在迭代器函数中调用`return`来完成关闭。

当我们呼叫`return`时，那么`next`就不会被呼叫。

`return`是可选方法。

不是所有迭代器都有。

有一个`return`调用的迭代器被称为可关闭的。

只有当迭代器没有用尽时，才应该调用`return`。

如果我们用`break`、`continue`、`return`或`throw`循环 for-of 循环，就会发生这种情况。

`return`应该产生一个返回`{ done: true, value: x }`的对象。

![](img/0a757fde2f750e6df8cc651a91c52893.png)

[瓦桑特](https://unsplash.com/@iamv_7?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

可迭代对象可以有不同的变化。

它们可以返回单个迭代器或多个实例。

它们也可以是可关闭的。