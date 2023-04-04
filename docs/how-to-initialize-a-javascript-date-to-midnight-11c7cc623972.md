# 如何将 JavaScript 日期初始化为午夜？

> 原文：<https://blog.devgenius.io/how-to-initialize-a-javascript-date-to-midnight-11c7cc623972?source=collection_archive---------0----------------------->

![](img/64145642e47cac4c2d4c703bab7f6f20.png)

萨姆·穆卡达姆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们可能希望将 JavaScript 日期设置为午夜。

在本文中，我们将研究如何将 JavaScript 日期初始化为午夜。

# 日期.原型.设定时间

我们可以使用`setHours`方法将日期的小时设置为午夜。

例如，我们可以写:

```
const d = new Date();
d.setHours(0, 0, 0, 0);
console.log(d)
```

我们调用`setHours`，将小时、分钟、秒和毫秒都设置为 0，以将日期`d`设置为与原始日期`d`相同日期的午夜。

这种变化使`d`对象发生变异。

因此我们从控制台日志中看到`d`是当前日期的午夜。

要将日期设置为明天的午夜，我们可以写:

```
const d = new Date();
d.setHours(24, 0, 0, 0);
console.log(d)
```

第一个参数是小时参数，是 24 而不是 0。

从控制台日志中，我们应该看到日期是第二天午夜。

如果我们想复制日期，我们可以将日期传递给`Date`构造函数:

```
const d = new Date(new Date().setHours(0, 0, 0, 0));
console.log(d)
```

# 日期.原型.设置时间

如果我们想使用 UTC 时间，我们可以使用`setUTCHours`方法将时间设置为午夜。

例如，我们可以写:

```
const d = new Date();
d.setUTCHours(0, 0, 0, 0);
console.log(d)
```

将日期设置为 UTC 午夜。

# moment.js

moment.js 库让我们用`startOf`方法将日期设置为午夜。

例如，我们可以写:

```
const d = moment().startOf('day');
console.log(d)
```

我们用`'day'`调用`startOf`，将 moment 对象的日期时间设置为 moment 日期所在日期的午夜。

# 结论

我们可以用本地 JavaScript 日期方法将 JavaScript 日期设置为午夜。

同样，我们可以使用 moment.js 来做同样的事情。