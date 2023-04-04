# 现代 JavaScript 的精华—生成器函数

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-generator-functions-2ed9ba1e29a8?source=collection_archive---------3----------------------->

![](img/fa9b34ceee04a0bfd2f5058ee1b2ef70.png)

照片由[卡洛·维拉里卡](https://unsplash.com/@sobermusings?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 生成器。

# 通过生成器实现可迭代对象

Iterables 可以用生成器来实现。

我们可以用`Symbol.iterable`方法创建一个可迭代的对象，使它成为可迭代的。

例如，我们可以写:

```
const obj = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  }
}
```

在我们的`Symbol.iterator`方法中有`yield`关键字。

然后我们可以写:

```
for (const x of obj) {
  console.log(x);
}
```

迭代这些值，我们得到:

```
1
2
3
```

`obj[Symbol.iterator]`是一个生成器方法，它生成我们可以迭代的值。

for-使用该方法作为迭代器来获取值。

# 无限迭代

我们可以用生成元来制造无穷大的变量。

例如，我们可以写:

```
const obj = {
  *[Symbol.iterator]() {
    for (let n = 0;; n++) {
      yield n;
    }
  }
}function* take(n, iterable) {
  for (const x of iterable) {
    if (n <= 0) {
      return;
    }
    n--;
    yield x;
  }
}for (const x of take(5, obj)) {
  console.log(x);
}
```

我们的可迭代`obj`对象有生成整数的`Symbol.iterator`方法。

然后我们创建了`take`函数来从`iterable`中产生第一个`n`项。

最后，我们遍历返回的变量。

# 惰性求值的生成器

生成器对于惰性求值很有用。

例如，我们可以创建一个从字符串中产生单个字符的函数。

我们遍历字符串并得到密钥。

例如，我们可以写:

```
function* tokenize(str) {
  for (const s of str) {
    yield s;
  }
}let c;
const gen = tokenize('foobar');while (c = gen.next().value) {
  console.log(c);
}
```

我们可以从对象中获取值。

# 继承和迭代器

我们可以像其他函数一样用生成器函数实现继承。

例如，我们可以通过编写以下代码向生成器添加一个实例方法:

```
function* g() {}
g.prototype.foo = function() {
  return 'bar'
};
const obj = g();
console.log(obj.foo());
```

我们向`prototype`属性添加了`foo`方法。

然后我们调用`g`生成器函数返回生成器。

然后我们可以在它上面调用`foo`方法。

控制台日志应该记录了`'bar'`。

如果我们试图得到迭代器的原型。

例如，我们可以写:

```
const getProto = Object.getPrototypeOf.bind(Object);
console.log(getProto([][Symbol.iterator]()));
```

然后我们看到了`Array Iterator`物体。

它有迭代器原型的`next`方法和其他属性。

# `this`在发电机中

发电机功能有自己的值`this`。

这是一个设置并返回一个生成器对象的函数。

它包含生成器对象单步执行的代码。

我们可以这样写来看`this`的值:

```
function* gen() {
  yield this;
}const [genThis] = gen();
console.log(genThis)
```

如果它在顶层，我们像析构一样得到了`this`的值，我们得到了窗口对象。

这是假设严格模式关闭。

如果严格模式打开:

```
function* gen() {
  'use strict';
  yield this;
}
```

那么`genThis`就是`undefined`。

如果我们的生成器在一个对象中:

```
function* gen() {
  'use strict';
  yield this;
}const obj = {
  gen
}const [genThis] = obj.gen();
console.log(genThis);
```

`genThis`就是物体本身。

生成器函数类似于传统函数。

![](img/20e1064625cfc9d83f1dd814ada76182.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Anastasia Palagutina](https://unsplash.com/@rashevsky?utm_source=medium&utm_medium=referral) 拍摄

# 结论

Iterables 可以用生成器来实现。

同样，我们可以实现继承，并用生成器函数得到`this`的值。