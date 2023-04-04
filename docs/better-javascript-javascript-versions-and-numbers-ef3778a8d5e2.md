# 更好的 JavaScript——JavaScript 版本和数字

> 原文：<https://blog.devgenius.io/better-javascript-javascript-versions-and-numbers-ef3778a8d5e2?source=collection_archive---------4----------------------->

![](img/333681543a928bac10f1bdb781ca48b3.png)

[安德鲁·布坎南](https://unsplash.com/@photoart2018?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

与任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 了解我们使用的 JavaScript 版本

了解我们使用的是哪个 JavaScript 版本很重要。

这是因为每个版本都有改进。

ES6 是变化最多的一个版本。

如果我们使用它的特性，它会使代码与旧的 JavaScript 非常不同。

此外，我们要注意的非标准功能，可能会突然出现偶尔。

对于新功能，浏览器支持可能有所不同。

所以我们应该注意这些特征的变化。

此外，API 特性也可能随不同的 JavaScript 引擎而变化。

因此，我们希望确保我们支持的浏览器能够为我们想要的特性提供支持。

否则，我们需要像 Babel 和 polyfill 库这样的程序来运行我们的应用程序。

严格模式是我们必须考虑的 JavaScript 的另一个方面。

它可以防止我们犯错误，比如赋值给像`undefined`这样的全局值，或者意外地创建全局变量。

它应该与旧代码向后兼容，因此我们可以使用`'use strict'`指令在部分代码中应用严格模式。

所以我们可以补充一点:

```
"use strict";
```

到顶端或者我们可以写:

```
function foo(x) {
  "use strict";
  // ...
}
```

通过打开严格模式，我们可以编写如下代码:

```
function foo(x) {
  "use strict";
  var arguments = [];
  // ...
}
```

然后，我们得到错误“uncathed 语法错误:意外的评估或严格模式中的参数”。

但是，如果我们需要将具有严格模式的文件与不具有严格模式的文件连接起来，那么我们可能会遇到问题。

如果我们在一个文件的顶部启用了严格模式，而在另一个文件的顶部为一个函数启用了严格模式，那么严格模式将应用于所有地方。

为了减少出现问题的机会，我们应该让我们的代码走出全局范围。

所以我们可以这样写:

`file1.js`

```
(function() {
  "use strict"; function f() {
    // ...
  }
  //...})()
```

以及:

`file2.js`

```
(function() {
  function f() {
    // ...
  }
  //...})()
```

`file1.js`开启了严格模式，而`file2.js`没有。

所以这个要好得多。

函数和变量也是私有的，这是另一个好处。

# JavaScript 的浮点数字

JavaScript 只有一种数字。

它们都是浮点型的。

所以:

```
typeof 27;
typeof 9.6;
typeof -2.1;
```

全部返回`'number'`。

JavaScript 数字是双精度浮点数字。

它们是 IEEE 754 标准规定的数字的 64 位编码。

大多数算术运算符处理任何类型的数字。

所以我们可以这样写:

```
0.1 * 1.9
-99 + 100;
21 - 12.3;
```

按位运算符仅适用于整数。

例如，我们写道:

```
5 | 1;
```

我们得到 5。

一个简单的表达式可能需要多个步骤来计算。

如果我们将一个数字转换成二进制字符串:

```
(5).toString(2); 
```

`toString`的基数为 2，这意味着我们转换为二进制。

所以十进制 5 转换成二进制数字。

它会丢弃左边额外的 0 位，因为它们不会影响值。

按位运算符的工作方式相同。

整数被转换成比特，这样就可以对它们进行运算。

![](img/97e247e2d501759a8aef69a3e37d2315.png)

尼克·希利尔在 Unsplash[上的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该检查我们支持的 JavaScript 版本，以便我们的应用程序可以运行。

另外，我们应该知道 JavaScript 只有一种数字。