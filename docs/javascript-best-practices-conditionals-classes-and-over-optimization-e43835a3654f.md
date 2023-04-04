# JavaScript 最佳实践——条件、类和过度优化

> 原文：<https://blog.devgenius.io/javascript-best-practices-conditionals-classes-and-over-optimization-e43835a3654f?source=collection_archive---------7----------------------->

![](img/12cfcb9a66b7fa6b078d5914fe4333b1.png)

安娜·席尔瓦在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 避免条件句

我们可以避免条件句，代之以多态。

例如，不写:

```
class Person {
  // ...
  getData() {
    switch (this.type) {
      case "student":
        return this.getStudentData();
      case "employee":
        return this.getEmployeeData();
      case "manager":
        return this.getManagerData();
    }
  }
}
```

我们可以将它们分成多个子类:

```
class Person {
  // ...
}

class Student extends Person {
  // ...
  getData() {
    return this.getStudentData();
  }
}

class Employee extends Person {
  // ...
  getData() {
    return this.getEmployeeData();
  }
}

class Manager extends Person  {
  // ...
  getData() {
    return this.getManagerData();
  }
}
```

我们将它们分成多个子类，而不是一个做多件事的大类。

共享代码在`Person`里。

Thes 休息是在方法专属的子类。

`this.getStudentData`、`this.getEmployeeData`、`this.getManagerData`可以留在子类中。

# 不要过度优化

如果我们不得不牺牲可读性，我们不应该过度优化我们的代码。

例如，我们不必缓存循环长度。

在现代浏览器中，它不会带来太多的性能差异，但我们使我们的代码更难阅读。

而不是写:

```
for (let i = 0, len = items.length; i < len; i++) {
  // ...
}
```

我们写道:

```
for (let i = 0; i < items.length; i++) {
  // ...
}
```

或者只使用 for-of 循环:

```
for (const item of items) {
  // ...
}
```

# 删除死代码

我们应该删除无用的代码，因为它们没有被使用。

例如，代替写作

```
const oldRequest = (params) => {
  //...
}const newRequest = (params) => {
  //...
}newRequest({
  //...
});
```

我们写道:

```
const newRequest = (params) => {
  //...
}newRequest({
  //...
});
```

# 使用 Getters 和 Setters

我们应该在对象上使用 getters 和 stress。

他们让我们在获取和设置数据之前进行验证。

我们可以封装任何在获取之前运行的逻辑。

日志记录和错误处理更容易，我们可以在 getters 中延迟加载我们的东西。

例如，不写:

```
function makeCart() {
  // ...

  return {
    cart: []
    // ...
  };
}
```

我们写道:

```
function makeCart() {
  let cart = []; function getCart() {
    return cart;
  } function setCart(newCart) {
    cart = newCart;
  }

  return {
    // ...
    getCart,
    setCart
  };
}
```

我们创建一个带有私有`cart`和`getCart` getter 函数和`setCart` setter 函数的购物车。

我们在代码中公开我们想要的。

现在我们可以通过书写来使用它:

```
const cart = makeCart();
cart.setBalance(cartItems);
```

# 使对象具有私有成员

我们可以使用闭包来保存对象中的私有成员。

例如，不写:

```
const person = {
  name: 'james',
  age: 20
}
```

我们写道:

```
const person = (() => {
  const age = 20;
  return {
    name: 'james'
  }
})();
```

我们用`name`而不是`age`返回一个对象。

隐藏在函数中。

这是保存私有变量的方法之一。

模块成员也是私有的，除非我们导出它们。

# 优先使用类语法而不是构造函数

构造函数增加了继承的难度。

要进行继承，我们必须继承方法，并将实例方法添加到构造函数的原型中。

例如，我们写道:

```
const Animal = function(name) {
  this.name = name;
};

Animal.prototype.move = function() {};

const Dog = function(name) {
  Animal.call(this, name);
  this.age = age;
};

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function() {};
```

我们必须创建`Animal`构造函数，并向其原型添加实例方法。

然后我们创建`Dog`构造函数，并用参数调用`Animal`构造函数。

然后我们用`Object.create`继承了`Animal`原型的方法。

然后我们将它的`Prototype`的`constructor`属性设置为自身，以便在使用`instanceof`时得到正确的结果。

然后我们添加`Dog`专有的实例方法。

有了类语法，就简单多了。

例如，我们写道:

```
class Animal {
  constructor(name) {
    this.name = name;
  }

  move() {
    /* ... */
  }
}

class Dog extends Animal {
  constructor(age) {
    super(name);
    this.age = age;
  }

  bark() {
    /* ... */
  }
}
```

我们把构造函数和方法都放在类里面。

我们用`super`调用父构造函数，

而我们`extends`来表示继承。

![](img/08fa788b805f841c640a1e126dfabfe6.png)

由[维克多 B.](https://unsplash.com/@vbchr?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

类语法使得继承更加容易。

条件句可以用多态代替。

如果优化没有带来太多好处，让我们的代码更难读，那就不要做。