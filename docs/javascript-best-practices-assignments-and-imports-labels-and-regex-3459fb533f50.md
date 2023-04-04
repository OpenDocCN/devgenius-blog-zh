# JavaScript 最佳实践—赋值和导入、标签和正则表达式

> 原文：<https://blog.devgenius.io/javascript-best-practices-assignments-and-imports-labels-and-regex-3459fb533f50?source=collection_archive---------24----------------------->

![](img/16161b96b30acf40b5958ea52ba67f52.png)

照片由 [Lydia Tan](https://unsplash.com/@purplelydia?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 不分配给导入的绑定

我们不应该直接给导入的绑定赋值。

因为它们是只读的，如果我们这样做的话会导致运行时错误。

例如，我们不应该写:

```
import mod, { named } from "./foo"
mod = 1; 
named = 2;
```

相反，我们写道:

```
import mod, { named } from "./foo"
const bar = 1; 
const baz = 2;
```

# 代码后没有内联注释

写一行代码后的行内注释很痛苦。

在一行之后也可能有足够的空间来写它们。

例如，如果我们有:

```
function getRandomNumber() {
  return 4; // roll a dice
            // It's a real dice.
}
```

我们必须在注释的第二行添加许多空格，使它们对齐。

如果我们不把它们串联起来，我们可以让生活变得更轻松:

```
function getRandomNumber() {
  return 4; 
  // roll a dice
  // It's a real dice.
}
```

# 嵌套块中没有变量或 F `unction`声明

我们不应该在嵌套块中使用变量或函数声明。

这是非法的语法，但是 JavaScript 解释器仍然允许它。

例如，不写:

```
if (foo) {
  function doSomething() {}
}
```

我们写道:

```
function doSomething() {}
```

# 在`RegExp`构造函数中没有无效的正则表达式字符串

如果我们写一个正则表达式，那么我们应该确保里面有一个有效的模式。

例如，我们不应该写:

```
RegExp('[')RegExp('.', 'z')
```

相反，我们写道:

```
RegExp('foo')

RegExp('\d', 'g')
```

# `No this`类或类对象之外的关键字

我们不应该在类或类对象之外使用`this`关键字。

它们只能在那些实体中使用。

所以与其写:

```
this.a = 0;
baz(() => this);
```

我们写道:

```
function Foo() {
  this.a = 0;
  baz(() => this);
}class Bar {
  constructor() {
    this.a = 0;
    baz(() => this);
  }
}
```

`Foo`是一个构造函数,`Bar`是一个类，所以我们可以在那里使用`this`。

# 没有不规则的空白

我们应该确保我们的空白字符是正常的空白字符。

大多数文本编辑器都可以用规则的替换不规则的。

# 没有迭代器

`__iterator__`属性不是标准属性。

所以我们不应该用它来创建迭代器。

相反，我们使用生成器来创建迭代器:

```
function* generator() { 
  yield 1;
  yield 2;
  yield 3;
}
```

我们有`generator`发生器功能。

我们可以通过编写以下代码来创建一个生成器:

```
const gen = generator();
```

# 没有变量名标签

我们不应该有相同变量名的标签。

这会给大多数人造成困惑。

例如，我们不应该有这样的代码:

```
let x = 'abc';function bar() {
  x: for (;;) {
    break x;
  }
}
```

我们用`x`作为变量和循环的标签。

相反，我们写道:

```
let x = 'abc';function bar() {
  z: for (;;) {
    break z;
  }
}
```

或者我们就是不用标签。

# 没有带标签的语句

带标签的语句让我们为一个循环设置一个名字，这样我们就可以对它使用`break`或`continue`。

然而，这是一个很少使用的功能，大多数人都不知道它。

因此，我们应该用别的东西。

例如，不写:

```
outer:
  while (true) {
    while (true) {
      break outer;
    }
  }
```

我们写道:

```
while (true) {
  break;
}
```

# 没有不必要的嵌套块

我们不应该有不必要的嵌套块。

例如，下面的内容是没有用的:

```
{
  var foo = baz();
}
```

这只对`let`和`const`变量有用，它们是块范围的。

所以我们写道:

```
{
  let foo = baz();
}{
  const foo = baz();
}
```

把`foo`只关在他们自己的街区。

# `No if`语句作为`else`块中的唯一语句

我们不应该有这样的代码:

```
if (foo) {
  // ...
} else {
  if (bar) {
    // ...
  }
}
```

因为这和:

```
if (foo) {
  // ...
} else if (bar) {
    // ...
}
```

它要短得多，但功能相同。

![](img/a2e42534ed3e43ed72b87a0614a56257.png)

照片由 [Jp Valery](https://unsplash.com/@jpvalery?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们不能将值重新分配给导入的绑定。

此外，我们应该删除标签、冗余块和冗余的`if`语句。

我们还应该确保正则表达式中包含有效的模式。