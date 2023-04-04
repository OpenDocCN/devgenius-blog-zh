# 可维护的 JavaScript —空值和对象检查

> 原文：<https://blog.devgenius.io/maintainable-javascript-null-and-object-checks-7a3449380eef?source=collection_archive---------7----------------------->

![](img/8e612f4ee7661b9d20864173ef107d76.png)

卢卡斯·法夫尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看如何检查各种数据来了解创建可维护的 JavaScript 代码的基础。

# 类型 of 和 null

如果操作数是`null`，则`typeof`运算符返回`'object'`。

这是一个我们不能使用`typeof`来检查的原始值。

与`null`对比通常不能给我们足够的信息。

然而，如果我们使用`===`和`!==`操作符，我们可以检查`null`。

例如，我们可以写:

```
const element = document.querySelector("div");
if (element !== null) {
  element.className = "found";
}
```

检查具有给定选择器的元素是否存在。

如果元素不存在，则`querySelector`返回`null`。

所以我们可以使用`!==`或`===`操作符来检查这个值。

# 检测参考值

我们不能使用`typeof`操作符来检查对象。

因此，我们需要另一种方法来检查它们。

对象包括对象文字和从类似`Object`、`Array`、`Date`和`Error`的构造函数创建的对象。

如果我们使用`typeof`操作符，它总是返回`'object'`。

这意味着它对我们没有多大用处。

`typeof null`也返回`'object'`，所以它对检查`null`也没有用。

相反，我们可以使用`instanceof`操作符来检测特定引用类型的值。

`instanceof`的一般语法是:

```
value instanceof constructor
```

其中`value`是我们要检查的值。

而`constructor`是我们要检查`value` os 是否从其创建的构造函数。

例如，我们可以用它来检测一个`Date`实例，方法是:

```
if (date instanceof Date) {
  console.log(date.getFullYear());
}
```

`instanceof`不仅检查用于创建对象的构造函数。

它还检查原型链上的构造函数。

由于几乎所有的对象都继承自`Object`，`date instanceof Object`也返回`true`。

因此，我们不能使用`value instanceof Object`来检查特定类型的对象。

`instanceof`操作符也可以处理我们自己定义的自定义类型。

例如，我们可以写:

```
function Person(name) {
  this.name = name;
}const jane = new Person("jane");
console.log(jane instanceof Object);
console.log(jane instanceof Person);
```

那么两个控制台日志都将是`true`。

第一个是`true`，因为`jane`继承了`Object.prototype`。

第二个是`true`，因为`jane`是从`Person`构造函数创建的。

`instanceof`操作符是在 JavaScript 中检测定制类型的唯一方法。

然而，它有一个限制。

如果我们把一个物体从一帧传递到另一帧。

如果我们有:

```
frameAPerson instanceof frameAPerson
```

然后返回`true`。

但是如果我们有:

```
frameAPerson instanceof frameBPerson
```

然后返回`false`。

每个帧都有自己的`Person`副本，而`instanceof`只考虑当前帧中的`Person`。

即使它们是相同的，情况也是如此。

![](img/e0d0c850660213078481d40bf5108d04.png)

[格兰特·里奇](https://unsplash.com/@grantritchie?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

`instanceof`操作符让我们检查各种对象。

唯一不能检查的是从不同帧传递过来的。