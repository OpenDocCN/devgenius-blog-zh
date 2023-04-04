# 面向对象的 JavaScript —继承和复制对象

> 原文：<https://blog.devgenius.io/object-oriented-javascript-inheritance-and-copying-objects-439bbba1a6aa?source=collection_archive---------5----------------------->

![](img/68c1c8bafaeebc77eac521ee2711e81e.png)

照片由[路易斯·汉瑟@shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看复制对象。

# 深层拷贝

我们可以通过将对象的属性从源对象递归复制到目标对象来深度复制对象。

例如，我们可以写:

```
function deepCopy(source, target = {}) {
  for (const key in source) {
    if (source.hasOwnProperty(key)) {
      if (typeof source[key] === 'object') {
        target[key] = Array.isArray(source[key]) ? [] : {};
        deepCopy(source[key], target[key]);
      } else {
        target[key] = source[key];
      }
    }
  }
  return target;
}
```

我们循环通过`source`的每个键。

然后我们检查每一处房产是否是自有房产。

然后我们检查`source[key]`对象是否是一个对象。

如果`source[key]`是一个数组，那么我们创建一个数组，然后进行复制。

如果它是一个对象，那么我们递归地调用`deepCopy`进行复制。

否则，我们将来自`source`的值赋给`target`。

一旦这些都完成了，我们就返回`target`。

然后我们可以通过写来使用它:

```
const foo = {
  a: 1,
  b: {
    c: 2
  }
}
const bar = deepCopy(foo, {});console.log(bar);
```

`deepCopy`将把所有自己的属性从`foo`递归复制到`bar`。

所以`bar`是:

```
{
  "a": 1,
  "b": {
    "c": 2
  }
}
```

`Array.isArray`不管上下文如何，让我们检查某个东西是否是数组。

# 使用 object()方法

我们可以创建一个`object`函数来创建一个返回构造函数实例的函数。

这个建议来自道格拉斯·克洛克福特，它使得设置原型变得容易，因为它采用了原型对象。

例如，我们可以写:

```
function object(proto) {
  function F() {}
  F.prototype = proto;
  return new F();
}
```

我们用`proto`属性创建了`object`函数。

然后我们将它设置为`F`的`prototype`。

我们返回一个`F`的实例。

这与`Object.create`不同，因为我们有一个构造函数的实例。

然后我们可以通过书写来使用它:

```
const obj = object({
  foo: 1
});
```

那么`obj`将从我们传入的对象中继承。

# 混合原型继承和复制属性

我们可以混合原型继承和复制属性。

为此，我们可以通过编写以下代码来扩展`object`函数:

```
function object(proto, moreProps) {
  function F() {}
  F.prototype = proto;
  const f = new F();
  return {
    f,
    ...moreProps
  };
}const obj = object({
  foo: 1
}, {
  bar: 2
});
```

我们向`object`函数添加了一个`moreProps`参数，这允许我们通过向它传递第二个参数来添加更多的属性。

`moreProps`是传入我们返回的对象，以便我们返回新的对象。

因此，`obj`是`{f: F, bar: 2}`，因为我们继承了`F.prototype`和`moreProps`。

![](img/1b043ac626d5510987811f44321bed6f.png)

照片由 [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以将原型继承与复制属性相结合，以获得我们想要的对象的所有属性。

深度复制可以通过将属性从源对象递归复制到目标对象来完成。