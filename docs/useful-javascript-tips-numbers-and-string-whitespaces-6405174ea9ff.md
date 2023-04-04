# 有用的 JavaScript 技巧——数字和字符串空格

> 原文：<https://blog.devgenius.io/useful-javascript-tips-numbers-and-string-whitespaces-6405174ea9ff?source=collection_archive---------19----------------------->

![](img/2e51c162df5d099493a6841eab6d9a26.png)

布鲁克·拉克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 检查一个数是否有限

方法让我们检查一个值是否是一个有限的数字。

例如:

```
Number.isFinite(1)
```

返回`true`。

另一方面:

```
Number.isFinite(1)
```

返回`false`。

# 将十进制数格式化为字符串

我们可以使用`toFixed`方法将十进制数格式化成字符串。

例如，我们可以写:

```
(20.22).toFixed()
```

并得到`'20'`。

此外，我们可以通过编写以下内容来传递我们想要保留的十进制位数:

```
(20.22).toFixed(1)
```

并得到`“20.2”`。

# 将数字格式化为区分语言的字符串

`toLocaleString`方法让我们获得一个字符串，该字符串包含一个根据区域设置格式化的数字。

例如，我们可以写:

```
(20.22).toLocaleString('fr-ca')
```

然后我们得到`“20,22”`。

# 将数字格式化为指数记数法

`toExponential`方法让我们将一个数字转换成一个指数符号的字符串。

例如，我们可以写:

```
(21.22).toExponential()
```

然后我们得到`“2.122e+1”`。

# 更改数字的小数位数

`toPrecision`方法将一个数字格式化为具有给定小数位数的字符串。

例如，我们可以写:

```
(21.21).toPrecision(5)
```

然后我们得到`“21.210”`。

我们也可以传入一个比数字的十进制位数小的数字。

然后我们得到一个字符串，其中的数字被格式化成指数符号。

例如，我们可以写:

```
(21.21).toPrecision(1)
```

我们得到了`“2e+1”`。

# 将数字转换为字符串

方法将一个数字转换成一个字符串。

它的参数是基数。

例如，如果我们想把十进制数 10 转换成二进制数，我们可以写:

```
(10).toString(2)
```

然后返回`“1010"`。

# 将字符串解析为整数

我们可以用`parseInt`方法将一个字符串解析成一个整数。

例如，我们可以写:

```
Number.parseInt('10.00')
```

我们能拿回 10 美元。

它还需要基数的第二个参数。

所以如果我们想把它转换成一个给定的基数，我们可以这样写:

```
Number.parseInt('16', 16)
```

然后我们得到 22，因为我们将`'16'`转换为基数为 16 的数。

# 将字符串解析为浮点数

有一个将字符串转换成浮点数的`Number.parseFloat`方法。

例如，我们可以写:

```
Number.parseFloat('10.00')
```

得到 10 分。

如果字符串以数字开头，它还可以通过删除非数字部分并将其余部分转换为数字来将字符串转换为数字。

例如，我们可以写:

```
Number.parseFloat('36 bottles')
```

然后我们得到 36。

# 检查数字是否在安全范围内

在 JavaScript 中，如果一个数字在`-2 ** 53`和`2 ** 53`之间，它就被认为是安全的。

为了检查这一点，我们可以使用`Number.isSafeInteger`方法。

例如，我们可以写:

```
Number.isSafeInteger(2 ** 53)
```

它返回`true`，并且:

```
Number.isSafeInteger(2 ** 53 + 1)
```

也就是`false`。

# 检查值是否为 NaN

检查`NaN`很棘手。

因此，我们应该使用`Number.isNaN`来做，因为我们不能使用`===`来做比较。

例如，我们可以写:

```
Number.isNaN(NaN)
```

这将返回`true`。

另外:

```
Number.isNaN('foo' / 'bar')
```

返回`true`。

# 检查值是否为整数

`isInteger`方法让我们检查一个值是否是整数。

我们可以通过书写来使用它:

```
Number.isInteger(1)
```

返回`true`，或者:

```
Number.isInteger(0.2)
```

返回`false`。

# 修剪字符串的开头

我们可以用`trimStart`方法删除字符串开头的空白。

例如，我们可以写:

```
'   foo'.trimStart()
```

然后返回`“foo”`。

# 修剪弦的末端

还有一个`trimEnd`方法来删除字符串末尾的空格。

例如，我们写道:

```
'foo   '.trimStart()
```

然后返回`“foo”`。

![](img/e12c272049d1be70b1283947dfe3622c.png)

照片由 [Kouji 鹤](https://unsplash.com/@pafuxu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 修剪绳子的两端

方法将删除字符串两端的空格。

例如，我们可以写:

```
'   foo   '.trim()
```

然后返回`'foo'`。

# 结论

有各种方法来解析和检查数字。

此外，JavaScript 字符串有删除空白的方法。