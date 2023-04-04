# 现代 JavaScript 的精华——可关闭迭代器

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-closable-iterators-a1dfae05e3cd?source=collection_archive---------6----------------------->

![](img/d8c4f313c7d87b8a6b8e18bf8a0414cf.png)

马克·卡马洛夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 可迭代对象。

# 可关闭迭代器

如果迭代器有`return`方法，它就是可关闭的。

这是一个可选功能。

数组迭代器是不可关闭的。

例如，如果我们写:

```
const arr = ['foo', 'bar', 'baz'];
const iterator = arr[Symbol.iterator]();
console.log('return' in iterator)
```

我们看到没有`return`方法。

另一方面，缺省情况下，生成器对象是可关闭的。

例如，如果我们有:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}const gen = genFn();
```

我们可以通过调用生成器`gen`上的`return`方法来完成对它的调用。

例如，我们可以写:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}const gen = genFn();
console.log(gen.next());
gen.return();
console.log(gen.next());
console.log(gen.next());
```

在我们给`return`打了电话之后，发电机就完成了。

所以最后 2 个`return`调用给我们:

```
{value: undefined, done: true}
```

如果迭代器是不可关闭的，我们可以在从 for-of 循环中退出后继续迭代。

因此，如果我们有一个带有不可关闭迭代器(如数组)的 iterable，我们可以继续遍历它:

```
const arr = ['foo', 'bar', 'baz'];for (const x of arr) {
  console.log(x);
  break;
}
for (const x of arr) {
  console.log(x);
}
```

在我们使用`break`跳出第一个循环后，第二个循环仍然从头到尾迭代。

# 防止迭代器被关闭

我们可以通过改变`return`方法来防止迭代器被关闭。

如果我们将`done`集合返回到`false`，那么它就无法关闭。

例如，我们可以写:

```
class NoReturn {
  constructor(iterator) {
    this.iterator = iterator;
  }
  [Symbol.iterator]() {
    return this;
  }
  next() {
    return this.iterator.next();
  }
  return (value) {
    return {
      done: false,
      value
    };
  }
}function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}
const gen = genFn();
const noReturn = new NoReturn(gen);for (const x of noReturn) {
  console.log(x);
  break;
}
for (const x of noReturn) {
  console.log(x);
}
```

由于我们将可关闭的生成器传递给了`NoReturn`构造函数，它将继续遍历这些项。

所以我们得到:

```
foo
bar
baz
```

记录在案。

它只是从停止的地方继续迭代，因为它还没有被关闭。

因此它可以从停止的地方继续迭代。

我们还可以通过将`prototype.return`方法设置为`undefined`来使生成器不可关闭。

然而，我们应该意识到这并不适用于所有的 transpilers。

例如，我们可以写:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}genFn.prototype.return = undefined;
```

我们将`genFn.prototype.return`属性设置为`undefined`。

然后我们可以通过写下以下内容来确认它是不可关闭的:

```
const gen = genFn();for (const x of gen) {
  console.log(x);
  break;
}
for (const x of gen) {
  console.log(x);
}
```

我们得到:

```
foo
bar
baz
```

从两个循环中。

使用`break`没有关闭发电机。

![](img/aede11e1194ac88a135d53feef6ec8c3.png)

[梁杰森](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以调整是否可以关闭迭代器。

如果关闭被禁用，这将让我们在中断和类似操作后遍历迭代器项。