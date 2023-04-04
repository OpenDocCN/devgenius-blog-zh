# JavaScript 最佳实践—传播、休息和承诺

> 原文：<https://blog.devgenius.io/javascript-best-practices-spread-rest-and-promises-b7b0663ef5c7?source=collection_archive---------22----------------------->

![](img/1a8fec408667e2aea1064f05455a412d.png)

克里斯·莱佩尔特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 用正则表达式文字代替`RegExp`构造函数的使用

正则表达式文字比使用`RegExp`构造函数要短得多。

所以我们应该用它们来支持构造函数。

例如，不写:

```
new RegExp("^\\d\\.$");
```

我们写道:

```
/^\\d\\.$"/
```

# 使用 Rest 参数代替`arguments`

`argyments`对象不支持箭头函数，也不是数组。

Rest 参数返回一个参数数组。

所以我们应该用那个来代替。

例如，不写:

```
function foo() {
  console.log(arguments);
}
```

我们写道:

```
function foo(...args) {
  console.log(args);
}
```

# 使用扩展语法代替`.apply()`

我们应该使用 spread 语法而不是`apply`。

例如，不写:

```
const args = [1, 2, 3, 100, 200];
Math.max.apply(Math, args);
```

我们写道:

```
const args = [1, 2, 3, 100, 200];
Math.max(...args);
```

我们不需要`apply`，除非我们想改变`this`的值。

# 使用模板文本而不是字符串串联

我们应该使用模板文字而不是字符串连接。

在模板文字中包含表达式要容易得多。

例如，不写:

```
const str = "hello, " + name + "!";
```

我们写道:

```
const str = `hello, ${name}!`;
```

# 返回每个`then()`内部，创建可读和可重用的承诺链

如果我们想在承诺上调用`then`，我们应该返回一些东西。

例如，不写:

```
myPromise.then(() => {
  doSomething()
})
```

我们写道:

```
myPromise.then(() => {
  return doSomething()
})
```

# 对未退回的承诺使用`catch()`

我们应该在未返回的承诺上使用`catch`来捕捉它们发生的任何错误。

例如，我们可以写:

```
myPromise
  .then(doSomething)
  .then(doMore)
  .catch(errors)
```

其中`errors`是一个回调函数，用于捕获承诺中出现的任何错误。

# 确保在 ES5 环境中使用之前创建一个`Promise`构造函数

`Promise`构造函数是在 ES6 中引入的。

所以任何用旧版本 ES 编写的代码都必须使用 promise 库来创建承诺。

例如，不写:

```
const x = Promise.resolve('foo');
```

我们写道:

```
const Promise = require('bluebird');
const x = Promise.resolve('foo');
```

# 没有嵌套的`then()`或`catch() S`语句

我们应该避免嵌套的`then`或`catch`语句。

例如，不写:

```
myPromise.then(val =>
  doWork(val).then(doMore)
)
```

我们写道:

```
myPromise
  .then(doWork)
  .then(doMore)
  .catch(errors)
```

# 避免在 Promise 静态方法上使用`new`

如果我们正在调用`Promise`静态方法，那么我们不应该'；使用`new`关键字。

例如，不写:

```
new Promise.resolve(value)
new Promise.reject(error)
new Promise.race([foo, bar])
new Promise.all([foo, bar])
```

我们写道:

```
Promise.resolve(value)
Promise.reject(error)
Promise.race([foo, bar])
Promise.all([foo, bar])
```

# `finally()`中没有退货单

`finally`中的`return`语句是无用的，因为没有什么会消耗结果。

因此，与其写:

```
myPromise.finally((val) => {
  return val
})
```

我们写道:

```
myPromise.finally((val) => {
  console.log(val)
})
```

# 不需要时，`Promise.resolve`或`Promise.reject`中没有包装值

当不需要值时，我们不应该将它们包装在`Promise.resolve`或`Promis.reject`中。

例如，不写:

```
myPromise.then((val) => {
  return Promise.resolve(val * 3);
})
myPromise.then((val) => {
  return Promise.reject('foo');
})
```

我们写道:

```
myPromise.then((val) => {
  return val * 3
})
myPromise.then((val) => {
  throw 'foo'
})
```

`return`将返回一个承诺，其中解析值为返回值。

`throw`会拒绝给定值的承诺。

# 创建新承诺时保持一致的参数名称

在创建新的承诺时，我们应该有一致的参数名。

这样，他们做什么就不会有任何混淆。

例如，不写:

```
new Promise((reject, resolve) => { ... })
```

其功能顺序错误，或者:

```
new Promise((ok, fail) => { ... })
```

有不标准的名字，我们写:

```
new Promise((resolve, reject) => { ... })
```

# 与回调模式相比，更喜欢异步/等待模式

`async`和`await`比回调模式短，所以我们可以起诉。

例如，不写:

```
myPromise
  .then((val) => {
    return foo();
  })
  .then((val) => {
    return bar();
  })
  .then((val) => {
    return val;
  })
```

我们写道:

```
const foo = async () => {
  const val1 = await myPromise;
  const val2 = await foo();
  const val3 = await bar();
  return va3;
}
```

![](img/0357297c1e0b7402a7c144d7ffb95355.png)

克里斯蒂娜·安妮·科斯特洛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们应该用 spread 和 rest 代替`apply`和`arguments`。

承诺可以用很多方法来清理。