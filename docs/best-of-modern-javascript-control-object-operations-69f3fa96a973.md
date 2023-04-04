# 现代 JavaScript 的精华——控制对象操作

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-control-object-operations-69f3fa96a973?source=collection_archive---------6----------------------->

![](img/24fd351582daf101b18452fc8c286d57.png)

让·卡洛·埃默在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究使用 JavaScript 代理的元编程。

# 处理负数组索引

有了代理，我们可以通过访问负索引的数组来增强数组。

例如，我们可以写:

```
const handler = {
  get(target, propKey, receiver) {
    const index = Number(propKey);
    if (index < 0) {
      propKey = String(target.length + index);
    }
    return target[propKey];
  }
};const target = [];
target.push(1, 2, 3);
const arr = new Proxy(target, handler);console.log(arr[-1])
```

我们用`get`方法创建了`handler`对象。

如果我们有一个小于 0 的`index`，那么我们通过给它加上`target.length`使它为正。

然后我们用数组的索引返回数组的属性。

接下来，我们用参数创建了`target`数组。

然后我们返回一个代理，它是一个类似数组的对象，我们可以对它使用负索引。

# 设置属性

`handler`对象中的`set`方法让我们控制如何在对象中设置属性。

例如，我们可以写:

```
function createArray(callback) {
  const array = [];
  return new Proxy(array, {
    set(target, propertyKey, value, receiver) {
      callback(propertyKey, value);
      return Reflect.set(target, propertyKey, value, receiver);
    }
  });
}
const arr = createArray(
  (key, value) => console.log(key, value));
arr.push('a');
```

创建一个数组，我们可以观察它的操作。

我们有一个`createArray`函数，它接受一个`callback`，每当我们在数组上设置一个值时，这个函数就会被调用。

用`propertyKey`和`value`调用`callback`。

然后我们会看到阵列操作在完成时被记录下来。

# `Revocable References`

我们可以创建可撤销的引用，这样我们只有在被允许的情况下才能访问对象。

例如，我们可以写:

```
const target = {};
const handler = {};
const {
  proxy,
  revoke
} = Proxy.revocable(target, handler);
```

`target`是一个我们可以用来创建代理的对象。

`handler`是一个让我们截取对象和改变对象的操作。

`proxy`是否代理返回。

而`revoke`是一个让我们撤销对代理的访问的函数。

# 元对象

JavaScript 对象有隐藏的属性，让我们可以获取对象。

有一个方法可以让我们获取对象的属性并返回它。

它们可以捕获 get 和 call 操作。

# 为代理实施不变量

我们可以用代理来实现各种不变量。

我们可以做的一件事是禁用对象的可扩展性。

我们可以使用`Object.preventExtensions`方法来禁止向对象添加属性。

例如，我们可以写:

```
'use strict'
const obj = Object.preventExtensions({});
obj.bar = 'foo';
```

来阻止我们给一个对象添加属性。

如果严格模式为 on，则当我们尝试向不可扩展的对象添加属性时，会出现错误“未捕获类型错误:无法添加属性栏，对象不可扩展”。

我们可以通过编写以下代码来检查对象是否可扩展:

```
console.log(Object.isExtensible(obj));
```

如果它返回`true`，那么我们可以向它添加属性。

![](img/f7fbd6a6dea2a4cf4290d24d45d468af.png)

由[路易·雷诺](https://unsplash.com/@louisrdn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用代理做各种事情，比如控制对一个对象的访问等等。

对于其他操作，我们不需要代理。`Object`构造函数有静态方法让我们控制对象操作。