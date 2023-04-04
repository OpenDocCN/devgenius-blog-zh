# 如何给一个 JavaScript 日期对象添加小时？

> 原文：<https://blog.devgenius.io/how-to-add-hours-to-a-javascript-date-object-860d8170b04f?source=collection_archive---------3----------------------->

![](img/7e1d7f9a66a36f60519601b34b8afe63.png)

[阿格巴洛斯](https://unsplash.com/@agebarros?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

有时，我们可能想给 JavaScript 对象增加时间。

在本文中，我们将研究如何向 JavaScript 日期对象添加小时。

# Date.prototype.getTime 和 Date.prototype.setTime

我们可以使用 JavaScript native `getTime`方法来获取以毫秒为单位的日期时间戳。

我们可以使用`setTime`方法来设置日期的时间戳，以毫秒为单位。

因此，要向现有日期添加小时数，我们可以写:

```
const date = new Date(2021, 1, 1)
const h = 2
date.setTime(date.getTime() + (h * 60 * 60 * 1000))
console.log(date)
```

我们有一个`date`对象，它有一个我们想要添加小时数的日期。

`h`有我们想要添加的小时数。

然后我们用新日期的时间戳(以毫秒为单位)调用`setTime`。

我们通过编写以下内容得到时间戳`date`加 2 小时:

```
date.getTime() + (h * 60 * 60 * 1000)
```

`(h * 60 * 60 * 1000)`是以毫秒为单位的 2 小时。

那么我们得到`date`是:

```
Mon Feb 01 2021 02:00:00 GMT-0800 (Pacific Standard Time)
```

# Date.prototype.getHours 和 Date.prototype.setHours

我们还可以使用`getHours`和`setHours`方法分别获取和设置 JavaScript 日期对象的小时。

例如，我们可以写:

```
const date = new Date(2021, 1, 1)
const h = 2
date.setHours(date.getHours() + h)
console.log(date)
```

然后我们得到与上一个例子相同的结果。

我们可能还想制作原始日期对象的副本，然后在复制的日期对象中更改小时。

例如，我们可以写:

```
const date = new Date(2021, 1, 1)
const h = 2
const copiedDate = new Date(date.getTime());
copiedDate.setHours(date.getHours() + h)
console.log(copiedDate)
```

我们通过向`Date`构造函数传递以毫秒为单位的时间戳来制作副本，就像我们在第 3 行中所做的那样。

`getTime`返回以毫秒为单位的时间戳。

# 日期.原型.`getUTCHours`和日期.原型.`setUTCHours`

我们还可以使用`getUTCHours`和`setUTCHours`方法分别获取和设置 JavaScript 日期对象的小时。

`getUTCHours`以 UTC 返回一天中的小时。

`setUTCHours`以 UTC 设置小时。

所以我们可以写:

```
const date = new Date(2021, 1, 1)
const h = 2
date.setUTCHours(date.getUTCHours() + h)
console.log(date)
```

给`date`增加 2 小时。

# Moment.js

我们还可以使用 moment.js 为日期添加小时。

例如，我们可以写:

```
const newDate = new moment(new Date(2021, 1, 1)).add(10, 'hours').toDate();
console.log(newDate)
```

将日期增加 10 个小时，然后传递到`moment`函数中。

我们在上面调用`add`来增加 10 个小时。

第一个参数是要添加的数量。

第二个参数是要相加的量的单位。

`toDate`将返回的带有新日期时间的 moment 对象转换回本地 JavaScript date 对象。

所以`newDate`是:

```
Mon Feb 01 2021 10:00:00 GMT-0800 (Pacific Standard Time)
```

从控制台日志中可以看到。

# 结论

我们可以用原生 JavaScript date 方法或 moment.js 为日期添加小时数。