# 如何访问 JavaScript 承诺的解析值？

> 原文：<https://blog.devgenius.io/how-to-access-the-resolved-value-of-a-javascript-promise-5dadcbaa2a2e?source=collection_archive---------0----------------------->

![](img/f5ff1984eea104d7735d48b629defd1f.png)

照片由 [Gregor Meier](https://unsplash.com/@gregormeier?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在许多情况下，我们希望获得 JavaScript 承诺的解析值。

在本文中，我们将研究如何访问 JavaScript 承诺的解析值。

# 然后用回叫来呼叫

获取 JavaScript 的值解析值的一种方法是用一个有一个参数的回调函数调用`then`方法。

参数具有已解析的值。

例如，我们可以写:

```
Promise.resolve(1)
  .then((result) => {
    console.log(result)
  });
```

我们调用`Promise.resolve`来返回解析为 1 的承诺。

然后我们用一个带有`result`参数的回调函数调用它的`then`。

`result`应该有解决价值的承诺。

因此`result`的值是 1。

# 使用异步和等待语法

我们还可以使用`async`和`await`语法获得承诺的解析值。

例如，我们可以写:

```
(async () => {
  const result = await Promise.resolve(1)
  console.log(result)
})()
```

我们用`await`语法将承诺的解析值赋给`result`变量。

因此，本例中的`result`也应该是 1。

`await`只能在`async`函数中使用。

# 结论

我们可以在`async`函数中使用`await`关键字获取 JavaScript 的解析值，或者使用回调函数调用`then`，该回调函数将解析值作为第一个参数。