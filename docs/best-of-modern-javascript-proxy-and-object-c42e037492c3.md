# 现代 JavaScript 的精华——代理和对象

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-proxy-and-object-c42e037492c3?source=collection_archive---------3----------------------->

![](img/beae8b75803e96275258c4bbf4da994f.png)

照片由[ярославалексеенко](https://unsplash.com/@webaliser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究使用 JavaScript 代理的元编程。

# 可撤销的代理

我们可以用 ES6 创建可撤销的代理。

为此，我们调用带有`target`和`handler`参数的`Proxy.revocable`方法。

例如，我们可以写:

```
const target = {};
const handler = {
  get(target, propKey, receiver) {
    return 'foo';
  }, has(target, propKey) {
    return true;
  }
};
const {
  proxy,
  revoke
} = Proxy.revocable(target, handler);
```

`Proxy.revocable`函数接受我们想要改变其行为的`target`对象。

`handler`是一个对象，它有各种方法让我们控制`target`的行为。

它返回一个带有`proxy`对象和`revoke`函数的对象来撤销代理。

我们可以调用`revoke`停止使用代理。

例如，如果我们有:

```
const target = {};
const handler = {
  get(target, propKey, receiver) {
    return target[propKey];
  },has(target, propKey) {
    return true;
  }
};
const {
  proxy,
  revoke
} = Proxy.revocable(target, handler);proxy.foo = 'bar';
console.log(proxy.foo);revoke();console.log(proxy.foo);
```

然后第二个控制台日志会给我们一个错误，因为代理已经用`revoke`函数撤销了。

因此，我们得到“未捕获的类型错误:无法在已撤销的代理上执行“get”。

# 作为原型的代理

我们可以使用代理作为对象的原型。

例如，我们可以写:

```
const target = {};
const handler = {
  get(target, propKey, receiver) {
    return target[propKey];
  }, has(target, propKey) {
    return true;
  }
};
const proto = new Proxy(target, handler);
const obj = Object.create(proto);
```

使用`Object.create`方法创建一个代理对象并将其作为原型。

# 转发截获的操作

我们可以用代理转发截获的操作。

例如，我们可以写:

```
const target = {};
const handler = {
  deleteProperty(target, propKey) {
    return delete target[propKey];
  },
  has(target, propKey) {
    return propKey in target;
  },
}const proto = new Proxy(target, handler);
```

增加前进`delete`操作和`in`操作。

我们只是在没有代理的情况下在`handler`方法中做同样的事情。

# 包裹物体会影响`this`

由于`handler`是一个对象，它有自己的值`this`。

因此，如果我们有:

```
const target = {
  foo() {
    console.log(this === target)
    console.log(this === proxy)
  }
};
const handler = {};
const proxy = new Proxy(target, handler);proxy.foo()
```

然后第一个控制台日志是`false`，第二个是`true`，因为我们在`proxy`上调用了`foo`。

另一方面，如果我们有:

```
const target = {
  foo() {
    console.log(this === target)
    console.log(this === proxy)
  }
};
const handler = {};
const proxy = new Proxy(target, handler);target.foo()
```

那么第一个控制台日志是`true`，第二个是`false`。

# 无法透明包装的对象

如果一个对象有一些私有数据，那么代理就不能透明地包装这个对象。

例如，如果我们有:

```
const _name = new WeakMap();
class Animal {
  constructor(name) {
    _name.set(this, name);
  }
  get name() {
    return _name.get(this);
  }
}
```

那么如果我们有:

```
const mary = new Animal('mary');
const proxy = new Proxy(mary, {});
console.log(proxy.name);
```

我们得到了`undefined`中的`name`属性。

这是因为`name`存储在 WeakMap 中，而不是作为`this`本身的直接属性。

`this`是代理，所以它没有`name`属性。

![](img/d06219541898386ed35a15ef02c910d0.png)

照片由 [Karin Hiselius](https://unsplash.com/@silverkakan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

代理是可撤销的，代理和原始对象之间的`this`值是不同的。