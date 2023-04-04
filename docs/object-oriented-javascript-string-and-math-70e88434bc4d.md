# 面向对象的 JavaScript —字符串和数学

> 原文：<https://blog.devgenius.io/object-oriented-javascript-string-and-math-70e88434bc4d?source=collection_archive---------4----------------------->

![](img/81cb374a4817f3f34b1a24063d964278.png)

照片由 [Kira auf der Heide](https://unsplash.com/@kadh?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看`String`和`Math`对象。

# 字符串对象方法

字符串有各种各样的方法，我们可以调用它们来做一些事情。

`toUpperCase`方法让我们返回字符串的大写版本。

例如，如果我们有:

```
const s = 'foo';
```

然后:

```
s.toUpperCase();
```

返回`'FOO'`。

`toLowerCase`方法将一个字符串全部转换成小写。

例如:

```
s.toLowerCase()
```

返回`'foo'`。

`charAt`方法返回在我们指定的位置找到的字符。

例如，如果我们有:

```
s.charAt(0);
```

然后我们得到`'f'`。

我们可以只写`s[0]`简称。

如果我们将一个不存在的位置传递给`charAt`，我们会得到一个空字符串。

`indexOf`方法让我们在字符串中搜索子串。

例如，如果我们有:

```
s.indexOf('o');
```

我们得到 1。

此外，我们可以选择用`indexOf`指定开始搜索的位置。

例如，我们可以写:

```
s.indexOf('o', 2);
```

我们得到 2。

`lastIndexOf`方法从字符串的末尾开始搜索。

例如，如果我们写:

```
s.lastIndexOf('o');
```

我们得到 2。

我们也可以搜索一系列字符。

例如，如果我们有:

```
const str = 'foobar';
str.indexOf('bar');
```

我们得到 3。

它们可以组合在一起。

所以我们可以写:

```
s.toLowerCase().indexOf('foo'.toLowerCase());
```

当我们指定开始和结束位置时，`slice()` 和`substring()`方法返回一段字符串。

如果我们有:

```
const str = 'foobar';
str.slice(1, 3);
str.substring(1, 3);
```

我们得到了`'oo'`。

`split`方法让我们用分隔符分割字符串。

例如，我们可以写:

```
const s = 'foo bar';
const arr = s.split(" ");console.log(arr);
```

那么`arr`就是`[“foo”, “bar”]`因为我们隔了一个空格。

`join`方法用一个分隔符将字符串数组连接成一个数组。

所以如果我们有:

```
["foo", "bar"].join(' ')
```

我们得到:

```
"foo bar"
```

方法将一个字符串追加到一个现有的字符串中。

所以如果我们有:

```
'foo'.concat("bar")
```

我们得到:

```
"foobar"
```

# 数学

`Math`对象不是一个函数。

它是一个有各种常量和方法的对象。

它包括的常数有:

*   `Math.PI` — pi
*   `Math.SQRT2`—2 的平方根
*   `Math.E` —欧拉常数
*   `Math.LN2`——2 的自然对数
*   `Math.LN10`-10 的自然对数

`Math.random()`可以生成 0 到 1 之间的随机数。

我们可以用这个表达式:

```
((max - min) * Math.random()) + min
```

在`min`和`max`之间产生一个随机数。

`Math`四舍五入的方法多种多样。

`floor()`向下舍入。

`ceil()`回合结束。

`round()`四舍五入至最接近的整数。

我们可以通过传入一个数字来使用它们:

```
Math.floor(9.1)
Math.ceil(9.1)
Math.round(9.1)
```

`Math.pow`将基数升至指数。

例如，kf 我们有:

```
Math.pow(2, 3);
```

然后我们得到 8。2 是基数，3 是指数。

`Math.sqrt`方法返回一个数字的平方根。

例如，我们可以写:

```
Math.sqrt(9);
```

我们得到 3。

![](img/4b4ce9e110de510e5f58fd318ff16192.png)

[cross y Jarvis](https://unsplash.com/@crissyjarvis?utm_source=medium&utm_medium=referral)在[unsprash](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄的照片

# 结论

字符串有各种方法让我们操纵它们。

`Math`对象让我们可以进行各种操作，并为我们提供一些常数。