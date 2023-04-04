# 面向对象的 JavaScript —类基础

> 原文：<https://blog.devgenius.io/object-oriented-javascript-class-basics-154a9c154cb5?source=collection_archive---------5----------------------->

![](img/d02f85d2fb7759b89feb9c3bf8d34fe5.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看类的基础。

# 班级

ES6 带来了许多有用的特性，帮助我们更容易地创建对象。

如果我们熟悉像 Java 这样的基于类的语言，那么 JavaScript 类应该看起来很熟悉。

但是 JavaScript 类是其原型继承模型上的语法糖。

在 ES6 之前，我们只能创建构造函数。

```
function Person(name) {
  if (!(this instanceof Person)) {
    throw new Error("Person is a constructor");
  }
  this.name = name;
};Person.prototype.eat = function() {
  //...
};
```

我们有带有`this.name`实例属性的`Person`收缩器。

而`Person.prototype.eat`是一个实例方法。

然后我们可以通过编写以下代码创建一个继承自`Person`构造函数的子构造函数:

```
function Employee(name, job) {
  if (!(this instanceof Employee)) {
    throw new Error("Employee is a constructor");
  }
  Person.call(this, name);
  this.job = job;
};Employee.prototype = Object.create(Person.prototype);
Employee.prototype.constructor = Employee;
Employee.prototype.work = function() {
  // ...
};
```

我们创建了用`name`参数调用`Person`构造函数的`Employee`构造函数。

然后我们将`this.job`设置为`job`。

然后我们用`Object.create`创建`Employee`的原型。

我们继承了`Person.prototype`的属性，因为我们使用了带有`Person.prototype`作为参数的`Object.create`方法。

然后我们可以创建我们的方法并将构造函数设置为`Employee`，这样在`Employee`实例上使用`instanceof`将会返回`true`。

有了类语法，我们可以使我们的生活变得更加容易:

```
class Person {
  constructor(name) {
    this.name = name;
  } eat() {
    //...
  }
}class Employee extends Person {
  constructor(name, job) {
    super(name);
    this.job = job;
  } work() {
    //...
  }
}
```

我们创建了`Person`类。

`constructor`方法拥有构造函数本身的内容。

原型方法在类中。

为了创建一个子类，我们使用`extends`关键字，然后调用`super`来调用`Person`构造函数。

然后我们有了`work`实例方法。

`eat`方法也继承自`Person`实例。

# 定义类别

我们可以用关键字`class`定义类。

这与 Java 或 C#等基于类的语言非常相似。

例如，我们可以写:

```
class Car {
  constructor(model, year) {
    this.model = model;
    this.year = year;
  }
}
```

`Car`类仍然是一个函数。

所以如果我们记录:

```
console.log(typeof Car);
```

我们看到`'function'`。

类不同于普通的函数。

它们没有被升起。

普通函数可以在作用域的任何地方声明，并且它是可用的。

但是类只有在被定义后才能被访问。

所以我们不能写:

```
const toyota = new Car();
class Car {}
```

因为我们会得到参考误差:

```
Uncaught ReferenceError: Cannot access 'Car' before initialization
```

在分隔一个类的成员时，我们不能使用逗号，但是我们可以添加分号。

所以我们不能写:

```
class C {      
  member,
  method() {}
}
```

我们会得到一个语法错误。

但是我们可以写:

```
class C {        
  method2() {};
}
```

![](img/048e8007d0e411709886fc4225f4da72.png)

照片由[丹妮尔·塞鲁洛](https://unsplash.com/@dncerullo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

类语法使得创建构造函数更加容易。

我们可以用`extends`和`super`做继承。