# JavaScript 最佳实践—更新的语法、格式和承诺拒绝

> 原文：<https://blog.devgenius.io/javascript-best-practices-newer-syntax-formatting-and-promise-rejections-f0c85c19c115?source=collection_archive---------32----------------------->

![](img/cd3f533b828c36308a3ddcdd17e5e472.png)

[布兰登·格林](https://unsplash.com/@brandgreen?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 将对象属性放在单独的行上

如果我们有很多对象属性，我们应该把它们放在单独的行上。

例如，不写:

```
const obj = {a: 1, b: [2, {c: 3, b: 4}]};
```

我们写道:

```
const obj = {
  a: 1,
  b: [2, {
    c: 3,
    b: 4
  }]
};
```

现在不会溢出页面了。

# 对象文字速记语法

我们应该使用简单的速记语法来节省打字和空间。

例如，不写:“

```
const bar = {
  x: x,
  y: y,
  z: z,
};
```

我们写道:

```
const bar = {
  x,
  y,
  z,
};
```

对于方法，不要写:

```
const bar = {
  x: function(){},
  y: function(){}
};
```

我们写道:

```
const bar = {
  x(){},
  y(){}
};
```

# 变量声明的换行符

如果变量声明很长，我们希望在声明中添加新的行。

如果我们有:

```
var a, b;
```

然后我们可以离开它。

如果更长，我们就把它们分成各自的行。

# 运算符的换行符样式

只要我们保持一致，我们可以把换行符放在我们想要的地方。

例如，我们可以写:

```
const fullHeight = paddingTop +
  innerHeight +
  paddingBottom;
```

或者:

```
const fullHeight = paddingTop
  + innerHeight
  + borderBottom;
```

# 块内无填充

我们可以跳过街区内的停车场。

例如，不写:

```
if (a) { b();}
```

我们写道:

```
if (a) {
  b();
}
```

# 语句之间的填充行

我们可以在语句之间添加填充线来对它们进行分组。

例如，我们可以写:

```
function foo() {
  let a = 1; return a;
}
```

# 为回调使用箭头函数

如果我们不需要在回调中引用它们自己的`this`，那么我们应该使用箭头函数进行回调。

例如，不写:

```
foo(function(a) { return a; });
```

我们写道:

```
foo(a => a);
```

# 使用`const`

我们可以用`const`声明常量。

一旦我们定义了一个新值，我们就不能给它们赋值。

例如，我们可以写:

```
const a = 0;
```

或者:

```
for (const a of [1, 2, 3]) {
  console.log(a);
}
```

# 更喜欢从数组和对象析构

我们可以对对象数组使用析构赋值语法。

他们让我们把他们的条目分解成变量。

例如，不写:

```
const foo = array[0];
```

我们写道:

```
const [foo] = array;
```

而不是写:

```
const foo = object.foo;
```

我们写道:

```
const { foo } = object;
```

# 更换`Math.pow`以利于`**`操作员

我们可以用`**`运算符代替`Math.pow`来做取幂运算。

它更短，我们不必调用方法。

而不是写:

```
const foo = Math.pow(2, 8);
```

我们写道:

```
const foo = 2 ** 8;
```

# 在正则表达式中使用命名捕获组

从 ES2018 开始，我们可以命名我们的捕获组。

这使得模式对每个人来说都更清楚。

例如，不写:

```
const regex = /(?\d{4})/;
```

我们写道:

```
const regex = /(?<year>\d{4})/;
```

现在我们知道我们在寻找年份。

# `Replace parseInt()`和`Number.parseInt()`带有二进制、八进制和十六进制文字

我们可以用数字文字代替`parseInt`和`Number.parseInt`调用。

例如，不写:

```
parseInt("1010101", 2)
```

并且:

```
Number.parseInt("1010101", 2)
```

我们可以写:

```
0b1010101
```

它要短得多。

# 使用对象遍布`Object.assign`

我们可以对对象使用 spread 运算符，

因此，我们应该使用它们，因为它更短。

例如，不写:

```
Object.assign({}, {foo: 'bar'})
```

我们写道:

```
const obj = {foo: 'bar'};
const copy = {...obj};
```

# 使用错误对象作为承诺拒绝原因

对于承诺拒绝原因，我们应该使用`Error`实例。

它们包括堆栈跟踪，告诉我们调试的有用信息。

我们需要它来找出错误的来源。

例如，不写:

```
Promise.reject('error');
```

我们写道:

```
Promise.reject(new Error("error"));
```

![](img/11c882207e18aa6d9d612f1d153840c4.png)

照片由 [Darius Cotoi](https://unsplash.com/@dariuscotoi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

`Error`实例告诉我们比其他对象更多的错误信息。

我们也可以用换行符和空格来格式化我们的代码。

有了新语法，我们可以编写更少的代码。

它们包括对象扩展、取幂和析构赋值语法。

此外，我们可以对正则表达式使用命名捕获组。