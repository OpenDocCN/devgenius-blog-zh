# 有用的 JavaScript 技巧——睡眠和数学

> 原文：<https://blog.devgenius.io/useful-javascript-tips-sleep-and-math-28224ce0ff8c?source=collection_archive---------18----------------------->

![](img/1cbad0e1855075b80bc1dc3f00d856d4.png)

Anna Dziubinska 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 暂停功能的执行

我们可以通过创建自己的`sleep`函数来暂停函数的执行。

例如，我们可以写:

```
const sleep = (ms) => {
  return new Promise(resolve => setTimeout(resolve, ms))
}
```

我们用`setTimeour`函数返回一个承诺。回调是`resolve`函数。

`ms`是`ms`中的暂停执行量。

然后我们可以通过写来使用它:

```
const foo = async () => {
  await sleep(2000);
  // do more stuff
}
```

我们用 2000 调用了`sleep`，所以在运行`sleep`调用下面的代码之前，函数会暂停 2 秒钟。

它可以与循环一起使用:

```
const loop = async () => {
  for (const item of arr) {
    await sleep(2000);
    console.log(item);   
  }
}
```

for-of 循环可以与承诺一起使用。

所以`sleep`会暂停循环体 2 秒。

# 生成随机和唯一的字符串

我们可以使用`randomstring`包来生成随机字符串。

例如，我们可以这样写:

```
const randomstring = require('randomstring');
randomstring.generate(20);
```

以生成长度为 20 的字符串。

为了生成多个唯一的字符串，我们可以将所有的项目放在一个集合中。

例如，我们可以写:

```
const randomstring = require('randomstring');
const strings = new Set()while (strings.size < 50) {
  strings.add(randomstring.generate(20));
}
```

然后，我们生成唯一的字符串，直到集合有 50 个字符串。

然后我们可以用`values`方法检索字符串。

例如，我们可以写:

```
for (const value of strings.values()) {
  console.log(value);
}
```

# JavaScript 数学对象

JavaScript `Math`对象有许多属性和方法来简化数学运算。

## Math.abs()

`Math.abs`方法返回一个数字的绝对值。

我们可以写:

```
Math.abs(-10)
```

得到 10 分。

## Math.acos()

返回参数的反余弦值。

例如，我们可以写:

```
Math.acos(0.3)
```

我们得到 1.2661036727794992。

## Math.asin()

返回参数的反正弦值。

例如，我们可以写:

```
Math.asin(0.5)
```

我们得到 0.525987755982989。

## Math.atan()

返回参数的矩形。

例如，我们可以写:

```
Math.atan(20)
```

我们得到 1.5208379310729538。

## Math.atan2()

返回其参数商的反正切值。

例如，我们可以写:

```
Math.atan2(3, 2)
```

并得到 0.982793723247329。

## Math.ceil()

将数字向上舍入到最接近的整数。

例如，我们可以写:

```
Math.ceil(1.99999)
```

然后得到 2。

## Math.cos()

返回以弧度表示的角度的余弦值。

例如，我们可以写:

```
Math.cos(0)
```

得到 1。

## Math.exp()

返回作为参数传递的每个指数乘以`e`的值。

例如，我们可以写:

```
Math.exp(2)
```

并得到 7.38905609893065。

## Math.floor()

将数字向下舍入到最接近的整数。

例如，我们可以写:

```
Math.floor(1.99999)
```

得到 1。

## Math.log()

获取一个数字的自然对数。

例如，我们可以写:

```
Math.log(Math.E)
```

得到 1。

## Math.max()

获取作为参数传递的数字集中的最大数字。

例如，我们可以写:

```
Math.max(1, 2, 3)
```

得到 3。

## Math.min()

获取作为参数传递的数字集中的最小数字。

例如，我们可以写:

```
Math.max(1, 2, 3)
```

得到 1。

## Math.pow()

将第一个参数提升到第二个参数。

例如，我们可以写:

```
Math.pow(2, 3)
```

得到 8 分。

## Math.random()

返回一个介于 0 和 1 之间的伪随机数。

例如，我们可以写:

```
Math.random()
```

并得到 0.08500663407619236。

## Math.round()

返回最接近的整数。

例如，我们可以写:

```
Math.round(1.3)
```

得到 1。

## Math.sin()

获取以弧度表示的角度的正弦值。

例如，我们可以写:

```
Math.sin(0)
```

并得到 0。

## Math.sqrt()

获取参数的平方根。

例如，我们可以写:

```
Math.sqrt(9)
```

得到 3。

## Math.tan()

获取以弧度表示的角度。

例如，我们可以写:

```
Math.tan(Math.PI * 2)
```

我们得到-2.442935982947064 e-16。

# 算术运算符

JavaScript 有常用的算术运算符。

它们是加法、减法、乘法、除法、余数和取幂。

然后我们可以写:

```
const three = 1 + 2;
```

用于添加。

但是我们必须确保两个操作数都是数字。否则，它被用作连接。

减法写成:

```
const three = 5 - 2;
```

除法可以写成:

```
const result = 20 / 10;
```

如果我们除以零，我们得到`Infinity`或`-Infinity`。

乘法可以这样写:

```
1 * 2
```

我们可以写下指数:

```
2 ** 3
```

得到 8 分。

我们可以写出除法的余数:

```
const result = 20 % 6
```

然后得到 2。

![](img/da4097637c87f1eb4b47f3e19671d9cd.png)

照片由 [Kouji 鹤](https://unsplash.com/@pafuxu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以使用许多运算和数学方法。

此外，我们可以通过使用`setTimeout`来暂停功能的执行。