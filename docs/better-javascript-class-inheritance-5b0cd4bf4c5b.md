# 更好的 JavaScript —类继承

> 原文：<https://blog.devgenius.io/better-javascript-class-inheritance-5b0cd4bf4c5b?source=collection_archive---------14----------------------->

![](img/7b854aa0e961d484ae87471947592e33.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 从子类构造函数中调用超类构造函数

为了创建一个继承自父构造函数的构造函数，我们需要在子类构造函数中调用超类构造函数。

例如，我们可以写:

```
function Person(name) {
  this.name = name;
}Person.prototype.toString = function() {
  return `Person ${this.name}`;
}function Actor(name, role) {
  Person.call(this, name);
  this.role = role;
}Actor.prototype = Object.create(Person.prototype);
Actor.prototype.constructor = Actor;
```

我们创建了服务于超类构造函数的`Person`构造函数。

实例方法在`prototype`属性中。

通过调用`Person`构造函数创建`Actor`子类，然后设置自己的状态。

然后我们使用`Object.create`创建原型，这样来自`Person`的所有方法都被继承。

然后我们将`constructor` 设置为`Actor`，这样如果我们将`instanceof`与`Actor`实例一起使用，如下所示:

```
new Actor('james', 'main') instanceof Actor
```

那么应该会返回`true`。

由于继承，我们可以在一个`Actor`实例中调用`Person`方法。

如果我们愿意的话，我们可以覆盖它们。

所以如果我们有:

```
const str = new Actor('james', 'main').toString()
```

这是可行的，我们得到`'Person james’`作为`str`的值。

创建超类和子类的一种不太容易出错的方法是使用类语法。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this.name = name;
  } toString() {
    return `Person ${this.name}`;
  }
}class Actor extends Person {
  constructor(name, role) {
    super(name);
    this.role = role;
  }
}
```

而不是我们以前拥有的。

他们是一样的。

只是课程版本更短，出错的机会更少。

`super`替换`Person.call`方法调用。

`extends`与使用`Object.create`并设置`constructor`属性相同。

所以类语法应该被使用，因为它被引入了，但是我们必须知道在它的下面，仍然是原型和构造函数。

# 永远不要重用超类属性名

我们不应该使用超类属性名。

例如，如果我们有:

```
function Person(name) {
  this.name = name;
}Person.prototype.toString = function() {
  return `Person ${this.name}`;
}function Actor(name, role) {
  Person.call(this, name);
  this.name = name;
  this.role = role;
}
Actor.prototype = Object.create(Person.prototype);
Actor.prototype.constructor = Actor;
```

然后我们在`Actor`和`Person`中都使用了`name`。

这会造成两者之间的冲突。

子类应该知道超类使用的所有属性。

所以我们不应该重新定义超类中的属性。

有了类语法，我们可以只使用`super`对象来引用超类的实例属性。

![](img/d45709901d163530e5ef0143a5cc3b24.png)

[昌博科](https://unsplash.com/@kochangbok?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

类语法让我们避免了许多我们可以用子类和超类创建的错误。

此外，子类和超类中不应该有相同的实例属性名。