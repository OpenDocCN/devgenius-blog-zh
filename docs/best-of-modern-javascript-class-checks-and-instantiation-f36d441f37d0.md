# 现代 JavaScript 的精华—类检查和实例化

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-class-checks-and-instantiation-f36d441f37d0?source=collection_archive---------12----------------------->

![](img/0000790108f3611c6019a16400732819.png)

蒂姆·多弗勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看如何用 JavaScript 定义类。

# 课程详细信息

上课有很多细节。

我们扩展的值可以是任意的表达式。

例如，我们可以扩展一个调用函数产生的类:

```
class Foo extends combine(Foo, Bar) {}
```

# 检查

当我们创建类时，JavaScript 解释器会做一些检查。

类名不能是`eval`或`arguments`。

不允许重复的类名。

名称`constructor`可以用于普通方法，而不能用于 getters、setters 或 generator 方法。

类不能作为函数调用。

如果这样做，将会抛出一个 TypeError。

原型方法不能用作构造函数。

所以我们不能写这样的东西:

```
class Foo {
  bar() {}
}new Foo.prototype.bar();
```

如果我们运行它，我们将得到“未捕获的类型错误:Foo.prototype.bar 不是构造函数”错误。

# 属性描述符

一个类`Bar`的静态属性是可写和可配置的，但不是无数的。

`Bar.prototype`不可写、不可枚举、不可配置。

`Bar.prototype.constructor`不可写也不可枚举，但可以配置。

`Bar.prototype`的属性是可写和可配置的，但不可枚举。

这意味着我们可以动态更新许多属性。

# 类有内部名称

一个类有自己的`name`属性来返回它的内部名称。

它的名字也可以用在类本身中。

例如，我们可以写:

```
class Foo {
  bar() {
    console.log(Foo.baz);
  }
}Foo.baz = 1;
```

我们引用了`Foo`来获取静态属性。

即使我们将类赋给一个变量，也是如此。

例如，如果我们有:

```
const Bar = class Foo {
  bar() {
    console.log(Foo.baz);
  }
}Bar.baz = 1;new Bar().bar();
```

那么`bar`方法调用仍然记录 1。

这意味着我们可以在内部引用它的原始名称。

# 子类

我们通过使用`extends`关键字来创建子类。

对于静态方法，它们由子类直接继承。

例如，如果我们有:

```
class Person {
  static logName() {
    console.log(Person.name)
  }
}class Employee extends Person {}console.log(Person.logName === Employee.logName)
```

然后控制台日志会记录`true`。

JavaScript 类的原型是`Function.prototype`。

这意味着 JavaScript 类实际上是一个函数。

我们可以这样写:

```
class Person {
  static logName() {
    console.log(Person.name)
  }
}

console.log(Object.getPrototypeOf.call(Object, Person) === Function.prototype)
```

我们调用`Object.getPrototypeOf`方法来获得`Person`类的原型。

然后我们对照`Function.prototype`进行检查。

而这个日志`true`。

所以我们知道类是幕后的函数。

# 初始化实例

通过编写以下代码初始化类实例:

```
function Person(name) {
  this.name = name;
}
```

在 ES5 中。

在 ES6 中，基本构造函数是用`super`方法创建的。

这会触发父构造函数调用。

在 ES5 中，当我们使用`new`调用构造函数时，就会调用超级构造函数。

它通过函数调用来调用。

![](img/78f5db1fbc41542c74ae4836af3f1786.png)

照片由[格伦·卡斯滕斯-彼得斯](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

当我们创建类时，JavaScript 解释器会做很多检查。

还有，静态方法由子类继承，类是函数。