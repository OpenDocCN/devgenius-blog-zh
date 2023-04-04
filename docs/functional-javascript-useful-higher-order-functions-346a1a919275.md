# 函数式 JavaScript——有用的高阶函数

> 原文：<https://blog.devgenius.io/functional-javascript-useful-higher-order-functions-346a1a919275?source=collection_archive---------9----------------------->

![](img/81c85782b4708fb416600f9c6522db6c.png)

照片由[劳拉·克劳](https://unsplash.com/@laurarain?utm_source=medium&utm_medium=referral)在[un plash](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄

JavaScript 在一定程度上是一种功能性语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将研究如何使用闭包来创建我们自己的高阶函数。

# 一元函数

我们可以创建自己的`unary`函数，该函数返回一个只调用一个参数的函数。

例如，我们可以写:

```
const unary = (fn) =>
  fn.length === 1 ?
  fn :
  (arg) => fn(arg)
```

我们有`fn`这是一个函数。

我们检查`fn`的`length`属性，返回它有多少参数。

如果只有一个参数，则返回`fn`函数。

否则，我们返回一个只有一个参数调用`fn`的函数。

这样，我们就可以通过`map`方法安全地使用类似`parseInt`的功能。

例如，我们可以写:

```
const nums = ['1', '2', '3'].map(unary(parseInt));
```

我们用`parseInt`调用`unary`函数，这样我们就可以用带有 n 个参数的回调函数将字符串解析成一个整数。

`parseInt`取要解析的值，基数作为第二个参数，所以我们不能直接通过`parseInt`来映射，因为基数是数组条目的索引，这不是我们想要的。

对于我们的`unary`函数，我们总是只通过传入的值来调用`parseInt`。

故`nums`为`[1, 2, 3]`。

# 功能一次

我们可以创建一个`once`函数，只调用一次。

例如，我们可以写:

```
const once = (fn) => {
  let done = false;
  return function(...args) {
    if (!done) {
      done = true;
      fn.apply(this, args);
    }
  }
}
```

我们有`once`函数，它只运行`done`为`false`时传入的`fn`函数。

如果是这样的话，那么我们将`done`设置为`true`并用`apply`调用`fn`。

`apply`将`this`值作为第一个参数，将参数数组作为第二个参数。

然后我们可以用它来调用一个函数，只需写:

```
const foo = once(() => {
  console.log("foo")
})foo();
foo();
```

我们用一个我们只想调用一次的函数来调用`once`函数。

然后我们可以调用它两次，看到`'foo'`只被记录了一次。

# 记忆

我们可以通过创建自己的`memoize`函数来缓存函数调用的返回值。

例如，我们可以写:

```
const memoize = (fn) => {
  const cache = {};
  return (arg) => {
    if (!cache[arg]) {
      cache[arg] = fn(arg);
    }
    return cache[arg]
  }
}
```

我们创建了一个接受`fn`函数的`memoize`函数。

在函数中，我们定义了`cache`对象。

我们返回一个带`arg`参数的函数。

我们检查`cache[arg]`是否在缓存中。

如果不是，那么我们在`cache`对象中设置值。

然后我们返回缓存的值。

然后我们可以通过写来使用它:

```
let fastFib = memoize((n) => {
  if (n === 0 || n === 1) {
    return 1;
  }
  return n + fastFib(n - 1);
});
```

给定`n`的值，我们传递我们的 Fibonacci 函数来返回 Fibonacci 数。

这要快得多，因为我们可以从`cache`对象中查找过去计算的值。

![](img/8da719c25a6c5b40cc975a6e3cd2df7b.png)

照片由 [Justus Menke](https://unsplash.com/@justusmenke?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以创建自己的高阶函数来用它做各种事情。