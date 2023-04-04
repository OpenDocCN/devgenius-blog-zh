# 面向对象的 JavaScript——单件和装饰件

> 原文：<https://blog.devgenius.io/object-oriented-javascript-singletons-and-decorators-6ba1eb2654d0?source=collection_archive---------4----------------------->

![](img/da3a8a45991dc17050a42a1f12b12d1c.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究一些基本的设计模式。

# 类单例

我们可以通过将类实例存储在一个全局对象中来创建一个单例对象。

例如，我们可以写:

```
function Foo() {
  if (typeof globalFoo === "undefined") {
    globalFoo = this;
  }
  return globalFoo;
}
```

然后我们可以通过编写以下代码来创建一个`Foo`实例:

```
const a = new Foo();
const b = new Foo();
console.log(a === b);
```

控制台日志应该记录`true`，因为我们将现有的实例存储在一个全局变量中。

如果不是`undefined`，我们就退回去。

# 单一存储在构造函数的属性中

我们知道不应该使用全局变量，所以我们可以将单例实例存储为构造函数的属性。

函数可以有属性，因为它们是对象。

所以我们可以写:

```
function Foo() {
  if (typeof Foo.instance === "undefined") {
    Foo.instance = this;
  }
  return Foo.instance;
}
```

唯一的变化是我们将属性存储在实例中，而不是存储在全局变量中。

如果我们把它放在一个函数中，我们也可以把它存储在私有属性中。

所以我们可以写:

```
const Foo = (() => {
  let instance;
  return function() {
    if (typeof instance === "undefined") {
      instance = this;
    }
    return instance;
  }
})();
```

做同样的事情。

这样，我们就不会意外地将实例更改为其他内容。

# 工厂模式

工厂模式是创造性的设计模式。

它处理创建对象。

工厂是一个功能，可以帮助我们用一个功能创建相似类型的对象。

例如，我们可以通过编写以下代码来创建一个函数:

```
const createElement = (type, url) => {
  if (type === 'Image') {
    return new Image(url);
  }
  if (type === 'Link') {
    return new Link(url);
  }
  if (type === 'Text') {
    return new Text(url);
  }
}
```

我们基于`type`值返回构造函数实例。

我们还采用了用于构造函数的`url`。

# 装饰图案

装饰模式是一种结构模式。

我们可以使用这种模式来扩展对象的功能。

使用这种模式，我们可以通过选择想要应用于对象的装饰器来扩展对象的功能。

例如，我们可以写:

```
function Person(name) {
  this.name = name;
  this.say = function() {
    console.log(this.name);
  };
}function DecoratedPerson(person, street, city) {
  this.person = person;
  this.person = person.name;
  this.street = street;
  this.city = city;
  this.say = function() {
    console.log(this.name, this.street, this.city);
  };
}
```

我们有两个构造函数`Person`和`DecoratedPerson`类。

`Person`是我们用作`DecoratedPerson`类基础的类。

`DecoratedPerson`类在`Person`类的基础上增加了额外的属性。

这样，我们保持`DecoratedPerson`的接口与来自`Person`的`Person`构造函数相同。

但是我们添加了更多`DecoratedPerson`类独有的属性。

![](img/15e5befc585539af9658e4f2d2738656.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以用类似语法的类创建单例对象。

装饰模式让我们扩展构造函数的功能。