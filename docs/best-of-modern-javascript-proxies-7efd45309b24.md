# 现代 JavaScript 的精华—代理

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-proxies-7efd45309b24?source=collection_archive---------8----------------------->

![](img/2b02106a84500daa5c43ca9a995effa9.png)

照片由 [Aryo Kadiono](https://unsplash.com/@asury?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究使用 JavaScript 代理的元编程。

# 元编程

元编程是编写代码来处理我们的应用程序级代码。

应用程序级代码是接受用户输入并为用户返回输出的代码。

有各种各样的元编程。

一种是自省。这是我们只读访问程序结构的地方。

自我修改是我们改变程序结构的地方。

调解是我们重新定义一些语言操作的语义的地方。

Javascript 中自省的一个例子是使用`Object.keys()`来获取对象的键。

自我修改的一个例子是将属性从一个地方移动到另一个地方。

例如，我们可以写:

```
function moveProperty(source, prop, target) {
  target[prop] = source[prop];
  delete source[prop];
}
```

将`prop`属性从`source`对象移动到`target`对象。

然后我们删除`source`对象的`prop`属性。

代理让我们与 JavaScript 应用程序进行调解。

我们可以用它来改变对象操作的方式。

# 委托书

代理为 JavaScript 带来了调解。

我们可以对一个对象进行许多操作。

我们可以得到属性值，也可以设置它。

此外，我们可以检查属性是否在对象中。

我们可以用代理定制这些操作。

例如，我们可以写:

```
const target = {};
const handler = {
  get(target, propKey, receiver) {
    console.log(target, propKey, receiver);
    return 'foo';
  }
};
const proxy = new Proxy(target, handler);console.log(proxy.bar)
```

我们创建一个用于代理的空的`target`对象。

而`handler`对象有一个`get`方法，让我们为给定的属性返回值。

`target`是我们传递给`Proxy`构造函数第一个参数的对象。

`propKey`是属性名。

因此，如果我们获得`proxy.bar`的值或`proxy`的任何其他属性，我们将返回`'foo'`。

我们还可以改变检查对象中是否存在属性的方式。

例如，我们可以写:

```
const target = {};
const handler = {
  has(target, propKey) {
    console.log(propKey);
    return true;
  }
};
const proxy = new Proxy(target, handler);console.log('bar' in proxy);
console.log('foo' in proxy);
```

然后我们得到`target`和`propKey`参数，它们的值与`get`中的值相同。

我们返回了`true`，所以每当我们对它使用`in`操作符时，它都会返回`true`。

`set`方法可以让我们改变对象的设置方式。

# 拦截方法调用

我们可以通过调用对象属性中的方法来拦截方法调用。

例如，我们可以写:

```
const target = {
  foo(text) {
    return `text ${text}`;
  }
};const handler = {
  get(target, propKey, receiver) {
    const origMethod = target[propKey];
    return function(...args) {
      const result = origMethod.apply(this, args);
      console.log(JSON.stringify(args), JSON.stringify(result));
      return result;
    };
  }
};
const proxy = new Proxy(target, handler);console.log(proxy.foo('baz'));
```

我们把`handler`方法中的`get`方法称为`target.foo`方法。

在`get`方法内部，我们调用带有参数的方法，但是使用`this`作为代理，而不是`target`对象。

这样，我们可以改变方法调用的行为。

我们记录了争论及其结果。

这对于记录方法调用很方便。

![](img/873813b31a0b1a24ed921d520f1455b6.png)

由 [Gustavo Zambelli](https://unsplash.com/@zamax?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

代理让我们控制如何获取和设置对象属性。

我们也可以改变检查属性的方式。