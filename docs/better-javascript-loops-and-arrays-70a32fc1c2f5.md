# 更好的 JavaScript——循环和数组

> 原文：<https://blog.devgenius.io/better-javascript-loops-and-arrays-70a32fc1c2f5?source=collection_archive---------9----------------------->

![](img/89a1acafb8a09e0856ee159392af3a74.png)

沃尔代·瓦格纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 不要在枚举过程中修改对象

我们不应该在枚举过程中修改对象。

for-in 循环不需要与对象修改保持同步。

因此我们可能会在循环中得到过期的项目。

这意味着如果我们改变被修改的对象，我们应该依靠 for-in 循环来实现可预测的行为。

所以我们不应该有这样的代码:

```
const dict = {
  james: 33,
  bob: 22,
  mary: 41
}for (const name in dict) {
  delete dict.bob;
}
```

我们有了`dict`对象，但是我们在 for-in 循环中修改了它。

但是这个循环并不需要获得最新的更改，所以`bob`可能仍然会出现。

# 对于数组迭代，for-of 循环优于 for-in 循环

for-in 循环不是用来遍历数组的。

顺序是不可预测的，我们得到的是项目的键，而不是项目本身。

所以如果我们有这样的东西:

```
const scores = [4, 4, 5, 7, 7, 3, 6, 6];
let total = 0;
for (const score in scores) {
  total += score;
}
const mean = total / scores.length;
```

`score`会是这个阵的关键。

所以我们不会把分数加起来。

此外，键将是一个字符串，所以我们将连接键字符串，而不是添加。

相反，我们应该使用 for-of 循环来遍历数组。

例如，我们可以写:

```
const scores = [4, 4, 5, 7, 7, 3, 6, 6];
let total = 0;
for (const score of scores) {
  total += score;
}
const mean = total / scores.length;
```

通过 for-of 循环，我们获得数组或任何其他可迭代对象的条目，这样我们实际上就获得了数字。

# 更喜欢迭代方法而不是循环

我们应该尽可能使用数组方法来操作数组条目，而不是循环。

例如，我们可以使用`map`方法将条目映射到一个数组。

我们可以写:

```
const inputs = ['foo ', ' bar', 'baz ']
const trimmed = inputs.map((s) => {
  return s.trim();
});
```

我们用回调函数调用了`map`来修剪每个字符串中的空白。

这比使用如下循环要短得多:

```
const inputs = ['foo ', ' bar', 'baz ']
const trimmed = [];
for (const s of inputs) {
  trimmed.push(s.trim());
}
```

我们必须编写更多的代码来进行修整，并将其推送到`trimmed`数组。

还有很多其他的方法，比如`filter`、`reduce`、`reduceRight`、`some`、`every`等等。我们可以用它来简化我们的代码。

![](img/b4db0092d0bca722f811f4f733b293c3.png)

照片由[Tine ivani](https://unsplash.com/@tine999?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们不应该在枚举过程中修改对象。

此外，我们应该更喜欢迭代方法而不是循环。

for-of 比 for-in 循环更适合迭代。