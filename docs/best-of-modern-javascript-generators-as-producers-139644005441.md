# 现代 JavaScript 的精华——作为生产者的生成器

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-generators-as-producers-139644005441?source=collection_archive---------5----------------------->

![](img/f2de4bd501a45254b69457b8cafac0a6.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Kervin Edward Lara](https://unsplash.com/@kealearl2013?utm_source=medium&utm_medium=referral) 拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 生成器。

# 作为迭代器的生成器

我们可以使用生成器函数作为迭代器。

例如，我们可以写:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}const gen = genFn();console.log(gen.next());
console.log(gen.next());
```

创建一个生成器函数和一个生成器。

`genFn`是用`function*`关键字表示的发生器功能。

该函数通过`genFn`调用返回一个生成器。

然后我们调用`next`来获取列表中的下一项。

`yield`关键字让我们通过`next`方法调用返回值。

因此，我们得到:

```
{value: "foo", done: false}
{value: "bar", done: false}
```

通过`value`属性获取产出的项目。

我们还可以使用 for-of 循环访问生成的值。

例如，我们可以写:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}const gen = genFn();for (const x of gen) {
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

for-of 循环可以访问生成器中的值。

spread 运算符允许我们通过提取生成的值并将其放入数组来将生成器转换为数组:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}const gen = genFn();
const arr = [...gen];
```

和析构也可以与生成器一起工作，方法是将生成的值指定为左侧变量的值:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}const [x, y] = genFn();
```

然后我们得到`x`是`'foo'`，`y`是`'bar'`。

# 从发电机返回

我们可以使用生成器中的`return`语句。

例如，我们可以写:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  return 'baz';
}
```

然后，我们可以通过编写以下代码来调用生成器:

```
const gen = genFn();
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
```

从控制台日志中，我们可以看到这些值是:

```
{value: "foo", done: false}
{value: "bar", done: false}
{value: "baz", done: true}
```

`return`语句将`done`设置为`true`。

然而，大多数其他使用 iterable 对象的 const5ructurs 忽略返回值。

例如，如果我们有 for-of 循环:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  return 'baz';
}const gen = genFn();
for (const x of gen) {
  console.log(x);
}
```

然后我们得到:

```
foo
bar
```

记录在案。

如果我们有扩展操作符:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  return 'baz';
}const gen = genFn();
const arr = [...gen];
```

那么`arr`就是`[“foo”, “bar”]`。

# 从生成器引发异常

我们可以从生成器中抛出异常。

例如，我们可以写:

```
function* genFn() {
  throw new Error('error');
}const gen = genFn();
console.log(gen.next())
```

我们有一个抛出错误的生成器函数。

当我们调用它，然后在返回的生成器上调用`next`时，我们会看到控制台中记录的错误。

![](img/f0b6f0478e8654e3e3163c0a79ff6237.png)

马丁·比约克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以在生成器函数中返回项目并抛出错误。

它们也能产生我们可以利用的价值。