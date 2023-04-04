# 面向对象 JavaScript 简介

> 原文：<https://blog.devgenius.io/introduction-to-object-oriented-javascript-caf89ec986ab?source=collection_archive---------8----------------------->

![](img/48f28bb1a6f24cd20639e01cbb08e5d9.png)

照片由[迈克·多纳](https://unsplash.com/@dorner?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将介绍 JavaScript 的面向对象部分。

# 面向对象编程和 JavaScript

面向对象编程语言有一些共同的特征。

它们是:

*   对象、方法和属性
*   班级
*   包装
*   聚合
*   可重用性/继承性
*   多态性

JavaScript 拥有这些特性中的大部分。

然而，JavaScript 没有类，尽管它有类语法。

类语法只是基于原型的继承模型之上的语法糖。

然而，它确实有其他部分。

我们可以用 JavaScript 以各种方式定义对象。

对象可以有属性和方法。

JavaScript 中的方法可以是构造函数或类的一部分。

或者它可以是对象文字的一部分。

我们可以通过继承和合并对象来使用代码。

此外，只要接口可以替换，我们就可以有多态性。

# 目标

对象是 JavaScript 面向对象编程的基本构件，

对象是某物的表示。

这东西可以是任何东西。

它可以模拟现实世界的物体，也可以表现更抽象的东西。

它们有颜色、名称、重量等特征。

物体通常用弹跳来命名，比如书，汽车等等。

方法用动词命名，因为它们做一些事情。

价值观是形容词。

# 班级

JavaScript 没有真正的类。

但是它有类语法。

JavaScript 继承是通过原型完成的，原型是其他对象继承的对象。

此外，JavaScript 有看起来像类的构造函数。

它们可以像类一样用`new`关键字实例化，但是它们是我们可以返回任何东西的函数。

它们可以有原型，以便所有实例共享相同的方法。

我们可以通过编写以下代码来创建构造函数:

```
function Person() {
  //...
}
```

这与以下内容相同:

```
class Person {
  //...
}
```

我们可以通过编写来编写方法:

```
function Person() {
  //...
}Person.prototype.eat = function(){
  //...
}
```

使用类语法，我们编写:

```
class Person {
  //...
  eat(){
    //...
  }
}
```

# 包装

封装是另一个面向对象的编程概念。

对象保存存储在属性中的数据。

他们可以通过方法对数据做一些事情。

封装也意味着我们只向外部公开绝对必要的东西。

每个部分对彼此了解得越少越好。

这是因为我们不希望一个部分在变化时影响到其他部分。

例如，我们不想知道第三方库内部是如何工作的。

我们只想使用它们公开的接口。

这样我们就不能在自己的 app 里使用库的内部了。

因此，如果库发生变化，这种变化不太可能会破坏我们的应用程序

许多面向对象的编程语言对类变量有不同的访问级别。

像 JavaScript 这样的一些语言可能有`public`、`private`和`protected` 方法。

JavaScript 没有访问控制。

Wd 只能在函数和模块中保存私有数据。

![](img/b5e1500fea5f2092ec72e063da4fa45d.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Berkay Gumustekin](https://unsplash.com/@berkaygumustekin?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

JavaScript 部分是面向对象的语言。

它能做很多他们能做的事情。

但是值得注意的是，缺少的东西包括类和私有或受保护的实例变量。