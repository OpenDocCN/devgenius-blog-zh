# 函数式 JavaScript——闭包

> 原文：<https://blog.devgenius.io/functional-javascript-closures-ffa520f13f6f?source=collection_archive---------6----------------------->

![](img/87275c03f1c62b48fa9e0e8803bd4842.png)

照片由[画框哈里拉克](https://unsplash.com/@framemily?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是一种函数式语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将看看如何使用闭包。

# 关闭

闭包是内部函数。

内部函数是函数中的函数。

例如，它是这样的:

```
function outer() {
  function inner() {}
}
```

闭包可以访问 3 个作用域。

它们包括在自己的声明中声明的变量。

此外，他们还可以访问全局变量。

他们可以访问外部函数的变量。

例如，如果我们有:

```
function outer() {
  function inner() {
    let x = 1;
    console.log(x);
  }
  inner();
}
```

然后控制台日志记录 1，因为我们在`inner`函数中有`x`，并且我们在控制台日志的同一个函数中访问它。

在`outer`功能之外`inner`功能将不可见。

我们也可以在`inner`函数中访问全局变量。

例如，如果我们有:

```
let global = "foo";function outer() {
  function inner() {
    let a = 5;
    console.log(global)
  }
  inner()
}
```

然后`'foo'`被记录，因为`inner`可以访问`global`变量。

`inner`可以访问的另一个作用域是`outer`函数的作用域。

例如，我们可以写:

```
function outer() {
  let outer = "outer" function inner() {
    let a = 5;
    console.log(outer);
  }
  inner()
}
```

我们有`outer`变量，我们在`inner`函数中访问它。

# 结束记得它的上下文

闭包会记住它的上下文。

所以如果我们在任何地方使用它，函数中的变量就是它们在原始上下文中的样子。

例如，如果我们有:

```
const fn = (arg) => {
  let outer = "outer"
  let innerFn = () => {
    console.log(outer)
    console.log(arg)
  }
  return innerFn;
}
```

那么`outer`和`arg`变量值将是相同的，不管它在哪里被调用。

`outer`是`'outer'`，`arg`是我们传入的任何东西。

由于我们用`fn`返回了`innerFn`，我们可以调用`fn`并将返回的函数赋给一个变量并调用它:

```
const foo = fn('foo');
foo()
```

我们将`'foo'`作为`arg`的值传入。

因此，我们得到:

```
outer
foo
```

从控制台日志中。

我们可以看到，即使我们在`fn`函数之外调用它，值也是一样的。

# 真实世界的例子

我们可以创建自己的`tap`函数来记录调试值。

例如，我们可以写:

```
const tap = (value) =>
  (fn) => {
    typeof(fn) === 'function' && fn(value);
    console.log(value);
  }tap("foo")((it) => console.log('value:', it))
```

创建我们的`tap`函数并调用它。

我们有一个函数，它接受一个`value`，然后返回一个接受函数`fn`的函数，并与控制台日志一起运行。

这样，我们可以传入一个值和一个函数。

然后我们得到:

```
value: foo
foo
```

记录在案。

第一个来自我们传入的回调。

第二个来自我们用`tap`返回的函数。

![](img/01d307e2bd018607f243dc02c8cf1779.png)

[梁杰森](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

闭包是内部函数。

他们可以访问外部函数的作用域、全局变量以及他们自己的作用域。

我们可以使用它或各种应用程序。