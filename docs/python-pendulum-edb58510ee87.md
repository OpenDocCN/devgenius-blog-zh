# Python 钟摆

> 原文：<https://blog.devgenius.io/python-pendulum-edb58510ee87?source=collection_archive---------2----------------------->

## Python 中一个新的日期时间模块

![](img/0942a454bdfc05fb356938fca8b9df08.png)

关于数据处理，Python 提供了很多库，比如标准库`datetime`、第三方库`dateutil`、`arrow`等等。

在这篇文章中，我想介绍一下我个人最喜欢的库`pendulum`，它使用起来非常方便，可以满足对日期的任何操作。

# 为什么是钟摆？

原生的`datetime`实例对于基本的情况来说已经足够了，但是当你面对更复杂的用例时，它们通常会显示出局限性，并且不那么直观。`Pendulum`提供了一个更干净、更易于使用的 API，同时仍然依赖于标准库。所以还是`datetime`但是更好。

> 与 Python 的其他`datetime`库不同，Pendulum 是标准`datetime`类的替代物(它继承了它)，所以，基本上，你可以用代码中的`DateTime`实例替换所有的`datetime`实例(使用`type`函数检查对象类型的库存在例外，例如`sqlite3`或`PyMySQL`)。

它还去除了简单的`datetimes`的概念:每个`Pendulum`实例都是时区敏感的，并且默认在`UTC`中，以便于使用。

`Pendulum`还通过提供更直观的方法和属性改进了标准的`timedelta`类。

# 装置

使用前需要安装，直接 pip 安装钟摆即可。

```
$ pip install pendulum 
Looking in indexes: [https://pypi.python.org/simple](https://pypi.python.org/simple)
Collecting pendulum
  Downloading pendulum-2.1.2.tar.gz (81 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 81.2/81.2 kB 4.1 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
...
Successfully installed pendulum-2.1.2 pytzdata-2020.1
```

# 基本用法

我们来看看用法，首先是 datetime，date，time 的创建。

```
import pendulum

dt = pendulum.datetime(
    2022, 7, 21, 20, 10, 30)
print(dt.__class__)
print(dt)
"""
<class 'pendulum.datetime.DateTime'>
2022-07-21T20:10:30+00:00
"""*# Change timezone*
dt = pendulum.datetime(
    2022, 7, 21, 20, 10, 30, tz="xxxx/xxxx")
print(dt)
"""
2022-07-21T20:10:30+08:00
"""

*# Create date*
d = pendulum.date(2022, 7, 21)
print(d.__class__)
print(d)
"""
<class 'pendulum.date.Date'>
2022-07-21
"""

*# Create time*
t = pendulum.time(20, 10, 30)
print(t.__class__)
print(t)
"""
<class 'pendulum.time.Time'>
20:10:30
"""
```

如果创建了`datetime`，时区默认为 UTC。如果不希望时区，或者希望时区是当地时区，那么`pendulum`也具体提供了两种方法。

```
import pendulum

*# Create datetime use local timezone*
dt = pendulum.local(
    2022, 7, 21, 20, 10, 30)
print(dt)
"""
2022-07-21T20:10:30+08:00
"""
print(pendulum.local_timezone())
"""
Timezone('xxxx/xxxx')
"""

*# Create datetime with no timezon
# tz is None*
dt = pendulum.naive(2022, 7, 21, 20, 10, 30)
print(dt)
"""
2022-07-21T20:10:30
"""
```

然后`pendulum`还提供了几种方法，比如创建当前日期时间，日期等等。

```
import pendulum

dt = pendulum.today()
print(dt)
"""
2022-07-21T00:00:00+08:00
"""

*# Get tomorrow's date*
dt = pendulum.tomorrow()
print(dt)
"""
2022-07-22T00:00:00+08:00
"""

*# Get yesterday's date*
dt = pendulum.yesterday()
print(dt)
"""
2022-07-20T00:00:00+08:00
"""
```

我们还可以从时间戳或字符串创建:

```
import pendulum

*# Based on given timestamp*
dt1 = pendulum.from_timestamp(1653828466)
dt2 = pendulum.from_timestamp(1653828466,
                              tz=pendulum.local_timezone())
print(dt1)
print(dt2)
"""
2022-05-29T12:47:46+00:00
2022-05-29T20:47:46+08:00
"""

*# Based on time string*
dt1 = pendulum.parse("2020-05-03 12:11:33")
dt2 = pendulum.parse("2020-05-03 12:11:33",
                     tz=pendulum.local_timezone())
print(dt1)
print(dt2)
"""
2020-05-03T12:11:33+00:00
2020-05-03T12:11:33+08:00
"""
```

# **日期时间相关操作**

还有很多操作，我们就一一介绍了。

```
import pendulum

dt = pendulum.local(
    2022, 3, 28, 20, 10, 30)

*# Get date and time*
print(dt.date())
print(dt.time())
"""
2022-07-21
20:10:30
"""

*# Replace and return new datetime*
print(dt.replace(year=9999))
"""
9999-03-28T20:10:30+08:00
"""

*# Convert to timestamp*
print(dt.timestamp())
"""
1648469430.0
"""

*# Return year, month, day...*
print(dt.year, dt.month, dt.day)
print(dt.hour, dt.minute, dt.second)
print(dt.tz)
"""
2022 3 28
20 10 30
Timezone('xxx/xxxx')
"""
```

然后生成字符串，`pendulum.DateTime`对象可以转换成各种格式的日期字符串。

```
import pendulum

dt = pendulum.local(
    2022, 3, 28, 20, 10, 30)

*# Most frequent uses*
print("datetime:", dt.to_datetime_string())
print("date:", dt.to_date_string())
print("time:", dt.to_time_string())
print("iso8601:", dt.to_iso8601_string())
"""
datetime: 2022-03-28 20:10:30
date: 2022-03-28
time: 20:10:30
iso8601: 2022-03-28T20:10:30+08:00
"""print("atom:", dt.to_atom_string())
print("rss:", dt.to_rss_string())
print("w3c:", dt.to_w3c_string())
print("cookie:", dt.to_cookie_string())
print("rfc822:", dt.to_rfc822_string())
"""
atom: 2022-03-28T20:10:30+08:00
rss: Mon, 28 Mar 2022 20:10:30 +0800
w3c: 2022-03-28T20:10:30+08:00
rfc822: Mon, 28 Mar 22 20:10:30 +0800
"""
```

有时候我们还需要判断当前日期是星期几，当前年份是星期几等等，`pendulum`也为我们打包了。

```
import pendulum

dt = pendulum.local(
    2022, 3, 28, 20, 10, 30)

*# Return days of week*
print(dt.isoweekday())  *# 1*

*# Return days of year*
print(dt.day_of_year)  *# 87*

*# Return days of month*
print(dt.days_in_month)  *# 31*

*# Return week of month*
print(dt.week_of_month)  *# 5*

*# Return week of year*
print(dt.week_of_year)  *# 13*

*# Check leap year*
print(dt.is_leap_year())  *# False*
```

最后是日期的操作，这是`pendulum`最厉害的部分。

```
import pendulum

dt = pendulum.local(
    2022, 7, 21, 20, 10, 30)

*# Return next month today*
print(dt.add(months=1))
"""
2022-08-21T20:10:30+08:00
"""

*# Return last month today*
print(dt.add(months=-1))
"""
2022-06-21T20:10:30+08:00
""" 
```

像 Python 的内置模块`datetime`，在添加日期的时候，最多支持天，我们无法计算下一周、下一个月、下一年的日期。`pendulum`很好操控，这是我最喜欢的一点。

当然，add 中的值是正的，相当于日期倒退；值为负数，相当于将日期向前推。

那么也可以减去两个日期:

```
import pendulum

dt1 = pendulum.local(
    2021, 1, 20, 11, 22, 33)

dt2 = pendulum.local(
    2022, 3, 30, 20, 10, 30)

period = dt2 - dt1
*# Return Period object*
*# Similar to timedelta in datetime*
print(period.__class__)
"""
<class 'pendulum.period.Period'>
"""

print(period.in_years())  *# 1*

print(period.in_months())  *# 14*

print(period.in_weeks())  *# 62*

print(period.in_days())  *# 434*

print(period.in_hours())  *# 10424*

print(period.in_minutes())  *# 625487*

print(period.in_seconds())  *# 37529277*
```

功能非常强大。Python 的`datetime`模块中的`timedelta`最多只能算出两个日期相差多少天，这里可以用年、月、日、时、分、秒。

当然`pendulum`的功能不仅仅是我们上面说的那些。有兴趣可以参考[官网](https://github.com/sdispater/pendulum)，不过这些都是常用的东西。