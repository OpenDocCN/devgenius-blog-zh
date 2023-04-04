# JavaScript 最佳实践—复制、返回、评估和内置原型

> 原文：<https://blog.devgenius.io/javascript-best-practices-duplicates-returns-eval-and-built-in-prototypes-aec55d5c4085?source=collection_archive---------28----------------------->

![](img/7a794fb8c063134c87455034ce88097b.png)

帕斯卡尔·莫尔霍弗在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 在`if-else-if`链中没有重复的条件

我们不应该在 if-else-if 链中有重复的条件。

只有第一个会运行。

所以与其写:

```
if (isSomething(x)) {
  foo();
} else if (isSomething(x)) {
  bar();
}
```

我们写道:

```
if (isSomething(x)) {
  foo();
} else if (isSomethingElse(x)) {
  bar();
}
```

# 对象文本中没有重复的键

我们不应该在对象文本中有重复的键。

例如，我们不应该有这样的对象:

```
const foo = {
  bar: "baz",
  bar: "bar"
};
```

相反，我们写道:

```
const foo = {
  bar: "baz"
};
```

# 没有重复的案例标签

我们的代码中不应该有重复的`case`标签。

例如，不写:

```
switch (a) {
  case 1:
    break;
  case 2:
    break;
  case 1:
    break;
  default:
    break;
}
```

我们写道:

```
switch (a) {
  case 1:
    break;
  case 2:
    break;
  default:
    break;
}
```

# 没有重复的导入

我们不应该对一个模块使用多个`import`语句。

例如，不写:

```
import { merge } from 'module';
import something from 'foo';
import { find } from 'module';
```

我们写道:

```
import { merge, find } from 'module';
import something from 'foo';
```

# 在此之前不返回

如果我们正在写一个`return`语句，那么我们可以删除`else`

例如，我们可以写:

```
function foo() {
  if (x) {
    return y;
  } 
  return z;  
}
```

而不是写:

```
function foo() {
  if (x) {
    return y;
  } else {
    return z;
  }
}
```

# 没有空块语句

空的块不是很有用，所以我们应该用一些东西填充它们或者移除它们。

例如，以下内容并不十分有用:

```
if (bar) {}while (bar) {}switch (bar) {}try {
  doSomething();
} catch (ex) {} finally {}for (;;){}
```

相反，我们用一些东西填充它们。

# 正则表达式中的空字符类

我们不应该在正则表达式中有空的字符类，因为它们不匹配任何东西。

例如，我们应该有这样的代码:

```
/^foo[]/.test("foobar");
"foobar".match(/^foo[]/);
```

相反，我们应该用一种模式来填充括号:

```
/^foo[a-z]/.test("foobar");
"foobar".match(/^foo[a-z]/);
```

# 没有空函数

我们的代码中不应该有空函数。

他们什么都不做。

例如，不写:

```
function foo() {}
```

我们写道:

```
function foo() {
  doSomething();
}
```

# 没有空的析构模式

空的析构模式什么都不做，并且很容易被误认为默认值是空对象。

例如:

```
const {a: {}} = foo;
```

很容易被误认为:

```
const {a = {}} = foo;
```

第一种是空的析构模式。

第二个是使用空对象作为默认值。

# 没有空值比较

由于数据类型强制，使用`==`或`!=`比较`null`可能会产生意想不到的结果。

所以要用`===`或者`!==`来做比较。

而不是写:

```
if (foo == null) {
  bar();
}while (qux != null) {
  bar();
}
```

我们写道:

```
if (foo === null) {
  bar();
}while (qux !== null) {
  bar();
}
```

# 没有调用 eval()

`eval`让我们从字符串中运行 JavaScript 代码。

但是我们不应该使用它，因为从字符串运行代码是不安全的。

性能优化也不能在字符串中的代码上进行。

而不是写:

```
eval("let a = 0");
```

我们写道:

```
let a = 0;
```

# 在`catch`子句中没有重新分配异常

我们不应该重新分配异常 ib `catch`子句。

如果我们这样做，那么我们会丢失异常中的数据。

所以与其写作；

```
try {
  // code
} catch (e) {
  e = 10;
}
```

我们写道:

```
try {
  // code
} catch (e) {
  const foo = 1;
}
```

# 不要扩展本机对象

我们不应该扩展内置对象，即使我们被允许这样做。

例如，我们不应该编写这样的代码:

```
Object.prototype.foo = 55;
```

向内置构造函数添加属性。

使用`defineProperty`也不好:

```
Object.defineProperty(Array.prototype, "bar", { value: 999 });
```

![](img/7083aa816adfdcbe5407eab7e60ab5a6.png)

劳伦·凯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们不应该有重复的条件、键或`case`标签。

同样，我们永远不应该使用`eval`。

我们不应该扩展本地对象。

空的析构模式也是无用的和欺骗性的。

我们要把`null`和`===`或者`!==`进行比较。