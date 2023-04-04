# 现代 JavaScript 精华—集合和弱集合

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-sets-and-weaksets-287923725ff1?source=collection_archive---------10----------------------->

![](img/ea0a523a6b509e5643bff9a2e101fae8.png)

照片由[伊戈尔·斯塔尔科夫](https://unsplash.com/@igorstarkoff?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究集合和弱集合。

# 设置 API

API 有各种方法可以让我们操作它。

构造函数让我们传入一个 iterable 对象并从中创建一个集合。

例如，我们可以写:

```
const set = new Set([1, 2, 3]);
```

从数组创建集合。

`Set.prototype.add`方法获取一个值并将其附加到集合中。

它返回包含新条目的集合。

`Set.prototype.has`取一个值，如果它在集合中就返回`true`，否则返回`false`。

`Set.prototype.delete`方法获取一个值并让我们移除它。

如果被删除，它返回`true`，否则返回`false`。

`Set.prototype.size` getter 返回集合中的项目数。

方法让我们从集合中移除所有的项目。

`Set.prototype.values`是一个方法，将一个集合的所有值作为迭代器返回。

`Set.prototype[Symbol.iterator]`返回一个 iterable 对象，让我们迭代一个集合。

`Set.prototype.forEach` let 接受一个带有签名的回调，`(value, key, collection)`作为签名，并为集合中的每个项目运行它，

`value`有设定值。

`key`与`value`的值相同。

`collection`是器械包本身。

第二个参数是我们在回调中使用的`this`的值。

集合也有`Set.prototype.entries`方法来返回一个 iterable 对象，每个条目都是一个键值数组。

键和值是相同的。

`Set.prototype.keys`方法返回给我们一个带有键的 iterable 对象，键与值相同。

# WeakSet

WeakSet 是一个不阻止其元素被垃圾收集的集合。

它像 WeakMaps 一样工作，不允许迭代、循环或清除。

WeakSets 没有太多的用例。

我们可以向它们添加对象，然后通过引用获取它们。

所以我们可以写:

```
const weakSet = new WeakSet();const obj = {};
weakSet.add(obj);
const hasObj = weakSet.has(obj);
```

我们传入一个地图，然后用`has`检查它的值。

我们只能将对象传递到 WeakSet 中。

如果我们试图传入一个原始值，我们将得到一个 TypeError。

此外，我们可以使用 WeakSets 来允许我们在给定的类实例上运行方法。

例如，我们可以写:

```
const bars = new WeakSet();class Bar {
  constructor() {
    bars.add(this);
  } method() {
    if (!bars.has(this)) {
      throw new TypeError('not a bar instance');
    }
  }
}
```

我们将`Bar`实例添加到构造函数中的`bars` WeakSet。

然后在`method`方法中，我们检查`Bar`实例是否是带有`has`的 WeakSet 的一部分。

如果不是，那么我们抛出一个错误。

这防止该方法在除了`Bar`实例之外的任何实例上运行。

# WeakSet API

WeakSets 有一个简单的 API，包含 3 个方法，工作方式类似于它们的`Set`对等物。

`WeakSet.prototype.add`接受一个`value`作为参数，让我们向它添加一个条目。

`WeakSet.prototype.has`获取一个`value`，如果条目存在则返回`true`，否则返回`false`。

`WeakSet.prototype.delete`取一个`value`并从 WeakSet 中移除元素。

![](img/68f9d3089180510e0b7a2d1fd6120350.png)

Adolfo Félix 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

集合让我们无需重复就能添加项目。

WeakSets 让我们保存不用时可以作为垃圾回收的物品。