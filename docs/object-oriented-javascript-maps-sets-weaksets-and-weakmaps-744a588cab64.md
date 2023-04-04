# 面向对象的 JavaScript —地图、集合、WeakSets 和 WeakMaps

> 原文：<https://blog.devgenius.io/object-oriented-javascript-maps-sets-weaksets-and-weakmaps-744a588cab64?source=collection_archive---------7----------------------->

![](img/7e089f2ce301e1fedc3eb944b3e28deb.png)

威廉·拜罗伊特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看地图迭代、集合、弱集合和弱地图。

# 迭代地图

我们可以用`keys`方法迭代映射来迭代键。

例如，我们可以写:

```
const m = new Map([
  ['one', 1],
  ['two', 2],
  ['three', 3],
]);for (const k of m.keys()) {
  console.log(k);
}
```

然后我们得到:

```
one
two
three
```

从控制台日志中。

我们可以用`values`方法迭代这些值:

```
for (const v of m.values()) {
  console.log(v);
}
```

然后我们得到:

```
1
2
3
```

从控制台日志中。

我们可以用`entries`方法遍历条目:

```
for (const [k, v] of m.entries()) {
  console.log(k, v);
}
```

然后我们得到:

```
one 1
two 2
three 3
```

我们重构了从`entries`方法返回的键和值。

# 将地图转换为数组

我们可以用 spread 操作符将地图转换成数组。

例如，我们可以写:

```
const m = new Map([
  ['one', 1],
  ['two', 2],
  ['three', 3],
]);const keys = [...m.keys()]
```

从地图中获取关键字并将其转换为数组。

所以我们得到:

```
["one", "two", "three"]
```

我们还可以扩展映射，将键值对扩展到键值对的嵌套数组中。

所以如果我们有:

```
const m = new Map([
  ['one', 1],
  ['two', 2],
  ['three', 3],
]);const arr = [...m]
```

然后我们得到:

```
[
  [
    "one",
    1
  ],
  [
    "two",
    2
  ],
  [
    "three",
    3
  ]
]
```

作为`arr`的价值。

# 一组

一个`Set`是不能有重复条目的值的集合。

例如，我们可以写:

```
const s = new Set();
s.add('first');
```

然后我们创建一个带有条目的集合。

`add`添加一个条目。

`has`检查值是否存在。我们可以通过书写来使用它:

```
s.has('first');
```

`delete`让我们删除一个值:

```
s.delete('first');
```

我们也可以创建一个数组，这样我们可以写:

```
const colors = new Set(['red', 'green', 'blue']);
```

用字符串创建一个集合。

# WeakMap 和 WeakSet

`WeakMap`和`WeakSet`与`Map`和`Set`相似，但更受限制。

`WeakMap` s 只有`has()`、`get()`、`set()`和`delete()`方法。

`WeakSet` s 只有`has()`、`add()`和`delete()`方法。

`WeakMap`的按键必须是对象。

我们只能用它的键访问一个`WeakMap`值。

`WeakSet`的值必须是对象。

我们不能迭代一个`WeakMap`或者`WeakSet`。

我们无法清除一个`WeakMap`或`WeakSet`。

`WeakMap`当我们不想控制我们保存的对象的生命周期时，s 是有用的。

当密钥不再可用时，它将被自动垃圾收集。

![](img/3bfd76751c3f9938d3537ee896cb7b43.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[克里·基特斯](https://unsplash.com/@kyllik?utm_source=medium&utm_medium=referral)拍摄的照片

# 结论

地图、集合、WeakMaps 和 WeakSets 是我们可以在应用程序中使用的有用数据结构。