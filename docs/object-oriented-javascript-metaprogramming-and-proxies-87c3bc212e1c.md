# 面向对象的 JavaScript —元编程和代理

> 原文：<https://blog.devgenius.io/object-oriented-javascript-metaprogramming-and-proxies-87c3bc212e1c?source=collection_archive---------6----------------------->

![](img/6727c46894f866dae00d4ca791cfbe07.png)

照片由 [Aryo Kadiono](https://unsplash.com/@asury?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究带有代理的 JavaScript 元编程。

# 元编程和代理

元编程是一种编程方法，在这种方法中，程序知道自己的结构并操纵自己。

有多种方法可以进行元编程。

一个是自省。这是我们对程序内部进行只读访问的地方。

自我修改是对程序进行结构上的改变。

代祷是我们改变语言语义的地方。

在 JavaScript 中，我们可以通过代理来做到这一点。

它们让我们控制如何访问和设置对象。

# 代理人

我们可以使用代理来决定一个对象的行为。

被控制的对象称为目标。

我们可以为对象上的基本操作定义自定义行为，如属性查找、函数调用和赋值。

代理需要两个参数，

一个是处理程序，它是一个带有方法的对象，可以让我们改变对象操作的行为。

目标是我们要将操作更改到的目标。

例如，我们可以通过编写以下代码来创建一个控制对象的代理:

```
const handler = {
  get(target, name) {
    return name in target ? target[name] : 1;
  }
}
const proxy = new Proxy({}, handler);
proxy.a = 100;console.log(proxy.a);
console.log(proxy.b);
```

我们用带有`handler`对象的处理程序创建了一个代理。

`get`方法让我们控制如何检索属性。

`target`是我们控制的对象。

`name`是我们想要访问的属性名。

在`get`方法中，我们检查`name`代理是否存在。

如果是，我们返回目标值，否则我们返回 1。

然后我们用`Proxy`构造函数创建一个代理。

第一个参数是一个空对象。

`handler`是我们控制操作的处理器。

`proxy.a`被定义，所以它的值被返回。

否则，我们返回默认值。

此外，在将值设置为对象之前，我们可以使用代理来验证值。

例如，我们可以通过编写以下代码来捕获`set`处理程序:

```
const ageValidator = {
  set(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('age must be a number');
      }
      if (value < 0 || value > 130) {
        throw new RangeError('invalid age range');
      }
    }
    obj[prop] = value;
  }
};
const p = new Proxy({}, ageValidator);
p.age = 100;
console.log(p.age);
p.age = 300;
```

我们有带`obj`、`prop`和`value`参数的`set`方法。

`obj`是我们要控制的对象。

`prop`是属性键。

`value`是我们要设置的属性值。

我们检查`prop`是否是`'age'`，以便我们验证`age`属性的赋值。

然后我们检查它是否是一个整数，如果不是，我们抛出一个错误。

如果超出范围，我们也会抛出一个错误。

然后我们用`Proxy`构造函数创建一个代理，用`ageValidator`作为处理程序和一个空对象来控制。

然后，如果我们试图将`p.age`设置为 300，我们会得到一个`RangeError`。

![](img/1e6911afeab24696d3a446c51db4ac15.png)

斯科特·韦伯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

代理让我们控制如何检索和设置对象属性。