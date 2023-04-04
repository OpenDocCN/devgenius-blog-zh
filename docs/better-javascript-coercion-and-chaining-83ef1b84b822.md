# 更好的 JavaScript——强制和链接

> 原文：<https://blog.devgenius.io/better-javascript-coercion-and-chaining-83ef1b84b822?source=collection_archive---------8----------------------->

![](img/e1c8e15dfa23729f3e58e580ac49c955.png)

[JJ 英](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 没有过度的胁迫

在我们的应用程序中不应该有过多的数据类型强制。

JavaScript 对类型非常宽松，所以一直在强制数据类型。

强制可能很方便，但是很容易产生意想不到的结果，因为规则很难记住。

当我们使用重载的函数签名时，强制是令人困惑的。

所以如果我们有:

```
function foo(x) {
  x = Number(x);
  if (typeof x === "number") {
    //...
  } else {
    //...
  }
};
```

在运行其余代码之前，我们将`x`转换为一个数字，因此`x`的类型始终是一个数字。

在强制之前`else`块永远不会被执行。

如果我们通过检查参数的类型来重载一个函数，那么我们不应该强制数据类型。

否则，我们移除 if-else 块，因为我们不需要它们。

相反，我们可以写:

```
function foo(x) {
  if (typeof x === "number") {
    //...
  } else if (typeof x === "object" && x !== null) {
    //...
  }
};
```

在块内运行代码之前，检查`x`的数据类型。

我们可以添加带函数的类型保护。

例如，我们可以写:

```
const guard = {
  guard(x) {
    if (!this.test(x)) {
      throw new TypeError("unexpected type");
    }
  }
};
```

我们用一些`test`的方法来检查类型。

当返回`false`时，我们抛出一个错误。

其他对象可以使用`guard`对象作为其原型，并自己实现`test`方法来进行类型检查。

要用给定的原型创建一个对象，我们可以对它调用`Object.create`:

```
const obj = Object.create(guard);
```

# 支持方法链接

方法链使得在一个语句中进行多个操作变得容易。

例如，对于一个字符串，我们可以多次调用`replace`:

```
str.replace(/&/g, "&amp;")
   .replace(/</g, "&lt;")
   .replace(/>/g, "&gt;")
```

这是因为`replace`返回一个替换完成的字符串。

消除临时变量会使代码更容易阅读，因为这样会减少干扰。

我们可以通过返回`this`用自己的方法做到这一点。

例如，我们可以写:

```
class Element {
  setBackgroundColor(color) {
    //...
    return this;
  } setColor(color) {
    //...
    return this;
  } setFontWeight(fontweight) {
    //...
    return this;
  }
}
```

使用返回自身实例的方法创建类。

这样，我们可以链接每个方法并获得最新的结果。

例如，我们可以这样使用它:

```
const element = new Element();
element.setBackgroundColor("blue")
  .setColor("green")
  .setFontWeight("bold");
```

这使得我们的生活更容易，因为我们不需要任何中间变量来获得最终结果。

其他让我们改变方法的流行方法包括一些数组方法和 jQuery 方法。

![](img/f468e952f30b29fb17c86d42168a44dc.png)

斯蒂芬·莱昂纳迪在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们不应该在不需要的时候强制数据类型。

此外，使方法可链接对每个人来说也更容易。