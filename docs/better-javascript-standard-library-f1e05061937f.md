# 更好的 JavaScript —标准库

> 原文：<https://blog.devgenius.io/better-javascript-standard-library-f1e05061937f?source=collection_archive---------9----------------------->

![](img/f29e186c5821c2d19e5971487d8954da.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 从标准类继承

使用 class 语法，我们可以很容易地创建标准类的子类。

例如，我们可以通过编写以下代码来创建`Array`构造函数的子类:

```
class MyArray extends Array {}const myArray = new MyArray(1, 2, 3);
```

那么`myArray.length`就是 3。

由于当我们调用`MyArray`构造函数时会调用`Array`构造函数，因此`length`属性被正确设置。

`MyArray`的`[[Class]]`值被设置为`Array`，因此`length`被正确设置。

如果我们得到`myArray`的原型:

```
Object.prototype.toString.call(myArray)
```

我们得到:

```
[object Array]
```

以下是各种内置构造函数的`[[Class]]`值:

*   `Array`——`[[Class]]`是`'Array'`
*   `Boolean`——`[[Class]]`是`'Boolean'`
*   `Date`——`[[Class]]`是`'Date'`
*   `Error`——`[[Class]]`就是`'Error'`
*   `Function`——`[[Class]]`就是`'Function'`
*   `JSON`——`[[Class]]`就是`'JSON'`
*   `Math`——`[[Class]]`就是`'Math'`
*   `Number`——`[[Class]]`就是`'Number'`
*   `Object`——`[[Class]]`就是`'Object'`
*   `RegExp`——`[[Class]]`是`'RegExp'`
*   `String`——`[[Class]]`是`'String'`

这种类型的继承只能用类语法来完成。

旧的构造函数语法无法正确设置`[[Class]]`属性。

# 原型是一个实现细节

原型具有对象从其继承的属性。

对象就是接口。

我们不应该检查我们不控制的对象的原型结构。

我们不应该检查实现我们不控制的对象内部的属性。

如果这是我们无法控制的事情，那么我们就无法改变它们。

所以我们必须用自己控制的代码来处理它们。

# 没有鲁莽的猴子补丁

猴子补丁是动态地改变代码。

JavaScript 并不阻止我们改变任何对象的属性，包括内置对象。

例如，我们可以通过编写以下代码向`Array`构造函数添加一个实例方法:

```
Array.prototype.split = function(i) { 
  return [this.slice(0, i), this.slice(i)];
};
```

但是其他人可能会做同样的事情。

所以我们可能会在内置对象中得到我们没有预料到的属性和方法。

它们可以被删除，破坏大量使用它的代码。

因此，如果我们向内置对象添加我们不期望的东西，我们就会遇到问题。

添加我们自己的代码的更好方法是将它们与内置对象分开。

例如，我们可以写:

```
function split(arr, i) { 
  return [arr.slice(0, i), arr.slice(i)];
};
```

创建我们自己的`split`函数。

然而，我们可以添加的一个可接受的改变现有内置对象的东西是 polyfills。

它们是添加了将来 JavaScript 引擎应该支持的功能的库。

它们的行为是标准化的，因此我们可以使用它们以安全的方式添加方法。

![](img/186020fb0aba330ce99e1e357a3c6aed.png)

照片由[rayu 马尔代夫摄影师](https://unsplash.com/@rayyu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

使用类语法，我们可以从标准类继承。

此外，我们不应该检查不受我们控制的代码，因为我们不能改变它们。

我们也不应该用自己的代码修补本机内置对象。