# 如何使用 JavaScript Spread 运算符，节省时间！—币安市场数据示例

> 原文：<https://blog.devgenius.io/how-to-spread-binance-market-data-using-javascript-spread-operator-537426f2e716?source=collection_archive---------12----------------------->

![](img/2d3cd64fee2071c69a3ecd242ee1bb97.png)

照片来自[斯蒂芬妮·勒布朗](https://unsplash.com/@sleblanc01)，Unsplash

S 有时候，你可能想轻松地复制或制作 Javascript 对象或数组的浅拷贝，或者可能向函数或类方法传递许多参数。

通常使用扩展运算符**来实现这个目标**。

使用这个操作符可以使代码简洁并增强其可读性。

在本文的后面，我将“**传播来自币安 API 的**样本数据。

# 为什么我们要使用扩展运算符？

spread 操作符是 JavaScript ES6 中操作符集合的新成员。它接受一个可迭代的对象(比如一个数组)并将其扩展成独立的元素。

## 句法

spread 运算符的语法由三个点(…)表示

后跟对象或数组或函数/方法参数的名称。

本文稍后将解释具体的用例。

# **如何使用 ES6 Javascript Spread 运算符**

Spread 运算符在以下用例中非常有用。

## 1.目标

让我们使用 Spread 操作符来组合这两个对象(myVehicle 和 updateMyVehicle)

请注意，颜色属性 red 已更新为黄色。这意味着使用 spread 操作符可以用另一个对象的新值更新属性。

我还可以在 **myUpdateVehicle** 对象的任何位置包含任何属性。我会包括**车主**、**马力**、**任何东西。**你应该试试**！**

控制台通常会按字母顺序排列属性，因为它们在对象中的位置并不重要。

## 2.数组

1.  我们可以使用 Spread 运算符轻松地组合 too 数组。下面是如何做的:

```
const numbersOne = [1, 2, 3];
const numbersTwo = [4, 5, 6];
const numbersCombined = [...numbersOne, ...numbersTwo];
```

您还可以将此与重组结合起来。这是怎么回事:

> “将`numbers`中的第一项和第二项赋给变量，其余的放入一个数组”——【W3schools.com】T21

```
const numbers = [1, 2, 3, 4, 5, 6];

const [one, two, ...rest] = numbers;
```

## 3.休息参数

rest 参数为您提供了一种健壮的方法来处理数量不定的参数。它涉及到在创建函数或方法时使用 Spread 运算符。

为了更好地理解什么是 rest 参数，让我们从头开始。

无论如何定义，你都可以调用一个参数个数不确定的函数。

例如:

您可能也想这样做:

```
...
function multiplyAll( parameter1, parameter2, ...args)
...
```

您仍然可以像往常一样访问 args 参数和其他参数？找出答案。

# 传播币安市场数据作为样本数据

币安提供了一个良好记录的 API，使第三方集成和访问币安数据和指标。

## 第一步

在这个例子中，我将从币安获得 BTC-USDT 符号的 5 次交易的结果作为市场数据，使用这个 API 端点( ***/api/v3/trades)、*** 来访问交易信息 ***。***

## 第二步

我将传播第一步(交易)的结果，然后传播第一个数组元素或数据的属性，即第一笔交易。

你也应该试试。

**下面是代码:**

# 结论

使用 Rest 操作符(`...`)，您可以快速地将一个现有数组或对象的全部或部分复制到另一个数组或对象中，而不会影响原来的对象或数组。

您还可以使用 spread 运算符来扩展函数或方法的参数。

在本文中，我使用了币安市场数据对象和数组作为例子。如果你想知道为什么；我喜欢展示样本代码的真实例子。**是的**，我在代码中使用了 spread 操作符。你也应该试试。

*感谢阅读！*

## 你应该阅读对象析构以及它如何帮助你节省时间！在下面的文章中:

[](/how-to-properly-destructure-javascript-objects-and-safe-time-560b564271eb) [## 如何析构 Javascript 对象和安全时间！

### 在某些时候，在编码时，我们发现自己经常使用点符号来引用对象属性…

blog.devgenius.io](/how-to-properly-destructure-javascript-objects-and-safe-time-560b564271eb)