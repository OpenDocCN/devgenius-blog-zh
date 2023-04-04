# 现代 JavaScript 的精华——迭代器和生成器

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-iterators-and-generators-d21536100140?source=collection_archive---------2----------------------->

![](img/0d464f11e2530f91bccb67a8260c10a9.png)

由[莎拉·多维勒](https://unsplash.com/@sarahdorweiler?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 可迭代对象和生成器函数。

# 发电机的清理

如果我们使用`break`来打破一个循环，那么我们可以用 try-finally 构造函数来清理。

例如，如果我们有:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
  console.log('clean up');}const gen = genFn();for (const x of gen) {
  console.log(x);
}
```

然后，当生成器返回所有项目时，控制台日志将运行。

然而，如果我们在循环中使用`break`:

```
for (const x of gen) {
  console.log(x);
  break;
}
```

则控制台日志永远不会运行。

当我们在循环中使用`break`时，我们可以通过用 try-finally 包装`yield`代码来运行清理步骤。

例如，我们可以写:

```
function* genFn() {
  try {
    yield 'foo';
    yield 'bar';
    yield 'baz';
  } finally {
    console.log('clean up');
  }
}const gen = genFn();for (const x of gen) {
  console.log(x);
  break;
}
```

我们添加了`finally`块，它将在`break`语句运行时运行。

这对于在我们的生成器上运行清理代码非常有用。

如果我们实现了自己的迭代器，我们可以将函数放在迭代器的`return`方法中:

```
const obj = {
  [Symbol.iterator]() {
    function hasNextValue() {
      //...
    } function getNextValue() {
      //...
    } function cleanUp() {
      //...
    } return {
      next() {
        if (hasNextValue()) {
          const value = getNextValue();
          return {
            done: false,
            value: value
          };
        } else {
          //...
          return {
            done: true,
            value: undefined
          };
        }
      },
      return () {
        cleanUp();
      }
    };
  }
}
```

我们在用`Symbol.iterator`方法返回的对象中包含了`return`方法。

# 关闭迭代器

我们可以通过创建自己的生成器函数来关闭任何迭代器。

例如，我们可以写:

```
function* take(n, iterable) {
  for (const x of iterable) {
    if (n <= 0) {
      break;
    }
    n--;
    yield x;
  }
}
```

当结束条件满足时，我们在循环中调用`break`来结束它。

# 发电机

生成器是我们可以暂停和恢复的代码片段。

它由生成器函数的关键字`function*`表示。

`yield`是发电机用来自行暂停的操作。

发电机可以通过`yield`接收输入和发送输出。

我们可以通过编写以下代码来创建一个生成器函数:

```
function* genFn() {
  yield 'foo';
  yield 'bar';
  yield 'baz';
}
```

这将返回一个生成器对象，我们可以调用它来返回 yield 的值:

```
const genObj = genFn();
console.log(genObj.next());
console.log(genObj.next());
console.log(genObj.next());
```

我们调用`genFn`返回一个生成器。

然后我们调用`next`依次得到每个值。

所以我们得到:

```
{value: "foo", done: false}
{value: "bar", done: false}
{value: "baz", done: false}
```

我们用属性`value`和`done`返回一个对象。

`value`具有来自`yield`的值。

`done`告诉我们是否所有的值都已经从生成器中生成。

![](img/3a4e7eeb9b11bc9562aaab902529d449.png)

卢卡·布拉沃在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以通过添加一个 try-finally 块来清理我们的生成器。

此外，生成器函数创建生成器，让我们按顺序返回值。