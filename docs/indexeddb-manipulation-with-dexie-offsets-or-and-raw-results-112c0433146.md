# 带 Dexie 的 IndexedDB 操作—偏移、Or 和原始结果

> 原文：<https://blog.devgenius.io/indexeddb-manipulation-with-dexie-offsets-or-and-raw-results-112c0433146?source=collection_archive---------2----------------------->

![](img/9c1f1ddc3d53f742415e3d22683df09c.png)

照片由[马特·琼斯](https://unsplash.com/@mattjonesdp?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

IndexedDB 是在浏览器中存储数据的一种方式。

它允许我们以异步方式存储比本地存储更多的数据。

Dexie 使得使用 IndexedDB 更加容易。

在本文中，我们将了解如何使用 Dexie 开始使用 IndexedDB。

# 忽略给定偏移量之前的前 n 项

我们可以调用`offset`方法来返回给定偏移量后的第一个`n`项。

例如，我们可以这样使用它:

```
const db = new Dexie("friends");
db.version(1).stores({
  friends: "id, name, age"
});
(async () => {
  await db.friends.put({
    id: 1,
    name: "jane",
    age: 78
  });
  await db.friends.put({
    id: 2,
    name: "mary",
    age: 76
  });
  await db.friends
    .orderBy('name')
    .offset(10);
})()
```

然后，我们跳过集合中的前 10 个结果，包括其余的结果。

# 逻辑或运算

我们可以使用`or`方法在查询中用逻辑 OR 运算符将两个条件组合在一起。

例如，我们可以写:

```
const db = new Dexie("friends");
db.version(1).stores({
  friends: "id, name, age"
});
(async () => {
  await db.friends.put({
    id: 1,
    name: "jane",
    age: 78
  });
  await db.friends.put({
    id: 2,
    name: "mary",
    age: 76
  });
  const someFriends = await db.friends
    .where("name")
    .equalsIgnoreCase("jane")
    .or("age")
    .above(40)
    .sortBy("name")
  console.log(someFriends)
})()
```

我们用`'name'`参数调用`where`来查询名称。

我们用`equalsIgnoreCase`找到具有给定名称的条目。

然后我们调用`or`来查找`age`大于 40 的项目。

我们调用`sortBy`来按照`name`字段排序。

# 获取包含集合中所有主键的数组

我们可以使用`primaryKeys`方法返回一个带有集合中主键数组的承诺。

例如，我们可以这样使用它:

```
const db = new Dexie("friends");
db.version(1).stores({
  friends: "id, name, age"
});
(async () => {
  await db.friends.put({
    id: 1,
    name: "jane",
    age: 78
  });
  await db.friends.put({
    id: 2,
    name: "mary",
    age: 76
  });
  const keys = await db.friends
    .where("name")
    .equalsIgnoreCase("jane")
    .primaryKeys()
  console.log(keys)
})()
```

`id`是每个条目的主键。

然后我们调用`primaryKeys`返回一个带有主键数组的承诺。

# 获得原始结果

我们可以用`raw`方法从查询中获得原始结果。

结果不会通过阅读挂钩过滤。

例如，我们可以写:

```
const db = new Dexie("friends");
db.version(1).stores({
  friends: "id, name, age"
});
(async () => {
  await db.friends.put({
    id: 1,
    name: "jane",
    age: 78
  });
  await db.friends.put({
    id: 2,
    name: "mary",
    age: 76
  });
  const keys = await db.friends
    .where("name")
    .equalsIgnoreCase("jane")
    .raw()
    .toArray()
  console.log(keys)
})()
```

去使用它。

使用`raw`不会将对象映射到它们映射的类。

# 结论

我们可以忽略结果中的第一个`n`项，获取主键，并使用 Dexie 从 IndexedDB 集合中获取原始结果。