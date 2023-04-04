# 面向对象的 JavaScript —参数和内置函数

> 原文：<https://blog.devgenius.io/object-oriented-javascript-parameters-and-built-in-functions-8aadd4218912?source=collection_archive---------5----------------------->

![](img/4461e3b98fc448cdd3a0737300a738f8.png)

杰斯·贝利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究各种函数和参数。

# 默认参数

我们可以在函数中设置默认参数。

为此，我们只需给一个参数赋值。

我们可以写:

```
function render(fogLevel = 0, sparkLevel = 100) {
  console.log(`Fog Level: ${fogLevel} and spark level:
${sparkLevel}`)
}
```

我们有一个带有`fogLevel`和`sparkLevel`参数的`render`函数。

我们将它们的默认值分别设置为 0 和 100。

那么如果我们有:

```
render()
```

然后我们得到:

```
'Fog Level: 0 and spark level: 100'
```

默认参数有自己的范围。

作用域夹在外部函数作用域和内部函数作用域之间。

所以如果我们有:

```
let scope = "outer";function scoper(val = scope) {
  let scope = "inner";
  console.log(val);
}
scoper();
```

那么`val`将是`'outer'`，因为它从函数外部取`scope`。

# 休息参数

Rest 参数让我们可以从函数中获得任意数量的参数。

例如，我们可以写:

```
function getTodos(...todos) {
  console.log(Array.isArray(todos));
  console.log(todos)
}
```

然后我们得到的第一个控制台日志是`true`。

而第二个是`[“eat”, “drink”, “sleep”]`。

`todos`是一个数组，因为它前面有一个 rest 操作符。

这使得它得到参数，并把它们都放在`todos`数组中。

我们通过第二个控制台日志获得所有这些信息。

# 扩展运算符

spread 操作符允许我们将数组条目作为函数的参数展开。

我们也可以用它在数组中传播数值。

例如，我们可以写:

```
const min = Math.min(...[1, 2, 3]);
```

这取代了旧的方式，即:

```
const min = Math.min.apply(undefined, [1, 2, 3]);
```

扩展运算符更容易理解，也更简短。

1，2，3 是`Math.min`的自变量。

我们还可以使用 spread 操作符将数组项合并到另一个数组中。

例如，我们可以写:

```
const midweek = ['Wed', 'Thu'];
const weekend = ['Sat', 'Sun'];
const week = ['Mon', 'Tue', ...midweek, 'Fri', ...weekend]
```

那么`week`就是:

```
["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
```

# 预定义功能

JavaScript 中有许多预定义的函数。

他们是:

*   `parseInt()`
*   `parseFloat()`
*   `isNaN()`
*   `isFinite()`
*   `encodeURI()`
*   `decodeURI()`
*   `encodeURIComponent()`
*   `decodeURIComponent()`
*   `eval()`

`parseInt`将非数字值解析为整数。

例如，我们可以通过写来使用`parseInt`:

```
parseInt('123')
```

`parseFloat`将非数字值解析为浮点数。

我们得到了`123`。

例如，我们可以这样写`parseFloat`:

```
parseFloat('123.45')
```

我们得到了`123.45`。

`isNaN`检查值是否为`NaN`。

所以如果我们有:

```
isNaN(NaN)
```

我们得到`true`。

`isFinite`检查一个值是否是一个有限数。

所以我们可以写:

```
isFinite(Infinity)
```

我们得到了`false`。

我们可以用`encodeURI`将一个字符串编码成一个 URI。

例如，如果我们有:

```
const url = 'http://www.example.com/foo?bar=this and that';
console.log(encodeURI(url));
```

我们得到了:

```
'http://www.example.com/foo?bar=this%20and%20that'
```

我们可以用`decodeURI`解码 URI 字符串。

`encoudeURIComponent`和`decodeURIComponent`可以对 URI 的一部分做同样的事情。

如果我们写:

```
const url = 'http://www.example.com/foo?bar=this and that';
console.log(encodeURIComponent(url));
```

我们得到了:

```
'http%3A%2F%2Fwww.example.com%2Ffoo%3Fbar%3Dthis%20and%20that'
```

`eval`让我们从字符串中运行代码。我们不应该使用它，因为任何人都可以向它传递一个恶意的字符串。

而字符串中的代码是无法优化的。

![](img/09ab3b0c41362d0d3d4c77bedfba2962.png)

Victor Grabarczyk 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

默认参数让我们设置参数的默认值，如果它们没有用实参设置的话。

JavaScript 附带了一些我们可以使用的预定义函数。

rest 和 spread 操作符在处理参数时非常方便。