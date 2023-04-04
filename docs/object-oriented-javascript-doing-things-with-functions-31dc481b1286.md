# 面向对象的 JavaScript——用函数做事

> 原文：<https://blog.devgenius.io/object-oriented-javascript-doing-things-with-functions-31dc481b1286?source=collection_archive---------3----------------------->

![](img/bbe6d5b2248496b82f0725347771719a.png)

[奥比·奥尼耶德](https://unsplash.com/@thenewmalcolm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看函数。

# 运行功能

我们可以通过在名字后面加上括号来运行一个函数。

不管函数是如何定义的，它都能工作。

例如，如果我们有一个函数:

```
const sum = function(a, b) {
  return a + b;
};
```

然后，我们通过编写以下代码来运行它:

```
sum(1, 2);
```

我们传入`sum`期望的数字参数。

# 匿名函数

匿名函数是没有名字的函数。

我们之前的`sum`函数有一个没有给变量赋值的函数。

这样，我们可以在任何地方调用它。

# 回调函数

回调函数是由其他函数调用的函数。

例如，我们可以写:

```
function runAdd(a, b) {
  return a() + b();
}
```

`runAdd`函数调用`a`和`b`函数。

因此，我们必须向它传递两个函数。

我们可以通过书写来使用它:

```
const sum = runAdd(() => 1, () => 2)
```

我们传入了一个返回 1 的函数和一个返回 2 的函数作为参数。

然后我们返回 3。

所以`sum`是 3。

当我们运行异步代码时，回调是有用的。

当异步代码有结果时，我们可以调用回调。

我们也可以用同步回调来做重复的操作，比如用数组方法`map`、`filter`和`reduce`。

我们可以编写自己的`map`方法:

```
function map(arr, callback) {
  const result = []
  for (const a of arr) {
    result.push(callback(a));
  }
  return result;
}
```

然后我们可以通过写来使用它:

```
const result = map([1, 2, 3], (x) => x * 2);
```

那么`result`就是`[2, 4, 6]`。

`map`取一个数组`arr`，用 for-of 循环遍历每个条目，并对每个条目调用`callback`。

# 即时功能

立即函数是定义后立即调用的函数。

例如，我们可以写:

```
(
  function() {
    console.log('foo');
  }
)();
```

然后我们创建了一个函数，并立即调用它。

我们应该用括号把它括起来，这样我们就知道我们在调用它。

我们也可以写:

```
(function() {
  console.log('foo');
})();
```

我们可以得到返回值并写出:

```
const result = (function() {
  //...
  return {
    //...
  }
}());
```

我们在函数中返回一些结果，并将其分配给`result`。

# 内部(私有)函数

函数就像任何其他变量一样，所以我们可以把它们放在另一个函数中。

例如，我们可以写:

```
function outer(param) {
  function inner(a) {
    return a * 100;
  }
  return inner(param);
}
```

我们有一个`inner`函数，返回其乘以 100 的参数。

然后我们返回结果。

拥有内部函数的好处是函数内部的变量不能从外部访问。

但是内部函数可以从外部函数访问变量。

这是一个很大的好处，因为我们不必在全局范围内定义变量。

![](img/22ff43eff6b3c1e444b012a7198eb9f7.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

定义私有函数有很多好处。

我们也可以定义直接调用的函数来返回一些东西。

函数可以是匿名的，也可以是另一个函数的参数。