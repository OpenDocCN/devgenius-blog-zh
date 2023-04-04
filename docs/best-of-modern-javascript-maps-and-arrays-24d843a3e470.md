# 现代 JavaScript 的精华——地图和数组

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-maps-and-arrays-24d843a3e470?source=collection_archive---------4----------------------->

![](img/000e88b819f11b3afeb0ce0853ab2f10.png)

照片由 [Ronak Jain](https://unsplash.com/@ronakjain?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将通过对地图进行数组操作来了解如何使用地图。

# 将地图转换为数组

我们可以使用 spread 操作符将 iterable 对象转换成数组。

这包括地图。

例如，我们可以写:

```
const map = new Map()
  .set('foo', 'one')
  .set('bar', 'two')
  .set('baz', 'three');const arr = [...map];
```

创建地图并将其转换为数组。

然后我们为`arr`得到一个如下的数组:

```
[
  [
    "foo",
    "one"
  ],
  [
    "bar",
    "two"
  ],
  [
    "baz",
    "three"
  ]
]
```

# 在地图条目上循环

我们可以用`forEach`方法循环遍历映射条目。

该方法接受一个回调，并将`value`和`key`作为参数。

例如，我们可以写:

```
const map = new Map()
  .set('foo', 'one')
  .set('bar', 'two')
  .set('baz', 'three');map.forEach((value, key) => {
  console.log(key, value);
});
```

然后我们得到:

```
foo one
bar two
baz three
```

从控制台日志中。

# 映射和过滤地图

为了映射和过滤地图，我们必须首先将地图转换为数组。

在`Map`构造函数中没有这样做的方法。

因此，要创建一个映射，然后使用它进行过滤和映射，我们可以编写:

```
const map = new Map()
  .set('foo', 1)
  .set('bar', 2)
  .set('baz', 3);const mappedMap = new Map(
  [...map]
  .map(([k, v]) => [k, v ** 2])
);
```

我们创建了一个名为`map`的地图。

然后我们用扩展操作符将`map`扩展到一个数组中。

然后我们在返回的数组实例上调用`map`，并返回一个带有`v`的新数组，其值为平方。

`k`关键是和那个保持不变。

我们在`Map`构造函数中进行映射，以获得一个返回的映射。

最后，我们得到一个如下的地图:

```
{"foo" => 1, "bar" => 4, "baz" => 9}
```

类似地，我们可以调用`filter`来过滤地图条目。

例如，我们可以写:

```
const map = new Map()
  .set('foo', 1)
  .set('bar', 2)
  .set('baz', 3);const mappedMap = new Map(
  [...map]
  .filter(([k, v]) => v < 3)
);
```

我们用相同的回调签名调用了`filter`方法，但是我们只返回值小于 3 的条目。

我们在`Map`构造函数中进行了过滤，以获得一个返回的地图。

最后，我们得到:

```
{"foo" => 1, "bar" => 2}
```

我们还可以使用 spread 运算符来合并地图。

例如，我们可以写:

```
const map = new Map()
  .set('foo', 1)
  .set('bar', 2)
  .set('baz', 3);const map2 = new Map()
  .set('qux', 4)
  .set('quxx', 5);const combinedMap = new Map([...map, ...map2])
```

我们创建了两张地图，`map1`和`map2`。

然后我们用扩展操作符将它们扩展到一个数组中。

`Map`构造函数会将所有条目转换成一个映射。

最后，我们得到:

```
{"foo" => 1, "bar" => 2, "baz" => 3, "qux" => 4, "quxx" => 5}
```

![](img/5f57d4ed86f7ecd4e87332d2dd6a480f.png)

照片由 [XPS](https://unsplash.com/@xps?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

将贴图转换为数组对于各种操作都很有用。

它让我们可以在地图上使用数组方法，这很有用，因为没有与数组方法等价的地图。