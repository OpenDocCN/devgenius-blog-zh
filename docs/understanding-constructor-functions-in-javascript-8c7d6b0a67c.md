# 理解 JavaScript 中的构造函数

> 原文：<https://blog.devgenius.io/understanding-constructor-functions-in-javascript-8c7d6b0a67c?source=collection_archive---------2----------------------->

## 通过实例了解 JavaScript 中的构造函数

![](img/0a4b6a7785b33c3db5e52ca610bff512.png)

查尔斯·德鲁维奥在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在本文中，您将借助示例了解 JavaScript 构造函数。让我们开始吧。

# 什么是构造函数？

在 JavaScript 中，构造函数用于创建新对象。它定义了属于新对象的属性和行为。可以把它看作是创建新对象的蓝图。

下面是一个构造函数的示例:

```
function **C**ar() {
  this.name = "Ford";
  this.color = "blue";
  this.speed = 260;
}
```

这个构造函数定义了一个带有一些属性的`**C**ar` 对象(name-color-speed)。

构造函数遵循一些约定:

*   构造函数用一个大写的名字来定义，以区别于其他不是`constructors`的函数。
*   构造函数使用关键字`this`来设置他们将要创建的对象的属性。在构造函数内部，`this`指的是它将要创建的新对象。
*   构造函数定义属性和行为，而不是像其他函数那样返回值。

# 使用构造函数创建对象

现在，我们可以用一个简单的构造函数来创建对象。看看下面的例子:

下面是一个`Bird`构造函数的例子，它允许我们创建新的对象。

```
function Bird() {
  this.name = "Albert";
  this.color  = "blue";
  this.numLegs = 2;
  // "this" inside the constructor always refers to the object being created
}

let blueBird = new Bird();
```

注意，在调用构造函数时使用了操作符`new`。这告诉 JavaScript 创建一个名为`blueBird`的`Bird`构造函数的新实例。如果没有`new`操作符，构造函数中的`this`将不会指向新创建的对象，从而产生意想不到的结果。

现在`blueBird`已经在`Bird`构造函数中定义了所有属性:

```
blueBird.name; // => Albert
blueBird.color; // => blue
blueBird.numLegs; // => 2
```

就像任何其他对象一样，可以访问和修改它的属性。

# 扩展构造函数以接收参数

您可以设计您的构造函数来接受参数。看看下面的例子:

```
function Bird(name, color) {
  this.name = name;
  this.color = color;
  this.numLegs = 2;
}
```

现在您可以使用这个构造函数创建新的对象。看看下面的例子:

```
let bird1 = new Bird("Rex", "Blue");
let bird2 = new Bird("Carlos", "White");
```

构造函数更加灵活。现在可以在创建时为每个`Bird`定义属性，这是 JavaScript 构造函数如此有用的一个原因。这些构造函数可以根据共享的特征将对象组合在一起，并定义一个自动创建它们的蓝图。

# JavaScript 对象原型

您还可以使用一个**原型**向一个构造函数添加属性和方法。看看下面的例子:

```
// constructor function
function Person () {
    this.name = 'John',
    this.age = 23
}

// creating objects
let person1 = new Person();
let person2 = new Person();

// adding new property to constructor function
Person.prototype.gender = 'Male';

console.log(person1.gender); // Male
console.log(person2.gender); // Male
```

# 结论

如您所见，这只是对 JavaScript 中构造函数的简单介绍。我鼓励你从其他资源中学习更多关于构造函数的知识。

感谢您阅读本文，希望您觉得有用。

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/better-javascript-class-inheritance-e6c3d8206003) [## 更好的 JavaScript —类继承

### 用实例理解 JavaScript 中的类继承

medium.com](https://medium.com/javascript-in-plain-english/better-javascript-class-inheritance-e6c3d8206003)