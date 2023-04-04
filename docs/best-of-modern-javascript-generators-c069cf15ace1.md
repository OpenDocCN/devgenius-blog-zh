# 现代 JavaScript 的精华——生成器

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-generators-c069cf15ace1?source=collection_archive---------7----------------------->

![](img/ffba1ee2c1aabc8af71ae71550fa9b45.png)

Jason Blackeye 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 生成器。

# 发电机的种类

有各种各样的发电机。

我们可以将生成器定义为生成器函数声明。

这采取以下形式:

```
function* genFn() {
  //...
}
const genObj = genFn();
```

生成器函数表达式是我们将生成器函数赋给变量的地方:

```
const genFn = function*() {
  //...
}
const genObj = genFn();
```

对象文字中的生成器方法让我们创建生成器方法:

```
const obj = {
  * gen() {
    //...
  }
};
const genObj = obj.gen();
```

生成器方法也可以在类中。

例如，我们可以写:

```
class Foo {
  * gen() {
    //..,
  }
}
const foo = new Foo();
const genObj = foo.gen();
```

我们创建了一个类，并添加了生成器方法`gen`。

我们可以使用生成器函数来创建我们自己的 iterable 对象。

例如，我们可以写:

```
function* keys(obj) {
  const propKeys = Reflect.ownKeys(obj); for (const propKey of propKeys) {
    yield propKeys;
  }
}const obj = {
  a: 1,
  b: 2
};for (const key of keys(obj)) {
  console.log(key);
}
```

我们创建了`keys`生成器函数，它从一个对象获取密钥并返回密钥。

然后我们用 for-of 循环遍历这些键。

`async`和`await`下面也用发电机，。

`await`的工作方式类似于`yield`，它暂停代码，直到检索到结果。

语法还使用了表面下的生成器。

例如，我们可以这样使用它:

```
async function fetchJson(url) {
  try {
    const request = await fetch(url);
    const res = await request.json();
    return res;
  } catch (error) {
    console.log(error);
  }
}
```

异步函数只返回承诺。

`return`语句返回解析为返回值的承诺。

# 发电机

发生器是可以暂停和恢复的功能。

生成器是唯一可以返回生成器的函数。

所以如果我们有一个生成器函数，我们可以写:

```
function* keys(obj) {
  const propKeys = Reflect.ownKeys(obj); for (const propKey of propKeys) {
    yield propKeys;
  }
}const obj = {
  a: 1,
  b: 2
};const gen = keys(obj)
```

我们调用`keys` genereator 函数来返回一个生成器。

发电机是`gen`并且 ot 不运行，直到我们调用`next`。

例如，如果我们有:

```
const gen = keys(obj);console.log(gen.next());
console.log(gen.next());
```

然后发电机功能将运行。

# 发电机的使用

生成器可以用于迭代器。

它们用`yield`产生数据，可以用`next`方法访问。

生成器可以产生带有循环和递归的值序列。

这意味着生成器可以与 for-of 和 spread 运算符一起使用。

他们也可以是数据消费者。

`yield`可以从`next`方法或任何其他来源获取值。

它们也可以同时存在，因为它们可以暂停。

![](img/2204563755561614fec6cae474b4144d.png)

由 [Claudel Rheault](https://unsplash.com/@claudelrheault?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

生成器可以是数据生产者，也可以是消费者。

`async`和`await`也在表面下使用生成器语法。