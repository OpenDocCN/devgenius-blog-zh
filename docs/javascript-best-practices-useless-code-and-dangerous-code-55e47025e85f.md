# JavaScript 最佳实践——无用代码和危险代码

> 原文：<https://blog.devgenius.io/javascript-best-practices-useless-code-and-dangerous-code-55e47025e85f?source=collection_archive---------11----------------------->

![](img/173b615e2b68fa46b8ab676ba3f7d130.png)

塔玛拉·比利斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 没有不必要的布尔转换

我们不应该有重复的布尔类型转换。

例如，我们不应该有这样的代码:

```
const foo = !!!bar;const foo = !!bar ? baz : bat;const foo = Boolean(!!bar);const foo = new Boolean(!!bar);if (!!foo) {
  // ...
}if (Boolean(foo)) {
  // ...
}while (!!foo) {
  // ...
}
```

相反，我们写道:

```
const foo = !bar;const foo = bar ? baz : bat;const foo = Boolean(bar);const foo = new Boolean(bar);if (foo) {
  // ...
}if (foo) {
  // ...
}while (foo) {
  // ...
}
```

我们删除了所有多余的`Boolean`或`!`调用和操作符，因为我们不需要它们。

# 没有不必要的标签

我们可以给循环贴标签。

但大多数时候，我们并不需要它们。

这也是一个很少使用的功能。

所以很多人会很迷茫。

例如，不写:

```
A: while (a) {
  break A;
}
```

我们写道:

```
while (a) {
  break;
}
```

# 没有不必要的括号

我们应该在不必要的地方加上括号。

例如，不写:

```
a = (b * c);(a * b) + c;for (a in (b));for (a of (b));typeof (a);
```

我们写道:

```
a = b * c;a * b + c;for (a in b);for (a of b);typeof a;
```

这为我们节省了打字和空间。

# 没有不必要的分号

我们不应该添加不必要的分号。

例如，不写:

```
const x = 5;;
```

我们写道:

```
const x = 5;
```

我们也不需要在函数声明后使用分号:

```
function foo() {
  //...
};
```

我们可以写:

```
function foo() {
  //...
}
```

# 没有案例陈述失败

记得在`case`语句中添加`break`。

例如，我们不应该写:

```
switch (foo) {
  case 1:
    doSomething(); case 2:
    doSomethingElse();
}
```

相反，我们写道:

```
switch (foo) {
  case 1:
    doSomething();
    break;
  case 2:
    doSomethingElse();
    break;
}
```

# 无浮动小数

浮动小数对一些人来说是令人困惑的。

我们可以写一个小数来代替`0.`

举个例子，

```
const num = .5;
```

将`num`设置为 0.5。

我们应该在我们的数字中加上前导零:

```
const num = 0.5;
```

# 没有重新分配 F `unction`声明

我们不应该把函数声明重新分配给某个东西。

如果我们这样做，会引起混乱。

例如，不写:

```
function foo() {}
foo = bar;
```

我们写道:

```
function foo() {}
const bar = 1;
```

# 没有给本机对象或只读全局变量赋值

我们不应该给本地对象或只读全局变量赋值。

例如，我们不应该有这样的代码:

```
window = {};
Object = undefined;
undefined = 1;
```

相反，我们将它们分配给我们自己的变量:

```
const a = {};
const b = undefined;
const c = 1;
```

# 使用较短符号的类型转换

可以使用一些较短的符号。

例如，我们可以写:

```
const num = +'123';
```

将`num`转换为数字。

我们也可以使用`!!`来转换成布尔型:

```
const b = !!foo;
```

其他如连接到一个字符串或使用按位运算符更容易混淆，可能应该避免。

# 全局范围内没有声明

我们不应该声明全局变量。

很容易发生范围冲突，并且很难追踪它们来自哪里。

例如，我们不应该有这样的代码:

```
var foo = 1;
bar = 2;
function baz() {}
```

相反，我们写道:

```
let foo = 1;
const bar = 2;
let baz = () => {}
```

我们将它们保持在块范围内，以防止意外访问。

# 没有隐含的 eval()

`setTimeout`和`setInterval`可以用字符串代替回调来运行代码。

这就像`eval`。该代码是危险的，并阻止优化，因为代码是在一个字符串中。

所以与其写:

```
setTimeout("alert('hello');", 100);setInterval("alert('hello');", 100);
```

我们写道:

```
setTimeout(function() {
  alert("hello");
}, 100);setInterval(function() {
  alert("hello");
}, 100);
```

![](img/bebf320af631d0293916bc42ab75060a.png)

照片由[梅丽莎·拉贝拉特](https://unsplash.com/@mlabellarte?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们应该总是传递一个回调函数而不是一个字符串给`setTimeout`和`setInterval`。

此外，我们不应该不必要地对布尔值进行大小写。

无用的标签和括号也要去掉。

我们也不应该创建全局变量或者给内置对象赋值。