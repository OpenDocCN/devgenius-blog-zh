# 现代 JavaScript 的精华——地图和 WeakMaps

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-maps-and-weakmaps-15ae4a9c2632?source=collection_archive---------10----------------------->

![](img/3c1074bae59431d0114158e60a0e6a82.png)

[库利基图斯](https://unsplash.com/@kyllik?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在这篇文章中，我们将看看地图和弱地图。

# 迭代和循环遍历地图

我们可以用各种方法迭代和循环遍历地图。

`Map.prototype.entries`返回一个 iterable 对象，为我们的映射的每一个条目提供键值对数组。

`Map.prototype.forEach`接受一个带有签名`(value, key, collection)`的回调，让我们遍历地图。

`value`有地图条目的值。

`key`拥有地图入口的钥匙。

`collection`有地图本身。

它的第二个参数是回调函数中`this`的值。

`Map.prototype.keys`返回一个带有映射键的 iterable。

`Map.prototype.values`返回映射中所有值的 iterable。

`Map.prototype[Symbol.iterator]`是遍历地图的默认方法。

它返回一个带有键值对数组的 iterable。

# WeakMap

WeakMaps 的工作方式很像地图。

WeakMaps 键是对象。他们很脆弱。

我们无法用一张地图查看所有条目。

我们无法清除地图。

我们必须放入对象作为键，所以我们不能写。

```
const wm = new WeakMap()wm.set('foo', 123);
```

因为这样做我们会得到一个类型错误。

但是我们可以写:

```
const wm = new WeakMap()wm.set({}, 123);
```

WeakMap 键握得不牢。

这意味着没有被任何对象或属性引用的对象可以被垃圾回收。

除非它们被藏在某个地方，否则我们无法接近它们。

一旦钥匙不见了，那么条目也就消失了。

我们无法得到一张地图的概貌。

这是因为没有办法检查它的内部。

获取 WeakMap 内容的唯一方法是通过密钥获取内容。

WeakMap 的用例包括缓存、管理侦听器和保存私有数据。

使用 WeakMaps，我们可以用它缓存一个对象，因为我们只能有对象键。

例如，我们可以写:

```
const cache = new WeakMap();function countOwnKeys(obj) {
  if (cache.has(obj)) {
    return cache.get(obj);
  } else {
    const num = Math.random();
    cache.set(obj, num);
    return num;
  }
}
```

创建一个 WeakMap 并获取条目(如果该键存在)

否则，我们向 WeakMap 添加一个条目。

我们还可以使用它们将侦听器存储在 WeakMap 中。

例如，我们可以写:

```
const listeners = new WeakMap();function addListener(obj, listener) {
  if (!listeners.has(obj)) {
    listeners.set(obj, new Set());
  }
  listeners.get(obj).add(listener);
}
```

我们使用`addListener`函数将事件监听器添加到集合中，如果它还不存在的话。

如果`obj`不在 WeakMap 中，那么我们创建一个条目，将`obj`作为键，将 set 作为值。

它对于保存私有数据也很有用，因为我们需要引用 key 对象来获取条目。

所以我们可以写:

```
const _counter = new WeakMap();class Countdown {
  constructor(counter) {
    _counter.set(this, counter);
  } increment() {
    let counter = _counter.get(this);
    if (counter < 1) {
      return;
    }
    counter++;
    _counter.set(this, counter);
  }
}
```

用构造函数中的`set`方法调用将`counter`存储在 WeakMap 中。

在`increment`方法中，我们用`_counter.get`方法得到计数器。

然后我们递增`counter`并用`set`设置新值。

![](img/adf6d4f4cfcf30f38d4da0202901594c.png)

照片由[艾里斯·蒂默曼斯](https://unsplash.com/@iristimmermans?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用各种方法遍历地图。

此外，WeakMaps 可以用于存储私有数据、缓存等等。