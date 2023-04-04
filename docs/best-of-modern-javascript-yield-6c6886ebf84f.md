# 现代 JavaScript 的精华— Yield

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-yield-6c6886ebf84f?source=collection_archive---------2----------------------->

![](img/3a148bda5289db0a9b5914ee15e56fcd.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 生成器。

# yield 关键字

`yield`关键字只能在生成器函数中使用。

例如，我们可以写:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  return 'baz';
}
```

但是我们不能写:

```
function* genFn() {
  ['foo', 'bar'].forEach(x => yield x);
}
```

我们会得到一个语法错误。

# 用`yield*`递归

`yield*`关键字让我们在一个生成器函数中调用另一个生成器函数。

例如，我们可以写:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}function* bar() {
  yield 'x';
  yield* genFn();
  yield 'y';
}const gen = bar()
```

在`bar`生成器函数中调用`genFn`生成器函数。

所以如果我们通过书写来使用它:

```
const arr = [...gen];
```

那么`arr`就是`[“x”, “foo”, “bar”, “baz”, “y”]`。

调用`genFn`返回一个对象，但是它不运行`genFn`。

`yield*`关键字运行生成器函数，当它的项目被产出的时候。

# `yield*`考虑迭代结束值

`yield*`忽略`return`值。

例如，如果我们有:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  return 'baz';
}function* bar() {
  yield 'x';
  yield* genFn();
  yield 'y';
}const gen = bar()
const arr = [...gen];
```

那么`arr`就是`[“x”, “foo”, “bar”, “y”]`，`yield*`被跳过`'baz'`，因为它被返回而不是产出。

我们可以使用`yield*`递归调用生成器函数。

所以我们可以很容易地用它从树结构中返回项目。

例如，我们可以创建一个具有不同分支节点的`Tree`类:

```
class Tree {
  constructor(value, left = null, center = null, right = null) {
    this.value = value;
    this.left = left;
    this.center = center;
    this.right = right;
  } *[Symbol.iterator]() {
    yield this.value;
    if (this.left) {
      yield* this.left;
    } if (this.center) {
      yield* this.center;
    } if (this.right) {
      yield* this.right;
    }
  }
}
```

`left`、`center`和`right`都是从`Tree`类创建的生成器函数。

所以我们可以写:

```
const tree = new Tree('a',
  new Tree('b',
    new Tree('c'),
    new Tree('d'),
    new Tree('e')),
  new Tree('f'));for (const x of tree) {
  console.log(x);
}
```

我们得到了:

```
a
b
c
d
e
f
```

`value`是树节点本身的值。

我们用`Tree`构造函数填充节点。

# 发电机作为观察员

生成器也可以是数据观察者。

我们用它来通过`next`方法发送值。

`next`方法保持从 then 生成器返回值，直到值用完。

例如，如果我们有:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  return 'baz';
}const gen = genFn();
console.log(gen.next());
```

我们得到了:

```
{value: "foo", done: false}
```

我们还可以使用一个参数调用`next`来返回生成器生成的内容的索引。

例如，我们可以写:

```
function* genFn() {
  console.log(yield);
  console.log(yield);
  console.log(yield);  
}const gen = genFn();
console.log(gen.next('a'));
console.log(gen.next('b'));
console.log(gen.next('c'));
```

我们使用`yield`关键字从传递给`next`的参数中获取值。

从`next`方法返回的值是:

```
{value: undefined, done: false}
```

`value`是`undefined`而`done`是`false`。

`yield`没有操作数的关键字从`next`方法中获取值。

![](img/29c371435b01f98e7187f38e824ea750.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

`yield`和`yield*`关键字有很多用途。

`yield`可以返回值，也可以从`next`方法中获取这些值。