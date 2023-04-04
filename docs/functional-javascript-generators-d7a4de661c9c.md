# 函数式 JavaScript —生成器

> 原文：<https://blog.devgenius.io/functional-javascript-generators-d7a4de661c9c?source=collection_archive---------5----------------------->

![](img/9bd4dcd6dbd772020b58d5b0fcb555bc.png)

由[杰森·黑眼](https://unsplash.com/@jeisblack?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是一种函数式语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将研究 JavaScript 生成器。

# 回调地狱

如果我们有些函数像:

```
let sync = () => {
  //..
}
let sync2 = () => {
  //...
}
let sync3 = () => {
  //...
}
```

并且每个函数都是同步的，那么我们可以一个一个地调用它，用我们的方式组合它们。

但是如果它们是异步的，那么我们就不能逐个调用。

异步函数可能有一个回调函数，让我们在得到结果时调用这个回调函数:

```
let async1 = (fn) => {
  //...
  fn( /* result data */ )
}
let async2 = (fn) => {
  //...
  fn( /* result data */ )
}
let async3 = (fn) => {
  //...
  fn( /* result data */ )
}
```

如果我们想按顺序调用它们，我们必须这样写:

```
async1(function(x) {
  async2(function(y) {
    async3(function(z) {
      //...
    });
  });
});
```

正如我们所看到的，我们的代码中有很多嵌套。

我们得把这个弄干净。

# 发电机

我们可以用生成器清理这些代码。

要创建一个生成器函数，我们可以写:

```
function* gen() {
  return 'foo';
}
```

生成器函数由关键字`function*`表示。

然后我们可以调用它来创建一个生成器:

```
let generator = gen();
```

`generatorResult`对象有`next`方法来返回我们生成的值。

返回的对象具有`value`和`done`属性。

所以我们可以写:

```
generator.next().value
```

我们得到了`'foo'`。

第二次调用`next`将返回一个`value`为`undefined`的对象。

`yield`关键字是一个新的关键字，它将为`next`方法带来价值。

每个`yield`语句的值将按照它们被列出的顺序返回。

`yield`暂停函数的执行，并将结果发送回调用者。

然后下次调用`next`时，运行`next` yield 语句。

所以如果我们有:

```
function* gen() {
  yield 'first';
  yield 'second';
  yield 'third';
}let generator = gen();
```

第一个`generator.next()`调用返回:

```
{value: "first", done: false}
```

然后第二次呼叫返回:

```
{value: "second", done: false}
```

第三次呼叫返回:

```
{value: "third", done: false}
```

第四次呼叫返回:

```
{value: undefined, done: true}
```

这表明生成器没有更多的值要返回。

`done`属性表示生成器是否已经返回了所有的值。

一旦`done`是`true`，我们就知道什么时候停止调用`next`。

# 将数据传递给生成器

我们还可以将数据传递给生成器。

为了做到这一点，我们创建了一个带有`yield`语句的生成器函数，该语句后面没有值。

例如，我们可以写:

```
function* fullName() {
  const firstName = yield;
  const lastName = yield;
  console.log(`${firstName} ${lastName}`);
}
```

我们有一个后面没有值的`yield`语句，所以它将接受我们传递给`next`的值。

然后我们可以通过写来使用它:

```
const fn = fullName();
fn.next()
fn.next('mary')
fn.next('jones')
```

我们创建了生成器函数。

然后我们叫`next`来启动发电机。

一旦我们这样做了，我们就可以开始向生成器传递值了。

我们称之为:

```
fn.next('mary')
```

向第一个`yield`语句传递一个值。

然后我们对第二个做同样的事情:

```
fn.next('jones')
```

一旦我们这样做了，我们从控制台日志中得到`'mary jones'`。

# 异步代码和生成器

`async`和`await`语法是基于生成器创建的。

例如，我们可以这样使用它:

```
const getData = async () => {
  const res = await fetch('https://api.agify.io/?name=michael')
  const data = await res.json();
  console.log(data);
}
```

我们有`async`和`await`语法。

`await`暂停`getData`的执行，直到结果出现。

所以它的作用类似于`yield`。

现在我们逐行运行我们的异步代码。

唯一的区别是`await`只对承诺起作用。

![](img/c906fa4a66cc865040b7aa5401f40278.png)

照片由[卢卡·布拉沃](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用生成器按顺序返回项目。

返回一个结果后，该函数暂停，并在请求下一个结果时恢复。

承诺的`async`和`await`语法基于生成器语法。