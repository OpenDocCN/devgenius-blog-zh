# 面向对象的 JavaScript —类继承

> 原文：<https://blog.devgenius.io/object-oriented-javascript-class-inheritance-5c5cf7dcdf8f?source=collection_archive---------10----------------------->

![](img/dead80a7c6663f7bd84d4af0b967cc4c.png)

照片由 [Matteo Maretto](https://unsplash.com/@matmaphotos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 JavaScript 子类、混合和多重继承。

# 子类

我们可以从 JavaScript 类创建子类。

例如，我们可以写:

```
class Animal {
  constructor(name) {
    this.name = name;
  } speak() {
    console.log(`${this.name} speaks`);
  }
}class Dog extends Animal {
  speak() {
    console.log(`${this.name} woofs`);
  }
}const mary = new Dog('mary');
```

创建`Animal`类。

`speak`方法在`Animal`类中。

我们创建了一个名为`Dog`的`Animal`的子类，它也有`speak`方法。

如果我们创建一个`Dog`实例，那么将使用`Dog`中的`speak`方法。

所以如果我们写:

```
mary.speak();
```

我们看到:

```
mary woofs
```

有几种方法可以访问一个类的超类。

我们可以调用`super()`调用超类，传入父构造函数的参数。

同样，我们可以调用`super.<parentClassMethod>`来调用父类的方法。

`super.<parentClassProp>`让我们访问父类属性。

所以如果我们有:

```
class Parent {}class Child extends Parent {
  constructor(name) {
    this.name = name;
  }
}const child = new Child('james')
```

我们将得到错误:

```
Uncaught ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
```

这是因为我们需要先调用`super`来调用父构造函数，然后才能设置`this`的属性。

如果我们省略`Child`中的`constructor`，这可以隐式完成。

如果我们有:

```
class Parent {}class Child extends Parent {}const child = new Child()
```

那么我们不会得到一个错误，因为提供了一个默认的构造函数。

默认值为:

```
constructor(...args) {
  super(...args);
}
```

# 混合蛋白

JavaScript 只支持单一继承，所以我们不能使用标准的 JavaScript 语法从多个类继承属性。

相反，我们必须用函数将多个类组合成一个类。

我们可以写:

```
class Person {}const BackgroundCheck = Tools => class extends Tools {
  check() {}
};const Onboard = Tools => class extends Tools {
  createEmail() {}
};class Employee extends BackgroundCheck(Onboard(Person)) {}
```

我们有返回`Tools`类的子类的`BackgroundCheck`类。

所以`Person`是`Onboard`函数中的`Tools`。

而`Onboard(Person)`是`BackgroundCheck`功能中的`Tools`。

这样，我们返回一个既有`check`又有`createEmail`方法的类，我们可以使用得到的类来创建`Employee`子类。

![](img/39f89ee6b6cd7fa7f1fbcc1323682276.png)

由 [Viacheslav Bublyk](https://unsplash.com/@s1winner?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用`extends`关键字创建子类。

此外，我们可以创建返回一个类的函数并组合它们，这样我们就可以从多个类继承。