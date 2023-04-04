# JavaScript 最佳实践—类和类型

> 原文：<https://blog.devgenius.io/javascript-best-practices-classes-and-types-4f43c91938cf?source=collection_archive---------23----------------------->

![](img/98c4d22c6be7ad7e8d7b5141cb68cb1a.png)

照片由 [Waranya Mooldee](https://unsplash.com/@anyadiary?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 类型检查

我们可以编写一致的 API 来避免类型检查。

而不是写:

```
function travelToNewYork(vehicle) {
  if (vehicle instanceof Airplane) {
    vehicle.fly(this.currentLocation, 'new york');
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, 'new york');
  }
}
```

我们写道:

```
function travelToNewYork(vehicle) {
  vehicle.move(this.currentLocation, 'new york');
}
```

我们有一个`move`方法，而不是`fly`和`drive`方法。

# 使用方法链接

我们可以在我们的方法中返回`this`来使慈善。

例如，不写:

```
class Cube {
  constructor(length, width, height) {
    this.length = length;
    this.width = width;
    this.height = height;
  }

  setHeight(height) {
    this.height = height;
  }

  setLength(length) {
    this.length = length;
  }

  setWidth(width) {
    this.width = width;
  }

  save() {
    console.log(this.height, this.length, this.width);
  }
}
```

我们写道:

```
class Cube {
  constructor(length, width, height) {
    this.length = length;
    this.width = width;
    this.height = height;
  }

  setHeight(height) {
    this.height = height;
    return this
  }

  setLength(length) {
    this.length = length;
    return this;
  }

  setWidth(width) {
    this.width = width;
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    return this;
  }
}
```

然后我们可以通过写来使用它:

```
new Cube(1, 2, 3).setWidth(2).setLength(4).setHeight(6);
```

# 继承之上的组合

我们应该构造类而不是继承它们。

子类没有组合类灵活。

例如，不写:

```
class Employee {
  constructor(name) {
    this.name = name;
  }

  // ...
}

class EmployeePayrollData extends Employee {
  constructor(salary) {
    super();
    this.salary = salary;
  }

  // ...
}
```

`EmployeePayrollData`不需要`Employee`的内容，所以不应该写这个。

相反，我们可以写:

```
class EmployeePayrollData {
  constructor(salary) {
    super();
    this.salary = salary;
  }

  // ...
}class Employee {
  constructor(name) {
    this.name = name;
  }

  setPayrollData(data){
    this.payrollData = new EmployeePayrollData(data);
  }
}
```

这两个类之间没有关系，所以不需要`extends`。

我们只是在我们喜欢的地方使用它们。

# 单一责任原则

每个类都应该有一个单一的责任。

例如，我们的`Employee`类只保存员工数据并执行员工操作:

```
class Employee {
  constructor(name) {
    this.name = name;
  }

  //...
}
```

如果我们想添加与员工无关的功能，那么就把它们放在别的地方。

# 开/关原则

开放/封闭原则意味着一段代码对于扩展是开放的，对于修改是封闭的。

这意味着我们应该能够在不改变现有代码的情况下添加新功能。

创建子类是添加功能的好方法:

```
class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this
      .adapter
      .request(url)
      .then(response => {
        // ...
      });
  }
}class NodeRequester extends HttpRequester{
  constructor() {
    super();
  }

  request(url) {
    // do node request
  }
}

class AjaxRequester extends HttpRequester {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // do ajax request
  }
}
```

# 利斯科夫替代原理

Liskov 替换原则指出，我们应该能够用子类替换父类，并获得相同的结果。

这意味着我们将共享代码保存在父类中。

例如，我们写道:

```
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  } getArea() {
    return this.width * this.height;
  }
}
```

我们有`Rectangle`类，它有与`Square`类相同的方法。

他们共享相同的计算。

唯一的区别是正方形的宽度和高度是一样的。

我们可以用一个`Square`实例替换一个`Rectangle`，并且仍然设置宽度和高度并获得面积。

![](img/4a08a90ec6f655b2f9a4dacccd580b06.png)

由[古斯塔沃·赞贝利](https://unsplash.com/@zamax?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们应该使我们的 API 一致，以避免类型检查。

我们可以在类方法中返回`this`,使它们可以链接。

类应该有一个单一的职责，子类应该能够被用来代替父类。