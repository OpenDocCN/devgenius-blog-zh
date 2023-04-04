# 更好的 JavaScript——原型

> 原文：<https://blog.devgenius.io/better-javascript-prototypes-e9db07269119?source=collection_archive---------8----------------------->

![](img/a6e4b637204d506ef7c104cf8c869e7a.png)

Viktor Forgacs 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# Object.getPrototypeOf 或 __proto__

从 ES6 开始，`__proto__`已经成为一个对象的标准属性。

我们可以像获取和设置任何其他属性一样获取和设置它。

例如，我们可以写:

```
const obj = {};console.log(obj.__proto__);
```

才能得到`obj`的原型。

我们用`__proto__`属性得到对象的原型。

我们可以这样写:

```
const obj = {};
obj.__proto__ = {
  foo: 1
};console.log(obj.__proto__);
```

我们得到了原型`{foo: 1}`。

如果我们想用原型创建一个对象，我们也可以用原型调用`Object.create`。

所以我们可以写:

```
const obj = Object.create({
  foo: 1
})
```

我们得到了同样的结果。

我们也可以使用`Object.getPrototypeOf`方法来获得一个对象的原型。

例如，我们可以写:

```
Object.getPrototypeOf(obj)
```

我们得到的结果与得到`__proto__`属性的结果相同。

属性是标准的，所以我们可以安全地使用它来获取和设置原型。

# 使构造函数成为新不可知论者

如果我们正在创建一个构造函数，那么我们可以把它作为一个函数来调用。

例如，如果我们有:

```
function Person(name) {
  this.name = name;
}const p = Person('james');
console.log(this.name)
```

那么`this`是全局对象并且`name`属性具有值`'james'`。

这绝对不是我们想要的。

如果我们有严格模式，那么`this`将会是顶层的`undefined`，所以我们不会意外地创建全局变量。

如果我们有:

```
function Person(name) {
  "use strict";
  this.name = name;
}const p = Person('james');
```

然后我们得到错误“未捕获的类型错误:无法设置 undefined 的属性“name”。

这为我们提供了一些保护，使我们免于像常规函数一样调用构造函数。

为了使检查更加可靠，我们可以在构造函数中添加一个实例检查:

```
function Person(name) {
  if (!(this instanceof Person)) {
    return new Person(name);
  }
  this.name = name;
}
```

这样，不管有没有`new`，我们仍然得到返回的`Person`实例。

最好的方法是使用类语法。

因此，我们可以将构造函数重写为:

```
class Person {
  constructor(name) {
    this.name = name;
  }
}
```

这样，如果我们在没有`new`的情况下调用`Person`，我们会得到错误‘未捕获的类型错误:没有‘new’就不能调用类构造函数 Person’。

![](img/b52d3db0b3a43d4f540280647ff8077b.png)

[UX 店](https://unsplash.com/@uxstore?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

有多种方法可以获得和设置原型。

此外，类语法是创建构造函数的最健壮的方法。