# 现代 JavaScript 的精华——写作承诺

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-composing-promises-2aa9a16ee16b?source=collection_archive---------3----------------------->

![](img/03241f72146e585e98a4c47fcacf7869.png)

照片由 [Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究如何处理 JavaScript promise 异常。

# 在基于承诺的函数中处理异常

我们可以在基于承诺的函数中处理异常。

如果在`then`和`catch`回调中抛出异常，那么方法会将它们转换成拒绝。

但是，如果我们在 promise 代码之前运行同步代码，那么整个函数会抛出一个异常。

这可能是我们想要解决的问题。

解决这个问题的一个方法是用 try-catch 包围 synchronous 和 promise 代码。

例如，我们可以写:

```
function foo() {
  try {
    syncFn();
    return asyncFn()
      .then(result => {
        //...
      });
  } catch (err) {
    return Promise.reject(err);
  }
}
```

我们有一个带有 try-catch 块的`foo`函数，它首先运行`syncFn`，然后运行返回承诺的`asyncFn`。

我们在 try 块中返回承诺链。

如果运行了`catch`块，那么我们返回一个被拒绝的承诺。

这让我们始终保持函数异步。

# 在回调中运行同步代码

我们也可以在回调中运行同步代码。

例如，我们可以写:

```
function foo() {
  return asyncFn()
    .then(result => {
      syncFn();
      return asyncFn2();
    })
    .then(result => {
      //...
    });
}
```

如果同步`syncFn`函数在`then`回调中运行并抛出异常，那么返回的承诺将被拒绝。

承诺链的起点也可以在`Promise`构造函数中。

例如，我们可以写:

```
function foo() {
  return new Promise((resolve, reject) => {
      syncFn();
      resolve(asyncFn());
    })
    .then(result => {
      //...
    });
}
```

我们在`Promise`构造函数回调中运行`syncFn`，这样我们就可以运行回调，以便在`syncFn`抛出错误时传播异常。

# 许下承诺

我们可以用不同的方式实现多个承诺。

如果我们想并行运行多个不相关的承诺，我们可以使用`Promise.all`方法。

例如，我们可以写:

```
Promise.all([
    asyncFn1(),
    asyncFn2(),
  ])
  .then(([result1, result2]) => {
    //...
  })
  .catch(err => {
    //...
  });
```

我们传入一组承诺返回函数。

然后我们在`Promise.all`中传递它。

然后我们用一个回调函数调用`then`，这个回调函数包含两个值的解析值的数组。

然后我们调用`catch`来捕捉它们发生的错误。

我们可以用它们遍历已解决的项目。

例如，我们可以写:

```
Promise.all([
    asyncFn1(),
    asyncFn2(),
  ])
  .then((results) => {
    for (const result of results) {
      console.log(result);
    }
  })
  .catch(err => {
    //...
  });
```

我们获取`results`数组，并用 for-of 循环遍历它。

# `Promise.race()`

`Promise.race`是另一种连锁承诺的方法。

它接受一个承诺数组，并返回一个已解决的承诺，该承诺的值是先解决的承诺的值。

例如，我们可以写:

```
Promise.race([
    asyncFn(),
    delay(3000).then(function() {
      throw new Error('timed out')
    })
  ])
  .then(function(text) {
    //...
  })
  .catch(function(reason) {
    //...
  });
```

我们有一系列与`asyncFn`和`delay`的承诺，它们都返回承诺。

然后我们用回调函数调用`then`,从最早解决的承诺中获取已解决的值。

![](img/8588d91382626816558aec5397974a7b.png)

照片由[德尔菲·德拉鲁阿](https://unsplash.com/@delfidelarua7?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用几种方法处理基于承诺的函数的错误。

此外，我们可以用许多不同的方式运行多个承诺。