# 使用 Dexie 的 IndexedDB 操作—排序、迭代和键

> 原文：<https://blog.devgenius.io/indexeddb-manipulation-with-dexie-sorting-iteration-and-keys-cbafc314a1b5?source=collection_archive---------3----------------------->

![](img/158e380492f188cb018849c28cf96e11.png)

弗洛里安·伯杰在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

IndexedDB 是在浏览器中存储数据的一种方式。

它允许我们以异步方式存储比本地存储更多的数据。

Dexie 使得使用 IndexedDB 更加容易。

在本文中，我们将了解如何使用 Dexie 开始使用 IndexedDB。

# 反转收集结果的顺序

我们可以用`reverse`方法颠倒收集结果的顺序。

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
  const friends = await db.friends
    .toCollection()
    .reverse()
    .toArray()
  console.log(friends)
})()
```

获取所有项目，然后颠倒返回的顺序。

# 按给定字段排序

我们可以调用`sortBy`方法按照给定的字段进行排序。

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
  const friends = await db.friends
    .where('age')
    .above(25)    
    .sortBy('age');
  console.log(friends)
})()
```

我们用`sortBy`方法通过`age`对结果进行排序。

# 将集合转换为数组

我们可以用`toArray`方法将集合转换成数组。

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
  const friends = await db.friends
    .where('age')
    .above(25)    
    .toArray();
  console.log(friends)
})()
```

我们使用`toArray`方法将查询方法返回的集合转换为带有数组的承诺。

# 获取唯一键

我们可以用`uniqueKeys`方法从一个集合中获得唯一的键。

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
    .toCollection()
    .uniqueKeys()
  console.log(keys)
})()
```

我们调用`uniqueKeys`从集合中获取唯一的键。

我们从集合中获取`id`值。

所以我们得到:

```
[1, 2]
```

从控制台日志中。

# 一旦给定的过滤器返回 true，停止迭代集合

一旦给定的过滤器是`true`，我们可以调用`until`方法来停止迭代集合。

例如，我们可以这样使用它:

```
const db = new Dexie("friends");
let cancelled = false;db.version(1).stores({
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
    .toCollection()
    .until(() => cancelled)
    .each(friend => {
      console.log(friend)
      cancelled = true;
    });
})()
```

我们用一个回调函数调用`until`，该回调函数返回`cancelled`的值。

然后我们调用`each`并记录`friend`的值，然后我们从`cancelled`到`true`。

所以在第一次迭代`each`之后，它将停止遍历集合的其余部分。

# 结论

我们可以对项目进行排序，将项目转换为数组，获得唯一的键，并使用 Dexie 停止给定条件下的迭代。