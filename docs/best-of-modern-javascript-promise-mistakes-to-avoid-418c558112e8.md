# 现代 JavaScript 的精华——承诺避免错误

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-promise-mistakes-to-avoid-418c558112e8?source=collection_archive---------9----------------------->

![](img/a96b353e661f6c3f1d269ee5fd69aa67.png)

马修·兰卡斯特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看我们应该避免的 JavaScript promise 错误。

# 链接和错误

我们可以在一个链中有一个或多个`then`调用。

如果我们有多个没有错误处理程序的`then`调用，那么错误会一直传递下去，直到出现错误处理程序。

所以如果我们有:

```
asyncFunc1()
  .then(asyncFunc2)
  .then(asyncFunc3)
  .catch(function(reason) {
    // ...
  });
```

然后，如果链中的任何承诺有错误，就会调用`catch`。

# 常见错误

一个承诺链可以通过一系列的`then`调用来构建。

我们出错的一个原因是我们返回了最初的承诺，而不是从`then`返回的承诺。

例如，我们不应该写:

```
function foo() {
  const promise = promiseFunc();
  promise.then(result => {
    //....
  }); return promise;
}
```

相反，我们应该写:

```
function foo() {
  const promise = promiseFunc();
  return promise.then(result => {
    //...
  });
}
```

我们必须返回承诺链，而不是原来的承诺。

# 嵌套承诺

我们不应该重复承诺。使用承诺的全部意义在于避免嵌套。

例如，如果我们有这样的东西:

```
promiseFunc1()
  .then(result1 => {
    promiseFunc2()
      .then(result2 => {
        //...
      });
  });
```

我们应该把它改写成:

```
promiseFunc1()
  .then(result1 => {
    return promiseFunc2();
  })
  .then(result2 => {
    //...
  });
```

# 创造承诺而不是连锁

我们不应该创造承诺，而应该建立联系。

所以`Promise`构造函数没有创建承诺:

```
function insert(entry) {
  return new Promise((resolve, reject) => {
    db.insert(entry)
      .then(resultCode => {
        //...
        resolve(resultCode);
      }).catch(err => {
        reject(err);
      })
  });
}
```

我们只是返回承诺链:

```
function insert(entry) {
  return db.insert(entry)
    .then(resultCode => {
      //...
    }).catch(err => {
      handleError(err);
    })
}
```

我们不应该做的是使用`Promise`构造函数创建一个新的承诺。

`then`已经返回了一个承诺，所以我们不需要`Promise`构造函数。

# 使用`then()`进行错误处理

`then`不宜用于递。

`catch(callback)`是`then(null, callback)`的简称。

但是，在同一行中使用是有问题的。

`then`方法中的错误回调并不捕捉在被实现回调拒绝时发生的错误。

例如，如果我们有:

```
promiseFunc1()
  .then(
    value => {
      fooo();
      return promiseFunc2();
    },
    error => {
      //...
    });
```

如果在第一个`then`回调中出现错误，第二个回调将不会捕捉到它。

# 错误处理

可能会出现两种错误。

当一个正确的程序遇到异常情况时，就会出现操作错误。

例如，如果有人输入了无效格式的内容，那就是操作错误。

程序员错误是我们程序中不正确的行为。

例如，我们可能将一个数组传递给一个函数，而它实际上需要一个字符串，那么这就是程序员的错误。

为了处理操作错误，我们应该抛出异常或者调用拒绝。

如果是程序员错误，那么程序应该会很快失败。

我们可以抛出一个异常来表明程序失败了。

![](img/e2a2aff3db530b2cc5868f93ca17ae5e.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

有许多常见的错误，我们应该避免承诺。