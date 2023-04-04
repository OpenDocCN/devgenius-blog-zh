# 更好的 JavaScript——高阶函数和调用

> 原文：<https://blog.devgenius.io/better-javascript-higher-order-functions-and-call-f2372ccdfa5f?source=collection_archive---------3----------------------->

![](img/06fbdaa7121e93261f259d7528015153.png)

阿灵顿研究公司在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 我们可以用高阶函数做的事情

我们可以用高阶函数做各种事情。

他们让我们简化重复的代码。

例如，不是编写一个循环来遍历一个数组并将条目推送到另一个数组。

而不是写:

```
const names = ["foo", "bar", 'baz'];
const upper = [];
for (let i = 0; i <  names.length; i++) {
  upper[i] = names[i].toUpperCase();
}
```

我们编写使用`map`方法做同样的事情:

```
const names = ["foo", "bar", 'baz'];
const upper = names.map((name) => name.toUpperCase());
```

我们用回调函数调用了`map`,以大写形式返回每个条目。

要短得多，简单得多。

我们可以创建自己的高阶函数，它接受回调，并通过回调函数来做我们想要的事情。

例如，我们可以写:

```
const buildString = (n, callback) => {
  let result = "";
  for (let i = 0; i < n; i++) {
    result += callback(Math.floor(Math.random() * 100) + i);
  }
  return result;
}const str = buildString(8, String.fromCharCode);
```

我们已经创建了接受`callback`函数的`buildString`函数。

在函数中，我们创建了一个`for`循环，并在循环的每次迭代中调用`callback`。

用字符代码调用 is，让我们得到字符。

然后我们可以通过传入一个长度值和`String.fromCharCode`来调用它。

那么`str`就是`'BO@B1&g’`。

高阶函数让我们只需一次就能修复任何错误，因为没有重复的代码。

在我们的程序中没有代码到处传播。

# 使用自定义接收器调用方法

我们可以在函数上使用`call`方法，让我们设置`this`的值和我们用函数调用的参数。

例如，如果我们有:

```
f.call(obj, arg1, arg2, arg3);
```

那就像打电话:

```
f.call(arg1, arg2, arg3);
```

将`f`中的`this`设置为`obj`。

对于调用可能已经被移除、修改或覆盖的方法来说，`call`方法非常方便。

例如，`hasOwnProperty`可以被任何对象覆盖。

我们可以设置:

```
obj.hasOwnProperty = 1;
```

那么如果我们写:

```
obj.hasOwnProperty("foo");
```

我们会得到一个错误，因为`hasOwnProperty`已经被设置为 1。

调用`hasOwnProperty`的更好方法是使用`call`方法。

例如，我们可以写:

```
const dict = {};
dict.foo = 1;
delete dict.hasOwnProperty;
({}).hasOwnProperty.call(dict, "foo");
```

我们用`foo`属性创建了`dict`对象。

并且我们从`dict`中删除了`hasOwnProperty`属性。

即使我们这样做了，我们仍然可以通过使用`call`方法用一个空对象调用`hasOwnProperty`。

第一个参数是`this`的值，我们想用它来调用`hasOwnProperty`。

而这个论点就是`hasOwnProperty`的论点。

![](img/7b8af3090d0f3088d1f3fc52270e0b47.png)

照片由 [Hassan OUAJBIR](https://unsplash.com/@hazardos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

高阶函数让我们将逻辑抽象成可重用的部分。

`call`方法让我们用我们想要的任意值`this`调用一个函数。