# 函数式 JavaScript —单子

> 原文：<https://blog.devgenius.io/functional-javascript-monads-dd7416cc2b8e?source=collection_archive---------2----------------------->

![](img/818973de63c905b8bc1bc817088ebdc3.png)

照片由[杰米街](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

JavaScript 部分是一种函数式语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将看看如何用 JavaScript 来传输函数和函子。

# 也许是函子

一个可能函子是让我们以不同的方式实现一个`map`函数的函子。

我们首先创建一个存储值的构造函数:

```
const MayBe = function(val) {
  this.value = val;
}MayBe.of = function(val) {
  return new MayBe(val);
}
```

然后我们添加`MayBe`仿函数特有的方法。

我们有`isNothing`方法来检查`this.value`是否有问题。

根据`this.value`是否有东西，`map`方法将返回不同的东西。

我们补充:

```
MayBe.prototype.isNothing = function() {
  return (this.value === null || this.value === undefined);
};MayBe.prototype.map = function(fn) {
  return this.isNothing() ? MayBe.of(null) : MayBe.of(fn(this.value));
};
```

我们共同拥有:

```
const MayBe = function(val) {
  this.value = val;
}MayBe.of = function(val) {
  return new MayBe(val);
}MayBe.prototype.isNothing = function() {
  return (this.value === null || this.value === undefined);
};MayBe.prototype.map = function(fn) {
  return this.isNothing() ? MayBe.of(null) : MayBe.of(fn(this.value));
};
```

然后我们可以通过写来使用它:

```
const str = MayBe.of("foo").map((x) => x.toUpperCase())
```

然后我们得到`MayBe`实例的`value`属性是`'FOO'`。

如果`this.value`为`null`或`undefined`，那么`map`将返回一个`MayBe`函子，其中`value`为`null`。

所以如果我们有这样的东西:

```
const str = MayBe.of("james")
  .map(() => undefined)
  .map((x) => `Mr. ${x}`)
```

我们将得到`value`的最终值为`null`，而不是抛出一个错误。

# 任一函子

`Either`仿函数允许我们用分支来解决问题。

我们创建一个`Nothing`或`Some`函子，并在一个对象中取出它们。

所以我们写道:

```
const Nothing = function(val) {
  this.value = val;
};Nothing.of = function(val) {
  return new Nothing(val);
};Nothing.prototype.map = function(f) {
  return this;
};const Some = function(val) {
  this.value = val;
};Some.of = function(val) {
  return new Some(val);
};Some.prototype.map = function(fn) {
  return Some.of(fn(this.value));
}
```

现在如果想保存一些数据，那么我们可以使用`Some`函子。

否则，我们使用`Nothing`仿函数来保存一些不存在的值。

# 单子

单子是一个带有`chain`方法的函子。

如果`this.value`有值，`chain`方法调用`join`方法来调用它返回一个`MayBe`实例。

例如，我们可以写:

```
const MayBe = function(val) {
  this.value = val;
}MayBe.of = function(val) {
  return new MayBe(val);
}MayBe.prototype.isNothing = function() {
  return (this.value === null || this.value === undefined);
};MayBe.prototype.map = function(fn) {
  return this.isNothing() ? MayBe.of(null) : MayBe.of(fn(this.value));
};MayBe.prototype.join = function() {
  return this.isNothing() ? MayBe.of(null) : this.value;
}MayBe.prototype.chain = function(f) {
  return this.map(f).join()
}
```

`join`方法检查`this.value`是`null`还是`undefined`。

如果是，那么我们返回一个`null` `MayBe`函子。

否则，我们返回`this.value`。

`chain`就是把`map`和`join`合在一起。

这样，如果我们将某个东西映射到`null`，那么它将保持`null`。

那么我们可以这样写:

```
let mayBe = MayBe.of({
  data: [{
    title: 'foo',
    children: [{
      bar: 2
    }]
  }]
})let ans = mayBe.map((arr) => arr.data)
  .chain((obj) => map(obj, (x) => {
    return {
      title: x.title
    }
  }))
```

然后我们从传递给`of`的对象中获取`title`。

![](img/719f33ae4ff44124cdcb12bd9bcdfb1f.png)

[约翰·普莱斯](https://unsplash.com/@johnprice?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

单子是一个具有`chain`方法的函子，它执行映射和连接。