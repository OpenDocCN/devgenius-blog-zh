# 面向对象的 JavaScript —生成器和映射

> 原文：<https://blog.devgenius.io/object-oriented-javascript-generators-and-maps-49485438672c?source=collection_archive---------3----------------------->

![](img/a15308a02e597f67e800f8e6838a8c01.png)

由[杰森·黑眼](https://unsplash.com/@jeisblack?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看发电机和地图。

# 发电机

生成器函数让我们创建生成器对象。

它们逐行运行，用`yield`返回结果，然后暂停，直到它被再次调用。

我们可以用它们来写无限循环，因为它们会在不需要的时候暂停。

例如，我们可以通过编写以下代码来创建一个生成器函数:

```
function* generatorFunc() {
  yield 1;
  yield 2;
  yield 3;
}
```

我们有`generatorFunc`发生器功能。

由`function*`关键字表示。

`yield`关键字让我们返回值并暂停值。

然后我们可以用它创建一个生成器，然后调用`next`:

```
const gen = generatorFunc();
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
```

然后我们得到:

```
{value: 1, done: false}
{value: 2, done: false}
{value: 3, done: false}
{value: undefined, done: true}
```

`next`返回一个具有`value`和`done`属性的对象。

`value`有返回值，而`done`表示是否有其他值要返回。

`done`为`true`表示没有其他值要返回。

如果我们使用不带参数的`yield`，那么我们可以向`next`传递一个值并获取该值。

例如，我们可以写:

```
function* generatorFunc() {
  console.log(yield);
  console.log(yield);
  console.log(yield);  
}const gen = generatorFunc();
console.log(gen.next());
console.log(gen.next('foo'));
console.log(gen.next('bar'));
console.log(gen.next('baz'));
```

我们在`generatorFunc`中控制台记录`yield`，然后当我们用一个参数调用`next`时，它将被记录。

第一个函数应该空着调用，因为它只是启动生成器函数，而不是运行主体。

它会返回:

```
{"done": false,"value": undefined}
```

生成器对象符合迭代器契约。

所以我们可以用它得到`Symbol.iterator`方法。

如果我们有:

```
console.log(gen[Symbol.iterator]() === gen);
```

我们记录`true`。

# 遍历生成器

生成器是迭代器。

任何支持 iterables 的东西都可以用来迭代生成器。

例如，我们可以使用 for-of 循环:

```
function* genFunc() {
  yield 1;
  yield 2;
}for (const i of genFunc()) {
  console.log(i)
}
```

然后我们得到:

```
1
2
```

我们也可以对生成器使用析构语法。

例如，我们可以写:

```
function* genFunc() {
  yield 1;
  yield 2;
}const [x, y] = genFunc();
```

那么`x`为 1`y`为 2。

# 收集

JavaScript 为我们提供了各种集合，比如`Map`、`WeakMap`、`Set`和`WeakSet`。

现在，我们不必创建黑客来创建这些数据结构。

# 地图

`Map`允许我们存储键值对。

它们让我们快速获得价值观。

例如，我们可以写:

```
const m = new Map();
m.set('first', 1);
```

那么我们可以通过写:

```
console.log(m.get('first'));
```

并通过键获取值。

我们可以用`delete`方法删除带有给定键的条目:

```
m.delete('first');
```

我们可以用`size`属性设置大小:

```
console.log(m.size);
```

我们还可以创建一个包含键值数组的映射:

```
const m = new Map([
  ['one', 1],
  ['two', 2],
  ['three', 3],
]);
```

![](img/aba6599330780a12d50a73ef8b8ad0e0.png)

照片由[卢卡·布拉沃](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用生成器来创建可迭代的。

此外，我们可以使用映射来存储键值对。