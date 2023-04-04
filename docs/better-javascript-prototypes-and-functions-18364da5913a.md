# 更好的 JavaScript——原型和函数

> 原文：<https://blog.devgenius.io/better-javascript-prototypes-and-functions-18364da5913a?source=collection_archive---------5----------------------->

![](img/f2f2bf540340594c608965ee467f911e.png)

照片由[维里·伊万诺娃](https://unsplash.com/@veri_ivanova?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 闭包比字符串更适合封装代码

我们不应该在字符串中使用代码。

如果我们需要运行代码，我们应该把它们放在一个函数中。

例如，如果我们有了`repeat`函数，我们就不应该写这样的代码:

```
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    eval(action);
  }
}
```

由于安全性、性能和范围问题，总是不好。

相反，我们应该写:

```
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    action();
  }
}
```

然后我们可以通过写来使用它:

```
const start = [];
repeat(1000, () => {
  start.push(Date.now());
});
```

我们传入了一个回调函数，以便在函数中运行`action`。

代码不应出现在字符串中。

相反，它应该接受一个回调函数并运行它。

# 不要依赖函数的 toString 方法

`toString`方法并不总是返回函数的源代码。

对于我们创建的函数，它确实会在大多数情况下返回源代码。

所以如果我们有:

```
(function(x) {
  return x + 2;
}).toString();
```

然后我们得到:

```
"function(x) {
  return x + 2;
}"
```

结果。

如果我们在内置的本地对象上使用`toString`方法，那么我们将看不到函数的源代码。

例如，如果我们有:

```
Math.max.toString()
```

然后我们得到:

```
"function max() { [native code] }"
```

如果我们做一些事情，比如调用函数`bind`:

```
(function(x) {
  return x + 2;
}).bind(16).toString();
```

然后我们得到:

```
"function(){return n.apply(e,t||arguments)}"
```

我们在字符串中没有得到任何有意义的东西。

所以我们不能依赖`toString`总是返回一个函数的源代码。

# 避免非标准堆栈检查属性

我们不应该使用像`arguments.callee`这样的属性来获取调用另一个函数的函数。

即使属性存在，我们也不应该在属性上使用`caller`属性来获取调用该函数的函数。

这些不是标准的，应该避免。

所以我们应该编写这样的代码:

```
function revealCaller() {
  return revealCaller.caller;
}
```

这些在 ES5 严格模式中都是不允许的，所以这是启用它的一个好处。

如果我们有:

```
function foo() {
  "use strict";
  return foo.caller;
}
```

我们得到“未捕获的类型错误:“调用者”、“被调用者”和“参数”属性不能在严格模式函数或对它们的调用的参数对象上访问”。

# 获取 prototype、getPrototypeOf 和 __proto__ 之间的区别

这些物体是相互分离的。

`prototype`和`__proto__`都是获取一个对象的原型并设置。

而`getPrototypeOf`是获取对象原型的方法。

`__proto__`已经成为一个标准，所以我们可以像`prototype`一样使用它。

`prototype`通常用于举例说明方法。

例如，我们可以通过编写以下代码来创建构造函数:

```
function Person(name) {
  this.name = name;
}Person.prototype.toString = function() {
  return `Person ${this.name}`;
};
```

`toString`是构造函数的实例方法。

然后我们可以写:

```
const person = new Person("james");
```

创建一个新的`Person`实例。

`__proto__`属性可以像常规属性一样使用，所以我们可以写:

```
person.__proto__
```

来得到这个物体的原型。

我们也可以给它赋值，并把它作为一个对象的属性。

`Object.getPrototypeOf`可以用来得到一个对象的原型，所以我们可以写:

```
Object.getPrototypeOf(person)
```

得到原型。

原型是一个对象从其继承的 JavaScript 对象。

![](img/ebc5a3a17c5ea4122e43b68c4f399af5.png)

亚历克斯·哈维的照片🤙🏻 on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该在代码中以字符串的形式运行代码。

同样，我们不应该在代码中使用`caller`和`callee`。

我们可以通过各种方式获取和设置对象原型。