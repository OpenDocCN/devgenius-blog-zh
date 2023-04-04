# 如何格式化一个 JavaScript 日期？

> 原文：<https://blog.devgenius.io/how-to-format-a-javascript-date-c2d19f1b61e8?source=collection_archive---------6----------------------->

![](img/808e820370aceb2e720aec6c29fb5a89.png)

照片由[克莱门特·法利泽](https://unsplash.com/@centelm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

格式化日期是我们在 JavaScript 应用程序中经常要做的事情。

在本文中，我们将研究如何用 JavaScript 格式化日期。

# 国际机场。DateTimeFormat 构造函数

格式化日期最简单的方法是使用`Intl.DateTimeFormat`构造函数。

它用`format`方法返回一个对象。

例如，我们可以写:

```
const date = new Date()
const dateFormat = new Intl.DateTimeFormat('en');
const formattedDate = dateFormat.formatToParts(date);
console.log(formattedDate)
```

`formatToParts`方法将以数组的形式获取日期对象的各个部分。

结果，我们得到:

```
[
  {
    "type": "month",
    "value": "2"
  },
  {
    "type": "literal",
    "value": "/"
  },
  {
    "type": "day",
    "value": "12"
  },
  {
    "type": "literal",
    "value": "/"
  },
  {
    "type": "year",
    "value": "2021"
  }
]
```

作为`formattedDate`的值。

然后我们可以在一个字符串中以我们喜欢的方式组合这些值。

为此，我们写道:

```
const date = new Date()
const dateFormat = new Intl.DateTimeFormat('en');
const formattedDate = dateFormat.formatToParts(date);
const formattedDateObj = formattedDate.reduce((formattedDateObj, {
  type,
  value
}) => ({
  ...formattedDateObj,
  [type]: value
}), {});
console.log(`${formattedDateObj.year}-${formattedDateObj.month}-${formattedDateObj.day}`)
```

我们通过回调函数调用`reduce`来从数组中创建`formattedDateObj`对象。

回调采用了`formattedDateObj`，这是我们到目前为止已经创建的对象。

第二个参数是数组中具有`type`和`value`属性的对象。

我们返回一个对象，它带有一个`formattedDateObj`对象的副本，并在最后添加了新的属性。

`reduce`的第二个参数是一个空对象。

我们将其设置为一个空对象，因此我们可以转换`formattedDate`数组的条目并将其放入空对象中。

然后，我们可以将这些属性组合成我们自己的日期字符串，就像最后一行中所做的那样。

获取日期各部分的另一种方法是单独提取日期的各部分。

例如，我们可以写:

```
const d = new Date();
const ye = new Intl.DateTimeFormat('en', {
  year: 'numeric'
}).format(d);
const mo = new Intl.DateTimeFormat('en', {
  month: 'short'
}).format(d);
const da = new Intl.DateTimeFormat('en', {
  day: '2-digit'
}).format(d);
console.log(`${da}-${mo}-${ye}`);
```

我们使用带有区域设置和对象的`IntlDateTimFormat`构造函数来指定如何将对象格式化为参数。

`'numeric'`返回 4 位数的年份。

`'short'`返回月份缩写。

并且`'2-digit'`返回 2 位数的日期。

所以我们应该得到这样的结果:

```
'12-Feb-2021'
```

已退回。

# `The toLocaleDateString Method`

`toLocaleDateString`方法也让我们可以轻松地设置日期格式。

例如，我们可以写:

```
const options = {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric'
};
const today = new Date();
console.log(today.toLocaleDateString("en-US", options));
```

我们用`options`对象来指定日期的格式。

选项与上一个示例中的相同。

`toLocaleString`将区域设置和选项作为参数。

因此，我们得到:

```
'Friday, February 12, 2021'
```

# 结论

我们可以用`Intl.DateTimeFormat`构造函数或`toLocaleDateString`方法轻松格式化 JavaScript 日期对象。