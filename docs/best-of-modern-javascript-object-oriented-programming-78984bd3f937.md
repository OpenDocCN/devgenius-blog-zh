# 现代 JavaScript 的精华——面向对象编程

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-object-oriented-programming-78984bd3f937?source=collection_archive---------4----------------------->

![](img/b069c3e82d51972540f6cd4e76cc684f.png)

由[亚当·格里菲斯](https://unsplash.com/@aggriffith?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# `No More apply()`

在 ES6 之前，我们必须使用`apply`来调用一个以数组 spread 作为参数的函数。

对于 ES6 或更高版本，我们可以使用 spread 操作符，而不是调用函数。

因为我们不必设置`this`的值，所以这更容易。

例如，不写:

```
Math.max.apply(Math, [3, 2, 1, 6])
```

我们写道:

```
Math.max(...[3, 2, 1, 6])
```

这就简单多了，我们不用担心额外的争论。

使用`push`方法，我们可以通过 ES5 或更高版本的`apply`方法推送多个项目:

```
var arr1 = [1, 2];
var arr2 = [3, 4];arr1.push.apply(arr1, arr2);
```

第一个参数是数组实例，我们将其设置为`this`的值。

对于 ES6，我们可以改为编写:

```
const arr1 = [1, 2];
const arr2 = [3, 4];
arr1.push(...arr2);
```

有了 spread 运算符，我们就不必再这样做了。

# `concat() can be Replace by Spread`

我们可以用 ES6 的 spread 操作符代替`concat`。

不要用 ES5 写:

```
var arr1 = [1, 2];
var arr2 = [3, 4];
var arr3 = [5, 6];console.log(arr1.concat(arr2, arr3));
```

我们写道:

```
const arr1 = [1, 2];
const arr2 = [3, 4];
const arr3 = [5, 6];console.log([...arr1, ...arr2, ...arr3]));
```

在 ES6 或更高版本中，我们使用 spread 运算符将所有 3 个数组合并到一起。

两个表达式都返回一个新数组，其中包含所有 3 个数组的元素。

# 对象文字中的函数表达式

在 ES6 或更高版本中，我们不再为对象方法写出`function`关键字。

它有一个方便的语法来定义对象中的方法。

在 ES5 或更早的版本中，我们必须写:

```
var obj = {
  eat: function() {
    //...
  },
  drink: function() {
    this.eat();
  },
}
```

在对象中定义方法。

对于 ES6 或更高版本，我们可以将其缩短为:

```
const obj = {
  eat() {
    //...
  },
  drink() {
    this.eat();
  },
}
```

我们去掉了`function`关键字和冒号，所以它更短了。

他们做同样的事情。

# 类的构造函数

ES6 为我们提供了一个方便的类语法来创建构造函数。

在 ES5 中，我们必须写:

```
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return 'Hello ' + this.name;
};
```

实例方法是通过向`prototype`属性添加一个属性来定义的。

在 ES6 中，我们可以通过编写以下内容来使用类语法:

```
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    return `Hello ${this.name}`;
  }
}
```

`constructor`有构造函数内部的东西。

而`greet`仍然是`Person`的`prototype`属性中的方法。

只是写法不同而已。

# 子类

用 ES5 创建子类很复杂。如果我们有超级构造函数和属性，那就更难了。

要用 ES5 或更早版本创建父类和子类，我们编写:

```
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return 'Hello ' + this.name;
};function Student(name, number) {
  Person.call(this, name);
  this.number = number;
}
Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;
Student.prototype.describe = function() {
  return Person.prototype.greet.call(this) + '-' + this.number;
};
```

我们以同样的方式创建`Person`构造函数。

然后为了创建`Student`子类，我们用`call`方法调用`Person`构造函数，用`this`和`name`调用父构造函数。

然后我们初始化`Student`的实例数据。

然后我们通过用`Person`的原型调用`Object.create`来创建`Student`的原型。

然后我们将`constructor`属性设置为`Student`以获得正确的构造函数。

然后我们将实例方法添加到`Student`类中。

很复杂，很难理解。

使用类语法，我们编写:

```
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    return `Hello ${this.name}`;
  }
}class Student extends Person {
  constructor(name, number) {
    super(name);
    this.number = number;
  }
  describe() {
    return `${Person.prototype.greet.call(this)}-'${this.number}`;
  }
}
```

我们所要做的就是使用`extends`关键字来表明它是一个子类。

并且我们调用`super`来调用父构造函数。

![](img/cc1e492969875435b77c839ac222fe70.png)

席琳·艺伎回忆录·塔加米在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

使用 ES6 或更高版本，面向对象编程更容易。