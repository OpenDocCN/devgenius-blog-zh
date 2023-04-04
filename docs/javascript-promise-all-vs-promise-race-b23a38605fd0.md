# JavaScript promise . all vs promise . race

> 原文：<https://blog.devgenius.io/javascript-promise-all-vs-promise-race-b23a38605fd0?source=collection_archive---------8----------------------->

# **承诺简介**

想象一下，你是一个著名的作家，你的粉丝正等着看你的新书。为了得到一些安慰，你答应出版后给他们寄一本。他们填写了一个表格，填写了他们的递送地址，这样副本就会被发送给他们。如果出现任何问题，他们仍然会得到通知。这是一个现实生活中诺言的类比。

承诺用于处理 Javascript 中的异步操作。当一个承诺被创建时，生产代码运行，最终产生结果。在 Javascript 中，为了更好地避免将回调函数传递给函数，从而导致代码难以管理和阅读，可以使用 promises。

# Promise 语法和属性

Javascript promise 对象可以被实现、拒绝或挂起。如果满足，结果是一个值，如果拒绝，结果是一个错误对象，如果挂起，结果是未定义的。

可以使用 Promise 构造函数创建 Promise，它有一个参数，一个回调函数有两个参数:resolve 和 reject。当回调函数执行时，如果一切顺利，它将调用 resolve 如果没有达到预期的操作，它将拒绝。

```
var promise = new Promise((myResolve, myReject) => {
// Code that takes some time

  myResolve(); // call this when success
  myReject();  // call this when error
});

// Code that waits the fulfillment of Promise
myPromise.then(
  function(value) // if successful 
  function(error) // if error 
);
```

# 承诺.所有 vs 承诺.比赛

Javascript 中的 Promise 对象提供了内置的方法，比如 Promise.all 和 Promise.race。

**Promise.all** 接受一个可重复的承诺作为输入，并在尝试完成所有**承诺后返回输入承诺结果的单个承诺。如果这些输入承诺中的一个被拒绝，它将退出。想象一下，访问你的博客页面，必须满足几个要求，如查看你的内容，你保存的博客和与导航条互动。如果任何这些东西没有正确显示，你将被重定向到其他地方。由于所有这些操作对于用户与站点的交互都至关重要，所以可以使用 Promise.all，在这种情况下，当其中一个功能失败时，就会被拒绝。**

```
const promise1 = Promise.resolve(3);
const promise2 = 22;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
});Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
// expected output: Array [3, 22, "foo"]
```

**Promise.race** 接受一个可迭代对象，比如一个承诺数组和一个待定承诺的返回值。就像名字一样，那些承诺将会相互竞争。该方法将返回解决或拒绝的第**个**已解决的承诺，并在此之后退出。在下面的示例中，promise2 执行得更快，因此它将首先被解析。

```
const promise1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'one');
});const promise2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'two');
});Promise.race([promise1, promise2]).then((value) => {
  console.log(value);

});
// expected output: "two"
```

Promise.all 不能处理部分失败，如果其中一个承诺被拒绝，则整个流程都存在失败回调。在返回一个已解决的承诺之前，race 不会等待所有的承诺都被解决。选择哪一个取决于我们需要完成什么。

资源:

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race) [## Promise.race() - JavaScript | MDN

### Promise.race()方法返回一个承诺，只要 iterable 中的一个承诺满足或拒绝…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race) [](https://alligator.io/js/promise-all-promise-race/#:~:text=need%20to%20accomplish.-,Promise.,to%20fulfill%20all%20of%20them.&text=race%20also%20accepts%20an%20array,either%20be%20resolved%20or%20rejected) [## JavaScript 中 Promise.all 和 Promise.race 的区别

### JavaScript 中的 Promise 对象提供了一些有用的内置方法，Promise.all 和 Promise.race 就是其中的两个…

鳄鱼. io](https://alligator.io/js/promise-all-promise-race/#:~:text=need%20to%20accomplish.-,Promise.,to%20fulfill%20all%20of%20them.&text=race%20also%20accepts%20an%20array,either%20be%20resolved%20or%20rejected)