# 面向对象的 JavaScript——聚合和继承

> 原文：<https://blog.devgenius.io/object-oriented-javascript-aggregation-and-inheritance-1e140bb3adc0?source=collection_archive---------1----------------------->

![](img/7e45c99575345a97378f75dc0ab47691.png)

照片由 [Sereja Ris](https://unsplash.com/@kimtheris?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将介绍 JavaScript 的面向对象部分。

# 聚合

将几个对象组合成一个新对象的能力称为聚合或合成。

多个小对象比一个大对象更容易管理。

使用 JavaScript 有几种方法。

我们可以使用扩展操作符或`Object.assign`方法将多个对象合并成一个。

要使用`Object.assign`方法，我们可以写:

```
const obj1 = {
  foo: 1
}
const obj2 = {
  bar: 2
}
const obj3 = {
  bax: 3
}const merged = Object.assign({}, obj1, obj2, obj3);
```

我们有 3 个对象，我们把它们都传递给了`Object.assign`，这样我们就可以把它们合并在一起，并返回一个包含所有这些属性的新对象。

此外，我们可以通过编写以下内容来使用 spread 运算符:

```
const obj1 = {
  foo: 1
}
const obj2 = {
  bar: 2
}
const obj3 = {
  bax: 3
}const merged = {
  ...obj1,
  ...obj2,
  ...obj3
};
```

这也将把三个物体结合在一起。

聚合对象的另一种方法是通过将作为另一个对象的属性来拥有子对象。

例如，`Book`对象可以有多个`Author`对象、`Publisher`对象、`Chapter`对象等等。

# 遗产

继承是让重用现有代码的一种优雅方式。

我们可以创建对象并直接继承。

我们可以用子类创建 JavaScript 类。

类和子类是父构造函数和子构造函数的语法糖。

要创建一个使用另一个对象作为原型的对象，我们可以使用`Object.create`方法。

例如，我们可以写:

```
const obj = {
  foo: 1
}const child = Object.create(obj);
```

然后`child`从`obj`的`foo`属性中继承。

如果没有明确继承任何东西，大多数对象都以`Object.prototype`为原型。

我们可以用关键字`extends`创建子类。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this.name = name;
  } eat() {
    //...  
  }
}class Author extends Person {
  constructor(name, genre) {
    super(name);
    this.genre = genre;
  }
}
```

我们有`Person`类，它是父类。

并且`Author`类继承自`Person`类。

所有的实例变量和方法都是继承的。

所以我们可以获取`name`属性，并使用`Author`实例调用`eat`方法。

我们用`super`调用父构造函数。

我们也可以用它来访问父类的属性和方法。

我们可以写:

```
class Person {
  constructor(name) {
    this.name = name;
  } eat() {
    //...  
  }
}class Author extends Person {
  constructor(name, genre) {
    super(name);
    this.genre = genre;
  } eat() {
    super.eat();
  } write() {
    console.log(`${super.name} is writing`)
  }
}
```

我们称之为`Person`的`eat`方法。

我们用`super.name`得到了`Person`实例的`name`。

![](img/ff1ccd54de55fdd9b284cb0a0b22d15c.png)

照片由[挂牛](https://unsplash.com/@niuhang?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上

# 结论

我们可以以多种形式聚集对象。

对象和构造函数/类可以分别从其他对象和构造函数/类继承。