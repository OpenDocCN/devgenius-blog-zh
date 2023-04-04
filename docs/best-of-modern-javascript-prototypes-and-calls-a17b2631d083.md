# 现代 JavaScript 精华——原型和调用

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-prototypes-and-calls-a17b2631d083?source=collection_archive---------5----------------------->

![](img/973b1c43fc5e6ea93a3ff5b6d03268e0.png)

乔·什切潘斯卡在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中的原型和方法调用。

# 原型链

JavaScript 对象是一个或多个对象的链。

第一个对象从后面的对象继承属性。

例如，数组的原型链有一个包含数组元素的实例。

`Array.prototytpe`具有由`Array`构造函数提供的属性。

`Object.prototype`具有`Object`构造器提供的属性。

而`null`是链条的末端。

我们可以使用方法的`Object.getPrototype`来得到数组的原型。

例如，我们可以写:

```
const arr = ['a', 'b'];
const proto = Object.getPrototypeOf(arr);
```

然后我们在`proto`变量中看到数组原型的内容。

我们可以看到各种方法、迭代器等等。

我们也可以使用`getOwnPropertyNames`方法来获取原型成员的名字。

我们可以写:

```
const arr = ['a', 'b'];
const p = Object.getOwnPropertyNames(arr);
```

我们得到`[“0”, “1”, “length”]`作为`p`的值。

这些是可以列举的属性。

# 分派的方法调用

如果我们调用一个实例，JavaScript 解释器执行 2 个步骤。

它从原型链中获取方法。

然后它调用带有值`this`和参数的方法。

例如，我们可以将这两个步骤写成:

```
const func = arr.toString;
func.call(arr);
```

# 直接方法调用的用例

直接方法调用在 ES5 中很有用，因为没有 spread 操作符来调用以数组 spread 作为参数的函数。

要调用带有一组项目作为参数的方法，我们可以编写:

```
const arr = [1, 2];
Array.prototype.push.apply(arr, [3, 4])
```

我们用`apply`方法调用`push`。

`arr`是`this`的值，是数组实例。

第二个参数是我们要传递给`push`的参数数组。

那么`arr`就是`[1, 2, 3, 4]`。

扩展运算符取代了`apply`的使用。

例如，我们可以写:

```
const arr = [1, 2];
arr.push(...[3, 4]);
```

这就简单多了，我们也不用担心`this`的值。

他们做同样的事情。

我们也可以使用 spread 操作符和`new`操作符。

例如，我们可以写:

```
new Date(...[2020, 11, 25])
```

`apply`不能和`new`一起使用，因为我们还没有创建构造函数的实例。

在 ES5 中，没有简单的方法将类似数组的对象转换成数组。

例如，如果我们想将`arguments`对象转换成一个数组，我们必须使用`Array.prototype.slice`方法来完成。

例如，我们可以写:

```
function foo(a, b, c) {
  var args = Array.prototype.slice.call(arguments);
  console.log(args);
}
```

我们称之为`Array.prototype.slice.call`，它接受一个可迭代的对象。

它返回一个数组，所以我们可以对它使用数组操作和方法。

同样，我们可以将它用于到`document.querySelectorAll`返回的节点列表，

例如，我们可以写:

```
var divs = document.querySelectorAll('div');
var arr = Array.prototype.slice.call(divs);
```

我们将作为节点列表的`divs`传递给`slice.call`方法，将其转换为数组。

在 ES6 中，这些都被 spread 和 rest 运算符所取代:

```
function foo(...args) {
  console.log(args);
}
```

和

```
const divs = document.querySelectorAll('div');
const arr = [...divs];
```

我们使用 rest 操作符和`foo`来获取数组形式的参数。

我们使用 spread 操作符将 div 扩展到一个数组中。

![](img/da3cb0bef05adcb10e65ac014ad201ba.png)

照片由[伯克利通信](https://unsplash.com/@berkeleycommunications?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

有几种方法可以调用方法。

我们可以从实例中调用它们，也可以用`call`和`apply`调用它们。