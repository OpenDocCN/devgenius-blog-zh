# 现代 JavaScript 的精华——一流的安全性和优缺点

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-class-safety-and-pros-and-cons-90141019e2af?source=collection_archive---------4----------------------->

![](img/91789cd80c076835c59bfd88a5905144.png)

[乔丹·罗兰](https://unsplash.com/@yakimadesign?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看如何用 JavaScript 定义类。

# 安全检查

JavaScript 解释器在实例化类时会做一些安全检查。

`this`最初在派生构造函数中未初始化意味着如果我们试图在子类中调用`super`之前访问`this`将会抛出错误。

当`this`被初始化时，调用`super`会产生一个`ReferenceError`，因为`super`已经被调用来初始化`this`。

如果构造函数返回一个非对象，那么抛出一个`TypeError`。

如果一个构造函数显式地返回一个对象，那么它就被用作结果。

这种情况下`this`初始化与否并不重要。

# extends 关键字

我们扩展的值必须是一个构造函数。

但是，`null`是允许的。

例如，我们可以写:

```
class Foo extends Bar {}
```

鉴于`Bar`是一个构造函数。

在这种情况下,`Foo.prototype`应该是`Bar`。

我们也可以写:

```
class Foo extends Object {}
```

由于`Object`是构造函数。

在这种情况下，`Foo.prototype`应该是`Object`。

我们也可以写:

```
class Foo extends null {}
```

那么`Foo.prototype`就是`null`。

# 在方法中引用基类属性

我们可以在方法中引用基类属性。

例如，如果我们有:

```
class Person {
  constructor(name) {
    this.name = name;
  } toString() {
    return `${this.name}`;
  }
}
```

然后我们可以通过编写以下代码为`Person`创建一个子类:

```
class Student extends Person {
  constructor(name, grade) {
    super(name);
    this.grade = grade;
  }

  toString() {
    return `${super.toString()} (${this.grade})`;
  }
}
```

我们创建`toString`来创建一个用`super.toString`调用`Person`的`toString`方法的方法。

这是通过在原型链上搜索`toString`方法来获得`toString`方法并调用它来完成的。

如果找到了，就调用这个方法。

这与我们在 ES5 或更早版本中所做的不同。

在早期版本中，我们用`call`方法调用超类方法。

例如，我们可以写:

```
var result = Person.prototype.toString.call(this);
```

在 ES6 或更高版本中，我们可以看到，我们不必直接引用父类。

我们就用`super`。

`super`可用于子类方法和构造函数。

它们不能在函数声明中使用。

使用`super`的方法不能移动。

它与创建它的对象相关联。

# 班级的利弊

上课有好处也有坏处。

类语法使得构造函数看起来更像基于类的语言中的类。

非传统的继承模式让许多人感到困惑。

它隐藏了管理原型和构造函数的复杂性。

类与任何当前代码都是向后兼容的，所以不会引入重大的变化。

类语法支持子类化。

初学者也更容易理解类语法而不是原型。

继承不需要库，这很好。

这使得它们更便于携带。

它们也为更高级的 OOP 特性提供了基础，比如 traits 和 mixins。

还可以使用 ide、文本编辑器等等更容易地对类进行静态分析。

然而，它们确实隐藏了 JavaScript 面向对象模型的真实本质。

JavaScript 类看起来像它自己的实体，但它实际上是一个函数。

然而，由于向后兼容性的需要，JavaScrtipt 类不可能是一个全新的实体。

这是使类语法与现有代码一起工作的一种折衷。

![](img/60cf20a3783bc7712e407f2de55522b5.png)

照片由 [freestocks](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

JavaScript 解释器为我们提供了对类的安全检查。

此外，类语法也有优点和缺点。