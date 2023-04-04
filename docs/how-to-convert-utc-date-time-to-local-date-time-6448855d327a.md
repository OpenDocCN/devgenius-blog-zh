# 如何将 UTC 日期时间转换为本地日期时间？

> 原文：<https://blog.devgenius.io/how-to-convert-utc-date-time-to-local-date-time-6448855d327a?source=collection_archive---------1----------------------->

![](img/01fe539f93a5e3af2ef210f7ee91f779.png)

照片由 [Lon Christensen](https://unsplash.com/@lonwake?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有时，我们可能希望在 JavaScript 应用程序中将 UTC 日期时间转换为本地日期时间。

在本文中，我们将研究如何将 UTC 日期时间转换为本地日期时间。

# 在日期字符串后添加 UTC，并将该字符串传递给日期构造函数

将 UTC 日期时间转换为本地日期时间的一种方法是在日期时间字符串中添加`'UTC'`后缀。

然后我们将它传递给`Date`构造函数。

例如，我们可以写:

```
const date = new Date('6/29/2021 4:52:48 PM UTC');
console.log(date.toString())
```

如果我们在太平洋时区，我们会得到:

```
Tue Jun 29 2021 09:52:48 GMT-0700 (Pacific Daylight Time)
```

记录在案。

正如我们所看到的，转换是自动进行的。

# 在日期字符串后添加 Z，并将该字符串传递给日期构造函数

`Z`后缀与`UTC`相同，用于设置 UTC 的日期时间。

我们可以将此后缀附加到 ISO-8601 日期时间，因为它是标准的一部分。

例如，我们可以写:

```
const date = new Date('2021-06-29T16:52:48.000Z');
console.log(date.toString())
```

然后我们得到:

```
Tue Jun 29 2021 09:52:48 GMT-0700 (Pacific Daylight Time)
```

因此如果我们处于太平洋时间。

# 调用`getTimezoneOffset`方法将日期时间转换为本地日期时间

我们可以在 JavaScript 日期偏移量上调用`getTimezoneOffset`来获得时差 UTC 和当地时区的小时数。

例如，我们可以写:

```
const date = new Date('2021-06-29T16:52:48.000');
const newDate = new Date(date.getTime() - date.getTimezoneOffset() * 60 * 1000);
console.log(newDate)
```

我们调用`getTime`来获取我们想要转换的`date`的时间戳。

然后，我们添加时区偏移量(以毫秒为单位),我们用以下内容创建:

```
date.getTimezoneOffset() * 60 * 1000
```

然后我们得到时区偏移量并除以 60，得到以小时为单位的偏移量。

然后我们得到:

```
Tue Jun 29 2021 02:52:48 GMT-0700 (Pacific Daylight Time)
```

结果。

# 结论

我们可以用各种日期方法将 UTC 日期时间转换成本地日期时间。