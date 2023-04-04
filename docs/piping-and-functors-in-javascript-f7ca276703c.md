# Javascript 中的管道和函子

> 原文：<https://blog.devgenius.io/piping-and-functors-in-javascript-f7ca276703c?source=collection_archive---------4----------------------->

![](img/5f9bcbcc682c4cab0fa9caf863a9029d.png)

JavaScript 部分是一种函数式语言。

学习 JavaScript，就要学习 JavaScript 的功能部分。

在本文中，我将讨论 JavaScript 的管道函数和函子。

# 管

您可以通过创建一个以函数数组作为参数的函数来创建一个`pipe`函数。

它返回一个带值的函数，你用它调用`reduce`，用`acc`调用`fn`。

例如，您可以写:

```
const compose = (...fns) =>
  (value) =>
  fns.reduceRight((acc, fn) => fn(acc), value)
```

唯一的区别是你使用了`reduceRight`而不是`reduce`，这样你就不需要调用`reverse`来应用所有的功能。

# 构图是联想的

组合是关联的，这意味着您可以重新排列操作的括号

例如:

```
compose(f, compose(g, h)) == compose(compose(f, g), h)
```

返回`true`。

# 函子

Functor 是一个普通对象，它在运行 River each value 以生成新对象时实现函数`map`。

函子是一个在其中保存一些值的容器。

例如，您可以写:

```
const Container = function(val) {
  this.value = val;
}
```

创建您的容器。

然后您可以使用`new`来调用`Container`构造函数:

```
let foo = new Container(3);
```

您可以创建一个`Container.of`属性来添加一个容器，让我们返回一个`Container`实例:

```
const Container = function(val) {
  this.value = val;
}Container.of = function(value) {
  return new Container(value);
}
```

您添加了静态方法`of`来返回一个`Container`实例。

`of`方法只是为您提供了一种使用`new`操作符创建`Container`实例的替代方法。

然后您可以用`of`方法创建一个`Container`实例:

```
const nested = Container.of(Container.of(1));
```

然后您将看到嵌套的`Container`实例。

# 仿函数实现名为 map 的方法

函子实现了`map`方法。

您可以通过将它添加到`prototype`属性来将其作为实例方法添加:

```
const Container = function(val) {
  this.value = val;
}Container.of = function(value) {
  return new Container(value);
}Container.prototype.map = function(fn) {
  return Container.of(fn(this.value));
}
```

然后，您可以在创建一个`Container`实例后使用`map`方法。

例如，您可以通过编写以下内容来使用它:

```
const squared = Container.of(3).map(a => a ** 2);
```

那么`squared`就是`value`为 9 的`Container`实例。

可以反复调用`map`来重复一个操作。

这意味着你可以写:

```
const squared = Container.of(3)
  .map(square)
  .map(square)
  .map(square);
```

那么`squared`就是一个`Container`实例，其中`value`为 6561。

Functor 只是一个实现了`map`方法的对象。

# 结论

函子是有值的普通对象。

该对象有一个`map`方法。

您可以通过管道函数来调用多个函数并获得返回值。

组合是关联的，所以你可以重新安排我们操作的括号。

今天就到这里，谢谢大家的关注。