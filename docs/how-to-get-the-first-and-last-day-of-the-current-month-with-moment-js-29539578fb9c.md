# 如何用 Moment.js 获取当月的第一天和最后一天？

> 原文：<https://blog.devgenius.io/how-to-get-the-first-and-last-day-of-the-current-month-with-moment-js-29539578fb9c?source=collection_archive---------1----------------------->

![](img/a40b310e7ee73bb7a8bb3c07027b6cc2.png)

Elena Mozhvilo 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们希望在 JavaScript 应用程序中获得当月的第一天和最后一天。

我们可以用 moment.js 轻松做到这一点。

在本文中，我们将看看如何用 Moment.js 获取当月的第一天和最后一天。

# 使用 Moment.js startOf 和 endOf 方法

我们可以使用`startOf`方法获得当前月的第一天。

并且我们可以使用`endOf`方法获得当前月的最后一天。

例如，我们可以写:

```
const startOfMonth = moment().clone().startOf('month').format('YYYY-MM-DD hh:mm');
const endOfMonth = moment().clone().endOf('month').format('YYYY-MM-DD hh:mm');
console.log(startOfMonth)
console.log(endOfMonth)
```

我们调用`moment`来创建一个带有当前日期和时间的 Moment 对象。

然后我们调用`clone`来克隆这个对象。

然后我们用`'month'`调用`startOf`返回当月的第一天。

然后我们调用`format`将日期格式化为人类可读的 YYYY-MM-DD 格式。

同样，我们用`endOf`方法来获取当月的最后一天。

如果当前月份是 2021 年 4 月，那么我们应该得到:

```
'2021-04-01 12:00'
```

对于`startOfMonth`。

而`endOfMonth`应该是:

```
'2021-04-30 11:59'
```

`startOf`和`endOf`也接受`'year'`、`'week'`和`'day'`作为参数，以获取给定日期和时间的第一年和最后一年、周或日。

# 结论

我们可以分别用 Moment.js 的`startOf`和`endOf`方法轻松得到当月的第一天和最后一天。