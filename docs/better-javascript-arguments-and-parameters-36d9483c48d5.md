# 更好的 JavaScript——自变量和参数

> 原文：<https://blog.devgenius.io/better-javascript-arguments-and-parameters-36d9483c48d5?source=collection_archive---------7----------------------->

![](img/eb9b3ff480da255b6c1b06f6f2244983.png)

由 [Sarah Kilian](https://unsplash.com/@rojekilian?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 可选参数

函数的参数可以是 JavaScript 中的任何东西。

我们可以有可选的参数。

如果它们没有被传入函数，那么它们就是`undefined`。

我们可以为参数提供一个默认值，可能是`undefined`。

例如，我们可以写:

```
function f(x = 1) {
  return x;
}
```

然后`x`的默认值为 1。

如果没有传入值，那么它就是 1。

# 接受关键字参数的选项对象

如果我们有一个函数有很多参数，那么很难记住参数是什么和它们的类型。

因此，使用的参数越多，函数就越难使用。

如果我们有很多参数，那么我们应该把它们组合成一个对象参数。

然后我们可以把对象参数析构成变量。

例如，如果我们有:

```
const alert = new Alert(100, 75, 300, 200,
  "Error", message,
  "green", "white", "black",
  "error", true);
```

那就很难记住了。

相反，我们应该把它们放入一个对象中:

```
const alert = new Alert({
  x: 100,
  y: 75,
  width: 300,
  height: 200,
  title: "Error",
  message: message,
  titleColor: "green",
  bgColor: "white",
  textColor: "black",
  icon: "error",
  modal: true
});
```

因为我们有属性键，所以很容易知道参数的意思。

然后在`alert`构造函数中，我们可以析构参数:

```
function Alert({
  x,
  y,
  width,
  height,
  title,
  message,
  titleColor,
  bgColor,
  textColor,
  icon,
  modal
}) {
  //...
}
```

我们得到了与参数相同的好处，并通过析构得到了作为变量的属性值。

通过对象参数，每个属性的含义都很清楚。

我们也不用担心订单的问题。

选项对象由可选参数组成，因此我们可以省略对象中的所有内容。

但是如果我们需要某些东西，我们可以把它们从对象参数中分离出来。

所以我们可以写:

```
function Alert(
  title,
  message, {
    x,
    y,
    width,
    height,
    titleColor,
    bgColor,
    textColor,
    icon,
    modal
  }) {
  //...
}
```

如果我们想为它们提供默认值，我们可以通过为属性分配默认值来轻松实现。

例如，我们可以写:

```
function Alert({
  x = 100,
  y = 100,
  width = 300,
  height = 300,
  title,
  message,
  titleColor,
  bgColor,
  textColor,
  icon,
  modal
}) {
  //...
}
```

设置参数的默认值。

`x`、`y`、`width`和`height`有默认值。

我们不必检查`undefined`或者使用`||`操作符来提供默认值。

![](img/4c44fd98c7ec619b747d9211da40375f.png)

照片由[cloudvisual.co.uk](https://unsplash.com/@cloudvisual?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

如果函数中有很多参数，那么我们应该使用一个对象参数。

我们可以给它们赋值，然后把它们析构成变量。

可选参数可以分配给形参。