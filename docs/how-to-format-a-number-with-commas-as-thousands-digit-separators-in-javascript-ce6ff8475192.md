# 如何在 JavaScript 中用逗号作为千位分隔符来格式化一个数字？

> 原文：<https://blog.devgenius.io/how-to-format-a-number-with-commas-as-thousands-digit-separators-in-javascript-ce6ff8475192?source=collection_archive---------0----------------------->

![](img/7e1f12c994895af425df37765622a653.png)

[Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

用逗号作为千位分隔符来格式化一个数字是为了让数字更容易阅读。

在本文中，我们将了解如何在 JavaScript 中用逗号将数字格式化为千位分隔符。

# 替换为正则表达式

我们可以使用带有正则表达式的`replace`方法，每三位数添加一个逗号。

为此，我们写道:

```
const str = (1234567890).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
console.log(str)
```

`\B(?=(\d{3})+`查找小数点前 3 位数的组。

`\B`防止`replace`在字符串的开头添加逗号。

`?=`是一个前瞻断言。

这意味着如果 3 位数模式后面有非单词边界，我们只匹配 3 位数。

`?!`是一个否定的前瞻断言。

这意味着只有在一位数模式前面有三位数模式时，我们才匹配它。

因此，我们确保只有在 3 位数字后面跟着另一位数字时才插入逗号。

所以我们得到`‘1,234,567,890’`作为`str`的值

# `The toLocaleString Method`

`toLocaleString`方法让我们自动将一个数字格式化成一个字符串，用逗号作为千位分隔符。

例如，我们可以写:

```
const str = (1234567890).toLocaleString()
console.log(str)
```

我们在想要格式化的数字上直接调用`toLocaleString`。

所以我们得到`‘1,234,567,890’`作为`str`的值

我们还可以设置区域设置，以保证格式化后的值总是包含逗号:

```
const str = (1234567890).toLocaleString('en-US')
console.log(str)
```

# 数字格式构造函数

我们也可以使用一个`NumberFormat`实例的`format`方法。

为了使用它，我们写:

```
const nf = new Intl.NumberFormat();
const str = nf.format(1234567890);
console.log(str)
```

我们创建一个`Intl.NumberFormat`实例。

然后我们用一个数字调用实例上的`format`。

然后我们得到和其他例子一样的结果。

# 结论

我们可以用正则表达式替换来添加逗号作为千位分隔符，或者我们可以使用格式化程序函数来做同样的事情。