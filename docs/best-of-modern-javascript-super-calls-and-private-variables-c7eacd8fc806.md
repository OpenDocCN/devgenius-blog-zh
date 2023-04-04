# 现代 JavaScript 的精华——超级调用和私有变量

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-super-calls-and-private-variables-c7eacd8fc806?source=collection_archive---------3----------------------->

![](img/10ead5811689cb9a0b0d9ed347d650fa.png)

由[卡伊奥·席尔瓦](https://unsplash.com/@caiohenriquesilva?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看如何用 JavaScript 定义类。

# 超级构造函数调用

在调用任何其他东西之前，我们必须调用`super`。

例如，我们不能写:

```
class Foo {}class Bar extends Foo {
  constructor(foo) {
    this.foo = foo;
    super();
    this.bar = bar;
  }
}
```

第一行必须是`super`调用。

相反，我们写道:

```
class Foo {}class Bar extends Foo {
  constructor(foo) {    
    super();
    this.foo = foo;
    this.bar = bar;
  }
}
```

删除`super`调用也会给我们一个错误。所以我们不能写:

```
class Foo {}class Bar extends Foo {
  constructor() {}
}
```

# 重写构造函数的结果

我们可以通过在`constructor`中返回我们想要的东西来覆盖构造函数的结果。

例如，我们可以写:

```
class Foo {
  constructor() {
    return {};
  }
}
```

然后当我们记录时:

```
console.log(new Foo() instanceof Foo);
```

我们让`false`回来了。

不管`this`是否被初始化，因为我们正在返回一个对象，而不是在我们的构造函数中隐式返回`this`。

如果我们像示例中那样覆盖结果，我们不必在子构造函数中调用`super`。

# 类的默认构造函数

如果我们不放任何东西进去，我们不需要指定一个空的构造函数。

所以如果我们有:

```
constructor() {}
```

我们可以移除它。

对于派生类，我们不需要仅仅为了调用超级构造函数而添加一个构造函数。

所以我们不必写:

```
constructor(...args) {
  super(...args);
}
```

在我们的代码中。

# 子类化内置构造函数

我们可以创建内置构造函数的子类。

例如，我们可以写:

```
class SomeError extends Error {}
throw new SomeError('error');
```

我们用关键字`extends`创建了一个`Error`的子类。

然后我们可以像任何其他的`Error`实例一样抛出它。

我们也可以创建`Array`构造函数的子类。

例如，我们可以写:

```
class Stack extends Array {
  get first() {
    return this[0];
  }
}
```

然后我们可以创建一个新的`Stack`实例，并使用可用的`Array`属性:

```
class Stack extends Array {
  get first() {
    return this[0];
  }
}const stack = new Stack();
stack.push('foo');
stack.push('bar');
console.log(stack.first);
console.log(stack.length);
```

我们调用`pusg`将条目推送到我们的`Stack`实例。

然后我们得到`first`和`length`属性。

`first`是我们定义的 getter。

而`length`是继承自`Array`。

# 类的私有数据

JavaScript 类没有私有成员。

如果我们想要私人数据，那么我们必须把它们藏在别的地方。

或者我们可以创建带有特殊命名方案的公共成员来表明它们是私有的。

我们可以在属性前加一个下划线来表示它们是私有的。

比如，我们可以写；

```
class Foo {
  constructor() {
    this._count = 0;
  }
}
```

我们添加了`this._count`实例属性来表明`count`是私有的。

我们也可以用弱地图和符号存储私有属性。

例如，我们可以写:

```
const _count = new WeakMap();class Counter {
  constructor(count) {
    _count.set(this, count);
  } increment() {
    let count = _count.get(this);
    count++;
    _count.set(this, count);
  }
}
```

我们创建了两个弱贴图，并使用`this`作为这两个弱贴图的键。

这些值被设置为我们传递给构造函数的值。

然后我们可以用弱贴图的`get`方法得到值。

并用`set`方法设置该值。

弱映射很有用，因为我们可以用`this`访问值，防止里面的项目被其他方式访问。

![](img/13d5ea2c73c08495afea80ee47570ad3.png)

照片由[戴恩·托普金](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

当我们调用`super`时，有几件事我们必须考虑。

此外，没有简单的方法来保持变量私有。