# 最佳现代 JavaScript——承诺

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-promises-b85eb7698b66?source=collection_archive---------3----------------------->

![](img/92a2789c9502e4e07b7a4dfa4f86814b.png)

照片由[莎伦·麦卡琴](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 承诺。

# 承诺的好处

承诺是连锁的。

它们看起来类似于同步代码。

例如，我们可以写:

```
asyncFunction(arg)
  .then(result1 => {
    console.log(result1);
    return asyncFunction2(x, y);
  })
  .then(result2 => {
    console.log(result2);
  });
```

我们在`then`回调中返回一个承诺，让我们再次调用`then`。

链接也很简单，因为我们只需要打电话给`then`，直到我们完成了我们想要的承诺。

编写异步调用更容易，因为我们可以做链接。

可以使用`catch`方法进行错误处理。

例如，我们可以写:

```
asyncFunction(arg)
  .then(result1 => {
    console.log(result1);
    return asyncFunction2(x, y);
  })
  .then(result2 => {
    console.log(result2);
  })
  .catch(error => {
    console.log(error);
  })
```

`catch`方法接受一个带有`error`参数的回调。

所以我们可以在回调的时候做任何我们想做的事情。

函数签名很清楚，我们可以很容易地看到参数。

Promises 也是编写异步代码的标准方式。

在 promises 作为原生特性引入 ES6 之前，有许多库实现了同样的功能。

但现在，ES6 使承诺成为标准，所以我们可以在任何地方使用它。

# 创造承诺

为了创建一个承诺，我们可以使用`Promise`构造函数。

例如，我们可以写:

```
new Promise((resolve, reject) => {
  setTimeout(() => resolve('DONE'), 100);
});
```

我们使用带有带参数`resolve`和`reject`的函数的构造函数。

`resolve`是一个函数，当它成功时被调用来返回值。

`reject`是一个被调用来返回承诺失败原因的函数。

承诺是价值的容器和事件的发射器。

Promises 看起来像同步代码，但它们是非阻塞的。

他们不能同步返回任何东西。

ES6 为我们引入了一种用生成器挂起和恢复代码的方法。

因此，这被用作 JavaScript 中承诺和异步函数的基础。

例如，如果我们有:"

```
async function main() {
  const x = await asyncFunc();
  console.log(x);
  //...
}
main();
```

`asyncFunc`函数调用后返回一个承诺并运行控制台日志。

`await`就像 JavaScript 生成器函数中的`yield`。

它做同样的事情。它暂停代码，然后在检索结果时继续运行。

# 利用承诺

承诺有三种状态。

它们可以是待定、已实现或已拒绝。

待定意味着结果还没有计算出来。

已实现表示结果计算成功。

拒绝表示计算过程中出现故障。

我们传递给`Promise`构造函数的参数称为执行器。

除了调用`reject`使一个承诺失败，我们还可以用异常拒绝一个承诺。

# 履行诺言

为了消费一个承诺，我们可以调用`then`来从一个承诺中获得解决的结果。

我们传递给`then`的回调接受一个承诺的解析结果。

`catch`接受一个回调，该回调接受我们传递给`reject`的参数。

例如，我们可以写:

```
promise
  .then(value => {
    //...
  })
  .catch(error => {
    //...
  });
```

`value`具有`promise`的解析值。

`error`具有被拒绝的值。

`then`也可以带 2 个参数。

一个是已解决的回调，第二个参数是被拒绝的回调。

所以我们也可以通过写来捕捉错误:

```
promise.then(
  null,
  error => {
    //...
  });
```

第二个参数与`catch`回调相同，捕捉`promise`的错误。

![](img/8469d95c3b9f32a688160e38444a3381.png)

照片由[蒂姆·莫斯霍尔德](https://unsplash.com/@timmossholder?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

承诺有很多好处，我们可以用标准的方式创造和消费它们。