# 现代 JavaScript 的精华—新的对象方法

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-new-object-methods-1a427aa9b3aa?source=collection_archive---------7----------------------->

![](img/a5236834b01ea5d94c727c222aa3ba98.png)

生殖健康用品联盟在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中新的 OOP 特性。

# 复制所有自己的属性

我们可以通过用`Reflect.ownKeys`方法获取所有自己的键来从对象中复制所有自己的属性。

例如，我们可以写:

```
function copyAll(target, ...sources) {
  for (const source of sources) {
    for (const key of Reflect.ownKeys(source)) {
      const desc = Object.getOwnPropertyDescriptor(source, key);
      Object.defineProperty(target, key, desc);
    }
  }
  return target;
}
```

我们有一个包含对象数组的`sources`数组，我们希望将这些对象的属性复制到`target`中。

我们循环遍历它们，然后用`Reflect.ownKeys`得到密钥。

然后我们得到属性描述符，它包括带有`Object.getOwnPropertyDescriptor`的名称和值。

然后我们可以用`Object.defineProperty`来定义属性。

第一个参数是要将属性复制到的`target`对象。

第二个参数是属性的`key`。

第三个参数是属性描述符。

# `Object.assign()`不适用于移动方法

`Object.assign`对于移动方法不太管用。

它不能移动任何继承的方法。

这是因为原型有很多不可枚举的方法，不能用`Object.assign`移动。

# 例子

我们可以使用`Object.assign`方法将新属性复制到`this`。

例如，我们可以写:

```
class Point {
  constructor(x, y) {
    Object.assign(this, {
      x,
      y
    });
  }
}
```

这比以下内容重复性更低:

```
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}
```

我们还可以用它来提供对象属性的默认值。

例如，我们可以写:

```
const DEFAULTS = {
  logLevel: 0,
  outputFormat: 'html'
};let options = {
  frequency: 100
}options = Object.assign({}, DEFAULTS, options);
```

我们将`DEFAULTS`和`options`作为参数传入，将属性放入空对象。

对象作为两个对象的所有属性返回。

同样，我们可以使用`Object.assign`方法向对象添加方法。

例如，我们可以写:

```
function SomeClass() {}Object.assign(SomeClass.prototype, {
  foo(arg1, arg2) {
    //...
  },
  bar() {
    //...
  }
});
```

我们定义了`SomeClass`构造函数，并用`Object.assign`方法向其原型添加了更多属性。

`Object.assign`也非常适合克隆对象。

例如，我们可以写:

```
const clone = Object.assign({}, orig);
```

通过将属性从对象复制到一个空对象并返回来创建一个`orig`对象的克隆。

# `Object.getOwnPropertySymbols(obj)`

`Object.getOwnPropertySymbols`方法让我们获得对象的非继承符号键。

它是对`Object.getOwnPropertyNames()`的补充，后者获取一个对象的所有非继承的字符串键。

例如，我们可以写:

```
const foo = Symbol('foo');
const obj = {
  [foo]: 'bar'
}const keys = Object.getOwnPropertySymbols(obj)
```

这个`keys`就是`[Symbol(foo)]`。

# Object.is(值 1，值 2)

`Object.is`是一种让我们比较两个值以确定它们是否相同的方法。

和`===`操作符差不多。

不同之处在于`NaN`等于自身。

另外，`-0`不被认为与`+0`相同。

所以:

```
Object.is(NaN, NaN)
```

返回`true`。

并且:

```
Object.is(-0, +0)
```

返回`false`。

![](img/fb4d6f631bd2a302a107f39b51c7b5b9.png)

照片由 [Daria Nepriakhina](https://unsplash.com/@epicantus?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用`Reflect.ownKeys`得到所有非继承的键。

另外，`Object`在 ES6 中增加了各种新方法。