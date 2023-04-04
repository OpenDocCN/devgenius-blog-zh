# 现代 JavaScript 的精华——生成器方法

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-generator-methods-e4eee6e002bb?source=collection_archive---------4----------------------->

![](img/7c88dade9190abcc91f62b5f8f5392e5.png)

[Jason Blackeye](https://unsplash.com/@jeisblack?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 生成器。

# 第`next()`

`next`调用启动一个观察器。

第一次调用`next`的唯一目的是启动观察器。

因此，如果我们向第一个`next`调用传递一个值，它不会被`yield`获得。

例如，我们可以写:

```
function* genFn() {
  while (true) {
    console.log(yield);
  }
}const gen = genFn();
gen.next('a');
gen.next('b');
gen.next('c');
```

然后:

```
b
c
```

已记录。

`next`的第一个调用将`'a'`提供给生成器，但是由于没有`yield`语句，所以无法接收它。

一旦它运行`yield`，那么就可以接收值。

调用`next`后，返回`yield`的操作数。

返回值将是`undefined`,因为我们没有操作数。

第二次调用`next`将`'b'`送入`next`，由`yield`接收。

这将记录在控制台日志中。

因为`yield`没有操作数，所以`next`再次返回`undefined`值。

因此，我们只能在调用`next`时向`yield`馈送数据。

例如，我们可以写:

```
function* genFn() {
  while (true) {
    console.log(yield);
  }
}const gen = genFn();
gen.next();
gen.next('a');
gen.next('b');
gen.next('c');
```

然后我们得到:

```
a
b
c
```

已登录控制台。

# `yield`捆绑松散

`yield`将它后面的整个表达式视为其操作数。

例如，如果我们有:

```
yield foo + bar + baz;
```

那么它被视为:

```
yield (foo + bar + baz);
```

而不是:

```
(yield foo) + bar + baz;
```

因此，我们必须用括号将我们的`yield`表达式括起来，这样我们可以避免语法错误。

例如，不要写:

```
function* genFn() {
  console.log('yielded: ' + yield);
}
```

我们写道:

```
function* genFn() {
  console.log('yielded: ' + (yield));
}
```

# `return()`和`throw()`

`return`和`throw`的方法与`next`相似。

让我们在`yield`的位置返回一些东西。

`throw`让我们在`yield`的位置抛出一个表达式。

`next`方法在`yield`操作员处暂停。

来自`next`的值被发送到`yield`。

`return`终止发电机。

例如，我们可以写:

```
function* genFn() {
  try {
    console.log('start');
    yield;
  } finally {
    console.log('end');
  }
}const gen = genFn();
gen.next()
gen.return('finished')
```

我们有`genFn`函数返回一个带有给定值的生成器。

`next`方法将运行发电机。

并且`return`结束发电机。

如果我们记录`return`调用的值，我们得到:

```
{value: "finished", done: true}
```

# `throw() lets us Throw Errors`

我们可以用关键字`throw`抛出错误。

例如，我们可以写:

```
function* genFn() {
  try {
    console.log('started');
    yield;
  } catch (error) {
    console.log(error);
  }
}const gen = genFn();
gen.next();
console.log(gen.throw(new Error('error')));
```

我们用生成器函数调用`throw`。

一旦我们调用`next`启动发电机，就会调用`catch`块。

然后我们会看到:

```
Error: error
```

用`catch`区块的控制台日志记录。

并且从`throw`调用返回`{value: undefined, done: true}`。

![](img/d600437e18ba6ce286fe131173ddeaef.png)

[Jerry Zhang](https://unsplash.com/@z734923105?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以调用`next`方法，并使用不带操作数的`yield`操作符。

这将从`next`获取值并返回它。

生成器也有`return`和`throw`方法来结束生成器并抛出错误。