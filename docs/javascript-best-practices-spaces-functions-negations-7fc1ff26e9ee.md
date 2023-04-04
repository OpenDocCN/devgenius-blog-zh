# JavaScript 最佳实践—空格、函数、否定

> 原文：<https://blog.devgenius.io/javascript-best-practices-spaces-functions-negations-7fc1ff26e9ee?source=collection_archive---------25----------------------->

![](img/483eb26fa19f398c6aed277424c2cb26.png)

照片由[乔纳森派](https://unsplash.com/@r3dmax?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 循环中没有函数

循环中的函数会导致错误，因为它们是如何在循环中创建的。

使用`var`，循环索引总是最后一个，因为它在完成之前不会被传递给变量函数。

而不是写:

```
for (var i = 0; i < 10; i++) {
  funcs[i] = function() {
    return i;
  };
}
```

我们写道:

```
for (let i = 0; i < 10; i++) {
  funcs[i] = function() {
    return i;
  };
}
```

来获取返回的函数中`i`的期望值，也就是每次迭代时循环的索引值。

使用`var`，该值将始终为 10。

# 没有神奇的数字

幻数很难理解，我们必须在多个地方改变它们，所以我们应该把它们赋给命名的常量。

而不是写:

```
const now = Date.now();
const anHourLater = now + (60 * 60 * 1000);
```

我们写道:

```
const SECONDS_IN_HOUR = 60 * 60;
```

# 在字符类语法中没有由多个码位组成的字符

我们不应该有由多个代码点组成的字符。

这是因为它们将与任一字符匹配。

所以 w 应该写:

```
/^[a]$/u.test("a")
```

而不是:

```
/^[Á]$/u.test("Á")
```

# 没有不同运营商的混合

混合不同的操作符是令人困惑的，所以我们不应该这样做。

例如，不写:

```
const foo = a && b || c || d; 
```

我们用一些括号来分隔表达式:

```
const foo = (a && b) || c || d;
```

# `require`与常规变量声明混合的调用

为了清楚起见，我们没有用逗号分隔常规变量来编写`require`调用，而是将它们都写在各自的行中。

例如，不写:

```
const eventEmitter = require('events').EventEmitter,
  foo = 10,
  bar = 'baz';
```

我们写道:

```
const eventEmitter = require('events').EventEmitter;
const foo = 10;
let bar = 'baz';
```

# 没有混合空间和缩进标签

混合空格和制表符进行缩进会导致文本编辑器解析和格式化的问题，所以我们应该将制表符转换成空格。

# 不使用链式赋值表达式

链式赋值表达式将创建全局变量。

不在变量声明关键字旁边的将是全局的。

它们也很难阅读。

例如，如果我们有:

```
const foo = bar = 0;
```

那么`bar`是全局的`foo`是常数。

相反，为了清楚起见，我们将它们分开:

```
const foo = -0;
const bar = 0;
```

# 没有多个空格

除了缩进，一个空格就足够了。

所以我们应该为表达式保留一个单独的空间。

而不是写:

```
if(foo  === "baz") {}
```

我们写道:

```
if(foo === "baz") {}
```

# 没有多行字符串

我们不应该用反斜杠写多行字符串。

不规范，不好。

所以与其写:

```
let x = "line 1 \
  line 2";
```

我们写道:

```
let x = `line 1 
  line 2`;
```

对于多行字符串，我们使用模板字符串而不是常规字符串。

# 没有多个空行

多个空行对于分隔代码没有用。

我们只需要一个。

所以与其写:

```
let foo = 5;var bar = 3;
```

我们写道:

```
let foo = 5;var bar = 3;
```

# 不重新分配本机对象

我们不应该将本机对象重新分配给任何其他对象。

我们不希望因此出现任何意外的结果。

所以与其写:

```
Object = null
undefined = 1
```

我们写道:

```
let foo = null;
let bar = 1;
```

# 没有否定的条件

被否定的条件很难读懂，所以我们应该尽可能地避免它们。

例如，不写:

```
if (!a) {
  doSomething();
} else {
  doMore();
}
```

我们写道:

```
if (a) {
  doSomething();
} else {
  doMore();
}
```

![](img/1a4f5ff3a4927521fa3dd77f8f2f0023.png)

Benoit Gauzere 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们的代码中不需要多个空格。

否定的条件应该用肯定的条件代替。

幻数应该用命名的常数代替。

请注意，我们没有包含具有多个代码点的字符的正则表达式。

空格和制表符不能混用。