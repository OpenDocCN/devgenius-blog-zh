# 面向对象的 JavaScript——私有实体和链接

> 原文：<https://blog.devgenius.io/object-oriented-javascript-private-entities-and-chaining-ad7a04dbb614?source=collection_archive---------6----------------------->

![](img/ec01e797896f10268d839f9ca3d8c2da.png)

[moto moto sc](https://unsplash.com/@scphotoporto?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究一些基本的编码和设计模式。

# 特许方法

特权方法是道格拉斯·克洛克福特创造的一个术语。

这些是可以访问私有方法和属性的公共方法。

它们作为一座桥梁，以可控的方式提供一些私有功能。

例如，我们可以写:

```
let APP = {};
APP.dom = (() => {
  const setStyle = function(el, prop, value) {
    console.log('setStyle');
  }; const getStyle = function(el, prop) {
    console.log('getStyle');
  }; return {
    setStyle,
    getStyle,
    //...
  };
}());
```

我们有在生命中定义的`setStyle`和`getStyle`函数。

我们通过返回对象，然后将它赋给一个在函数外部可用的属性来公开它们。

这些函数内部可以有私有变量，这样我们就可以做任何我们想做的事情，而不用暴露所有的东西。

# 即时功能

立即函数通过将代码包装在匿名函数中并立即运行，让我们保持全局名称空间的整洁。

例如，我们可以写:

```
(function() {
  // ...
}());
```

创建一个函数并立即调用它。

这样，我们可以在其中保存私有变量和方法。

例如，我们可以写:

```
let APP = {};
APP.methods = (() => {
  function _private() {
    // ...
  }
  return {
    foo() {
      console.log('...');
      _private();
    },
    bar() {
      console.log('...');
    }
  };
}());
```

我们有 IIFE，它用调用私有`_private`函数的`foo`方法返回一个对象。

# 模块

ES5 没有模块，所以如果我们为浏览器编写脚本，我们可能必须使用模块来编写脚本。

例如，我们可以通过编写以下代码来创建一个模块:

```
let APP = {
  module: {}
};
APP.module.foo = (() => {
  const another = APP.module.another;
  let foo, bar; function hidden() {}
  //... return {
    hi() {
      return "hello";
    }
  };
}());
```

我们创建了一个引用了一个`another`模块的`foo`模块。

在模块中，我们定义了一些变量和`hidden`函数。

我们在一个对象中返回一个公共的`hi`方法。

那么我们可以`hi`通过写:

```
APP.module.foo.hi()
```

# 链接

链接是一种让我们在一行中调用多个方法的模式。

这些方法链接到一个链上。

例如，我们可以写:

```
class Foo {
  foo() {
    this.foo = 'foo';
    return this;
  } bar() {
    this.bar = 'bar';
    return this;
  } baz() {
    this.baz = 'baz';
    return this;
  }
}
```

我们有多个返回`this`的方法，也就是`Foo`实例。

这样，我们可以在一个链中调用这些方法。

所以我们可以写:

```
new Foo().foo().bar().baz();
```

来调用方法链。

![](img/97e4800461a216d21f375e08e23d6f4c.png)

迈克·阿隆佐在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

函数中可以有私有变量、对象和函数。

此外，我们可以用自己的对象创建自己的模块。

我们可以通过在方法中返回`this`来使它们可链接。