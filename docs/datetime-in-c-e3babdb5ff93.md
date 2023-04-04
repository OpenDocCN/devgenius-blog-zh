# C#中的日期时间

> 原文：<https://blog.devgenius.io/datetime-in-c-e3babdb5ff93?source=collection_archive---------5----------------------->

![](img/37b3047bfd008432d28439681ec35255.png)

在中处理日期和时间。我们需要使用日期时间结构。它表示从 0001 年 1 月 1 日 00:00:00 到 9999 年 12 月 31 日 23:59:59 的日期和时间。

还可以使用构造函数创建新的 DateTime 对象。空构造函数创建一个初始日期:

`1 DateTime dateTime = new` `DateTime();`

`2 Console.WriteLine(dateTime); // 01.01.0001`

也就是说，我们将获得最小可能值，也可以如下获得:

`1 Console.WriteLine(DateTime.MinValue);`

若要设置特定日期，必须使用接受参数的构造函数之一:

`1 DateTime date1 = new` `DateTime(2022, 08, 28); // year - month - day`

`2 Console.WriteLine(date1); // 28.08.2022`

设置时间:

`1 DateTime date1 = new` `DateTime(2022, 8, 28, 18, 30, 25); // year- month - day - hour - minute - second`

`2 Console.WriteLine(date1); // 28.08.2022 15:32:49`

如果要获取当前时间和日期，可以使用许多 DateTime 属性:

`1 Console.WriteLine(DateTime.Now);`

`2 Console.WriteLine(DateTime.UtcNow);`

`3 Console.WriteLine(DateTime.Today);`

控制台输出:

```
28.08.2022 11:43:33
28.08.2022 8:43:33
28.08.2022 0:00:00
```

日期时间。属性获取计算机的当前日期和时间，即 DateTime。UtcNow —相对于格林威治标准时间(GMT)和日期时间的日期和时间。今天—仅当前日期。

处理日期时，请记住默认情况下使用公历来表示日期。但是如果我们想得到 1582 年 10 月 5 日是星期几，会发生什么呢:

`1 DateTime someDate = new` `DateTime(1582, 10, 5);`

`2 Console.WriteLine(someDate.DayOfWeek)`

控制台将突出显示星期二的值。然而，从历史上可以知道，从儒略历到公历的第一次转变发生在 1582 年 10 月。然后，在 10 月 4 日(星期四)(根据儒略历)的日期之后，立即切换到 10 月 15 日(星期五)(根据公历)。所以，事实上，10 天被放弃了。也就是 10 月 4 日之后是 10 月 15 日。

在大多数情况下，这一事实不太可能影响计算，但是当处理非常旧的日期时，应该考虑这一方面。

**日期时间操作**

DateTime 结构的主要操作与添加或减去日期有关。例如，您需要给一个日期加上或减去几天。

添加日期有多种方法:

Add(TimeSpan 值):向日期添加一个 TimeSpan 值。

AddDays(double value):将当前日期加上几天

AddHours(double value):为当前日期增加几个小时

AddMinutes(double value):为当前日期增加几分钟

AddMonths(int value):向当前日期添加几个月

AddYears(int value):向当前日期添加一些年份

例如，向某个日期添加 3 小时:

`1 DateTime date1 = new` `DateTime(2022, 8, 28, 18, 30, 25); // 28.08.2022 18:30:25`

`2 Console.WriteLine(date1.AddHours(3)); // 28.08.2022 21:30:25`

**减法(日期时间日期)方法**用于减去日期:

`1 DateTime date1 = new` `DateTime(2022, 8, 28, 18, 30, 25); // 28.08.2022 18:30:25`

`2 DateTime date2 = new` `DateTime(2022, 8, 28, 15, 30, 25); // 28.08.2022 15:30:25`

`3 Console.WriteLine(date1.Subtract(date2)); // 03:00:00`

这里的日期相差三个小时，所以结果是“03:00:00”。

减法方法没有分别减去天数、小时数等选项。但这并不需要，因为我们可以将负值传递给 AddDays()方法和其他 add 方法:

`1 // subtract three hours`

`2 DateTime date1 = new` `DateTime(2022, 8, 28, 18, 30, 25); // 28.08.2022 18:30:25`

`3 Console.WriteLine(date1.AddHours(-3)); // 28.08.2022 15:30:25`

除了加法和减法运算，还有许多日期格式化方法:

`1 DateTime date1 = new`

`2 Console.WriteLine(date1.ToLocalTime()); // 28.08.2022 21:30:25`

`3 Console.WriteLine(date1.ToUniversalTime()); // 28.08.2022 15:30:25`

`4 Console.WriteLine(date1.ToLongDateString()); // August 28th 2022`

`5 Console.WriteLine(date1.ToShortDateString()); // 28.08.2022`

`6 Console.WriteLine(date1.ToLongTimeString()); // 18:30:25`

`7 Console.WriteLine(date1.ToShortTimeString()); // 18:30`