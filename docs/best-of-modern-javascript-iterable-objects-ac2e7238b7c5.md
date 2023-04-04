# 现代 JavaScript 的精华——可迭代对象

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-iterable-objects-ac2e7238b7c5?source=collection_archive---------3----------------------->

![](img/a8805eda3ae7927394bdc5a22a78c178.png)

照片由[马尔特·温根](https://unsplash.com/@maltewingen?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 可迭代对象。

# 可迭代计算数据

可迭代的内容可以来自计算的数据。

方法可以返回我们可以迭代的可迭代对象。

我们有 3 个返回可迭代对象的方法。它们包括`entries`、`keys`和`values`方法。

`entries`返回编码为键值数组的条目的 iterable。

对于数组，键是索引，值是元素。

对于集合，键和值都是元素。

`keys`返回一个包含条目关键字的 iterable 对象。

`values`返回一个包含条目值的 iterable 对象。

例如，我们可以写:

```
const arr = ['foo', 'bar', 'baz'];
for (const [key, value] of arr.entries()) {
  console.log(key, value);
}
```

然后我们得到:

```
0 "foo"
1 "bar"
2 "baz"
```

记录在案。

# 地图和集合

`Map`构造函数将一个可迭代的键值对转换成一个映射。

例如，我们可以写:

```
const map = new Map([
  ['foo', 'one'],
  ['bar', 'two']
]);
```

然后我们可以调用`get`来获取值。

例如，我们可以写:

```
const foo = map.get('foo');
```

并返回`'one'`。

同样，我们可以将一个数组传递给`Set`构造函数。

例如，我们可以写:

```
const set = new Set(['red', 'green', 'blue']);
```

然后我们可以使用`has`方法来检查元素是否存在。：

```
const hasRed = set.has('red');
```

WeakMaps 和 WeakSets 的工作原理相似。

地图和集合本身是可迭代的，但是弱地图和弱集合不是。

# 承诺

`Promise.all`和`Promise.race`把承诺看得比承诺更重。

所以我们可以写:

```
Promise.all([
  Promise.resolve(1),
  Promise.resolve(2),
]).then(() => {
  //...
});
```

并行运行一系列承诺。

`Promise.race`解析为承诺数组中第一个承诺的值。

例如，我们可以写:

```
Promise.race([
  Promise.resolve(1),
  Promise.resolve(2),
]).then(() => {
  //...
});
```

决定完成的第一个承诺的价值。

# 实现 Iterables

JavaScript 可迭代对象有`Symbol.iterator`方法。

该方法返回一个具有`value`和`done`属性的对象。

`value`有我们从 iterable 返回的值。

`done`表示是否有剩余值。如果是`true`，那么就没有值可以返回了。

例如，我们可以写:

```
const obj = {
  [Symbol.iterator]() {
    let step = 0;
    const iterator = {
      next() {
        if (step <= 2) {
          step++;
        }
        switch (step) {
          case 1:
            return {
              value: 'foo', done: false
            };
          case 2:
            return {
              value: 'bar', done: false
            };
          default:
            return {
              value: undefined, done: true
            };
        }
      }
    };
    return iterator;
  }
};
```

`Symbol.iterator`方法根据`step`号返回一个对象。

然后我们可以使用 for-of 循环来确认对象是可迭代的:

```
for (const x of obj) {
  console.log(x);
}
```

我们应该得到:

```
foo
bar
```

记录在案。

如果是`false`，我们可以省略`done`，如果是`undefined`，我们可以省略`value`。

所以我们可以写:

```
switch (step) {
  case 1:
    return {
      value: 'foo'
    };
  case 2:
    return {
      value: 'bar'
    };
  default:
    return {
      done: true
    };
}
```

![](img/7bb1d3071a3d7adcf6da75cebb0d12cc.png)

[Tine ivani](https://unsplash.com/@tine999?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

Iterable 对象有特殊的方法来区分它们。