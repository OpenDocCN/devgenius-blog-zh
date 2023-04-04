# 现代 JavaScript 的精华——集合操作

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-set-operations-aa2b3fd05b9?source=collection_archive---------5----------------------->

![](img/76a6532370ea261a5dacd53df54e9547.png)

照片由[雅各布·欧文](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在这篇文章中，我们将看看集合。

# 遍历集合

我们可以用 for-of 循环遍历集合，因为集合是可迭代的对象。

例如，我们可以写:

```
const set = new Set(['foo', 'bar', 'baz']);
for (const x of set) {
  console.log(x);
}
```

然后我们可以写:

```
foo
bar
baz
```

记录在案。

spread 运算符使用 iterables 将它们转换为数组。

这包括集合，所以我们可以写:

```
const set = new Set(['foo', 'bar', 'baz']);
const arr = [...set];
```

而`arr`就是`[“foo”, “bar”, “baz”]`。

# 映射和过滤

为了进行映射和过滤操作，我们可以使用 spread 操作符将集合转换为数组。

然后我们可以在这些方法上调用`map`和`filter`方法。

例如，我们可以通过编写以下内容将集合项目映射到新值:

```
const set = new Set([1, 2, 3]);
const squares = new Set([...set].map(x => x ** 2));
```

然后我们得到:

```
{1, 4, 9}
```

而不是:

```
{1, 2, 3}
```

要过滤项目，我们可以在数组上调用`filter`方法:

```
const set = new Set([1, 2, 3]);
const filtered = new Set([...set].filter(x => (x % 3) == 0));
```

我们过滤掉所有不能被 3 整除的东西，所以我们得到:

```
{3}
```

# 并集、交集和差集

我们可以用各种数组运算来计算两个集合的并、交和差。

# 联合

并集是同时包含集合`a`和`b`元素的集合。

我们可以使用 spread 运算符创建一个并集。

所以我们可以写:

```
const a = new Set([1, 2, 3]);
const b = new Set([1, 3, 5]);
const union = new Set([...a, ...b]);
```

而`union`就是`{1, 2, 3, 5}`。

spread 运算符将两个集合组合成一个数组。

`[...a, ...b]`相当于`[...a].concat([...b])`。

`Set`构造函数将它转换回一个集合，并在这个过程中删除重复项。

# 交集

集合交集是指集合`a`中的元素也在集合`b`中的集合。

要从 2 个集合创建交集，我们可以写为:

```
const a = new Set([1, 2, 3]);
const b = new Set([1, 3, 5]);
const intersection = new Set(
  [...a].filter(x => b.has(x)));
```

我们创建 2 个集合`a`和`b`。

然后我们可以用`filter`方法得到`a`中的所有项目，这些项目也在`b`中。

`has`方法检查`b`是否也有相同的项目。

`Set`构造函数将数组转换回集合。

因此，我们得到:

```
{1, 3}
```

fot `intersection`。

# 集合差异

集合差异允许我们从 2 个集合`a`和`b`中创建一个集合，其中集合`a`中的项目不在集合`b`中。

要创建集合差异，我们可以写:

```
const a = new Set([1, 2, 3]);
const b = new Set([1, 3, 5]);
const difference = new Set(
  [...a].filter(x => !b.has(x)));
```

我们用一个回调函数调用`filter`，该回调函数否定`has`中返回的内容，以返回所有不在`b`中但在`a`中的项目。

所以我们得到:

```
{2}
```

作为`difference`的值。

![](img/ea45082b0b05029689f2a4bc9e1e4598.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [KAL VISUALS](https://unsplash.com/@kalvisuals?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以遍历集合，用数组运算来计算并、差和交。