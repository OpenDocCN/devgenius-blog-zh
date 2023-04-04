# 现代 JavaScript 的精华——类结构

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-class-structure-e4a455b118db?source=collection_archive---------10----------------------->

![](img/d2a38fda51b0b7956272f00a9c6838ab.png)

[泰勒·威尔科克斯](https://unsplash.com/@taypaigey?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看如何用 JavaScript 定义类。

# 类定义的成员之间没有分隔符

没有分离器分离方法。

我们可以加上分号，但它被忽略了。

它是为将来可能包含分号结束成员的语法准备的。

禁止使用逗号。

# 类声明不会被挂起

类声明没有被提升。

它们只能在定义类之后使用。

例如，我们可以写:

```
foo();

function foo() {}
```

因为`foo`被吊起来了。

但是我们不能写:

```
new Bar();class Bar {}
```

因为`Bar`没有被提升，所以我们得到一个 ReferenceError。

这不是一个很大的限制。

我们大多数人都希望只有在定义了变量之后才使用它。

# 类别表达式

我们可以定义一个类并将它赋给一个变量。

这叫做类表达式。

例如，我们可以写:

```
const Foo = class {
  //...
};const foo = new Foo();
```

我们用`class`关键字创建了这个类，并把它赋给了`Foo`变量。

然后我们使用`new`操作符来创建实例。

如果我们的类是命名的，那么这个名字只在类本身内部可见。

例如，如果我们有:

```
const Foo = class Bar {
  getClassName() {
    return Bar.name;
  }
};
const foo = new Foo();
```

然后`foo.getClassName()`返回`'Bar'`。

# 在类定义的主体内

类体只能包含方法，不能包含属性。

具有数据属性的原型是一种反模式，所以不允许它们是件好事。

一个类可能有`constructor`，一个静态方法和原型方法。

例如，我们可以写:

```
class Bar {
  constructor(prop) {
    this.prop = prop;
  }
  static staticMethod() {
    return 'static';
  }
  prototypeMethod() {
    return 'something';
  }
}
```

`constructor`让我们用我们想要的实例变量实例化这个类。

`static`关键字定义了一个静态方法，它定义了一个可以在类本身而不是类实例上调用的方法。

如果没有`static`关键字，那么我们有一个实例方法。

这是`prototype`属性的属性。

# 静态数据属性

类可以有静态数据属性。

我们可以直接在类中添加一个属性。

例如，我们可以写:

```
class Foo {
  constructor(prop) {
    this.prop = prop;
  }
  static staticMethod() {
    return 'classy';
  }
  prototypeMethod() {
    return 'prototypical';
  }
}Foo.bar = 'baz';
```

在`Foo`类上定义静态`bar`属性。

我们可以用`Foo.bar`直接访问。

创建静态属性的第二种方法是创建静态 getter。

例如，我们可以写:

```
class Foo {
  constructor(prop) {
    this.prop = prop;
  } static get BAZ() {
    return 'baz';
  }
}
```

我们可以用`Foo.BAZ`得到值。

# Getters 和 Setters

类可以有 getters 和 setters。

例如，我们可以写:

```
class Foo {
  get prop() {
    return this.bar;
  }
  set prop(value) {
    this.bar = value;
  }
}
```

`get`关键字表示一个 getter。

而`set`关键字表示二传手。

getter 应该返回一些东西。

setter 应该将实例变量设置为一个值。

我们通过编写以下内容来访问 getters:

```
const foo = new Foo();
foo.prop = 100;
console.log(foo.prop);
```

我们在第二行使用二传手。

我们用第三行的 getter 得到了这个值。

`foo.prop`第 3 行应该返回 100。

![](img/9b7168253ecb0c92e4616a9f3634eaff.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以向类中添加构造函数、静态成员、实例成员以及 getters 和 setters。