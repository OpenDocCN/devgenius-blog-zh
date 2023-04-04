# 最佳现代 JavaScript——for-of 循环

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-for-of-loop-b351c75bf131?source=collection_archive---------6----------------------->

![](img/cb27c6288a4c83e5b4f4d47411b16c45.png)

Andrew Ling 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 可迭代对象。

# 可迭代数据源

我们可以使用 for-of 循环来遍历各种类型的可迭代对象。

例如，我们可以通过编写以下代码来循环遍历一个数组:

```
const arr = ['foo', 'bar', 'baz'];
for (const x of arr) {
  console.log(x);
}
```

然后我们得到:

```
foo
bar
baz
```

记录在案。

我们也可以用它来搭配琴弦。

例如，我们可以写:

```
const arr = ['foo', 'bar', 'baz'];
for (const x of 'foo') {
  console.log(x);
}
```

然后我们分别记录`'foo'`的字母。

地图也是可复制的对象，所以我们可以在 for-of 循环中使用它。

例如，我们可以写:

```
const map = new Map().set('foo', 1).set('bar', 2);
for (const [key, value] of map) {
  console.log(key, value);
}
```

我们用`Map`构造函数创建了一个带有一些键和值的映射。

然后在 for-of 循环中，我们遍历每个条目，并从 map 条目中析构键和值。

所以我们得到:

```
foo 1
bar 2
```

从控制台日志中。

集合也是可以迭代的可迭代对象。

例如，我们可以写:

```
const set = new Set().add('foo').add('bar');
for (const x of set) {
  console.log(x);
}
```

我们创建了一个集合，并向其中添加了一些项目。

然后我们对它使用 for-of 循环。

所以我们得到:

```
foo
bar
```

从控制台日志中。

`arguments`是一个在传统函数中可用的对象，用于从函数调用中获取参数。

我们可以在 for-of 循环中使用它，因为它是一个可迭代的对象:

```
function logArgs() {
  for (const x of arguments) {
    console.log(x);
  }
}
logArgs('foo', 'bar');
```

我们将`'foo'`和`'bar'`传递给`logArgs`函数，并从 for-of 循环中获取值。

DOM 节点列表也可以通过 for-of 循环进行迭代。

例如，我们可以写:

```
for (const div of document.querySelectorAll('div')) {
  console.log(div);
}
```

遍历页面上的所有 div。

# 可迭代计算数据

如果一个函数返回一个 iterable 对象，我们可以用 for-of 循环遍历它。

例如，我们可以写:

```
const arr = ['a', 'b', 'c'];
for (const [index, element] of arr.entries()) {
  console.log(index, element);
}
```

然后我们得到:

```
0 "a"
1 "b"
2 "c"
```

我们用`arr.entries`方法获取索引和元素，并获取循环体内每个条目的值。

# 普通对象是不可迭代的

普通对象是不可迭代的，所以我们不能在 for-of 循环中使用它们。

例如，类似于:

```
for (const x of {}) {
  console.log(x);
}
```

会给出“未捕获类型错误:{}不可迭代”的错误。

对象是不可迭代的，因为我们要么遍历对象的属性来检查它的结构。

或者我们循环数据。

这应该分开保存，以避免将对象与其他类型的数据结构混淆。

如果我们想让一个对象可迭代，我们可以创建`Symbol.iterator`生成器方法来实现。

![](img/3ca5e5e54179feed4106cd72c38e2f66.png)

pawebukowski 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

for-of 循环可以遍历多种可迭代对象。