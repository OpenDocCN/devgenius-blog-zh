# JavaScript 最佳实践——不属于生产代码的东西

> 原文：<https://blog.devgenius.io/javascript-best-practices-things-that-dont-belong-in-production-code-751136cfb035?source=collection_archive---------26----------------------->

![](img/e8e1a404b73200e19968fc0a54dcef5e.png)

由 [Tara Raye](https://unsplash.com/@tararaye?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 条件语句中没有赋值运算符

条件语句中的赋值操作符可能是一个错误。

如果这就是我们所拥有的一切，那么我们应该检查一下这是否真的是我们想要的。

例如，不写:

```
if (user.title = "manager") {
  // ...
}
```

我们写道:

```
if (user.title === "manager") {
  // ...
}
```

# 没有会与比较混淆的箭头函数

很容易混淆箭头函数的箭头和比较运算符。

例如，如果我们有:

```
const foo = a => 1 ? 2 : 3;
```

那么我们就很容易被迷惑。

为了清楚起见，我们写道:

```
const foo = (a) => (1 ? 2 : 3);
```

# `Remove console Before Going to Production`

我们应该在代码投入生产之前移除`console`。

这有利于调试，但不适合最终用户查看输出。

如果我们有这样的东西:

```
console.log("foo bar");
```

我们应该移除它们。

# 不要修改用`const`声明的变量

如果变量是用`const`声明的，就不应该被重新赋值。

如果我们试图这样做，我们会得到一个错误。

所以与其写:

```
const a = 0;
a = 2;
```

或者:

```
const a = 0;
a += 1;
```

我们写道:

```
const a = 0;
const b = a + 1;
```

# 条件中没有常量表达式

我们不应该在条件中有常量表达式。

他们要么一直跑，要么从不跑。

这使得它们毫无用处。

例如，不写:

```
if (false) {
  doSomething();
}
```

从来没有运行，我们写道:

```
if (foo && bar) {
  doSomething();
}
```

# 在构造函数中返回值

如果我们在构造函数中返回值，那么对象就会被返回。

否则，我们返回构造函数的实例。

后一种情况大概是大多数人想要的。

所以大部分时候不应该返回自己的对象。

而不是写:

```
class A {
  constructor(a) {
    this.a = a;
    return a;
  }
}
```

我们写道:

```
class A {
  constructor(a) {
    this.a = a;
  }
}
```

返回一个`A`的实例，而不是`a`的值。

# `continue`报表

`contunue`语句让我们跳到循环的下一次迭代。

例如，我们可以写:

```
for (i = 0; i < 10; i++) {
  if (i >= 5) {
    continue;
  }
  //...
}
```

如果`i`大于或等于 5，跳到下一次迭代。

这可能是有用的。

# 正则表达式中的控制字符

一般来说，regex 或 JavaScript 中很少使用控制字符，所以使用它们可能是个错误。

而不是写:

```
const pattern1 = /\x1f/;
const pattern2 = new RegExp("\x1f");
```

我们写道:

```
const pattern1 = /\d/;
const pattern2 = new RegExp("\d");
```

# 不要使用`debugger in Production Code`

在我们的代码投入生产之前，我们应该删除所有的`debugger`语句，这样应用程序才能正常运行。

如果我们不这样做，那么当我们点击语句时它会暂停。

而不是写:

```
function toBool(x) {
  debugger;
  return Boolean(x);
}
```

我们写道:

```
function toBool(x) {  
  return Boolean(x);
}
```

# 不删除变量

`delete`操作符仅用于删除变量属性。

它不能和变量一起使用。

所以我们不应该有这样的代码:

```
let x;
delete x;
```

# 没有看起来像除法的正则表达式

我们不应该有一个看起来像组织的正则表达式。

例如，代码如下:

```
/=foo/
```

很可能是个错误。

相反，我们写道:

```
/foo/
```

# F `unction`定义中没有重复的参数

重复参数在 JavaScript 中是非法的语法，所以我们不应该这样写。

例如，不写:

```
function foo(a, b, a) {
  console.log(a);
}
```

我们写道:

```
function foo(a, b) {
  console.log(a);
}
```

# 类成员中没有重复的名称

我们不应该在类成员中有重复的名字

最后出现的实例将覆盖前面的实例。

所以与其写:

```
class Foo {
  bar() {
    console.log("bar");
  }
  bar() {
    console.log("baz");
  }
}
```

我们写道:

```
class Foo {
  bar() {
    console.log("baz");
  }
}
```

![](img/42a63ae25e10f4da50e855ec6bc8c01e.png)

胡安·恩卡拉达在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们不应该有重复的参数和类成员。

我们可能不应该在`if`语句中使用赋值操作符。

`console`和`debugger`不属于生产代码。