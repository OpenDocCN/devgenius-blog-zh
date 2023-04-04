# PHP — P97:日期和时间简介

> 原文：<https://blog.devgenius.io/php-p97-date-and-time-introduction-8174c21a447e?source=collection_archive---------15----------------------->

![](img/78631f0018a918d5ec36327319059c29.png)

这是另一个让人们感到害怕的事情:日期和时间。不如我们也去神秘化这个话题吧。让我们先看看传统的做法，然后在接下来的几篇文章中看看碳的做法。在下一篇文章中，我们还将研究使用`DateTime`对象的时间加法和减法。

最简单的方法就是用例子。我们将处理以下日期和时间函数:

*   `getdate()`
*   `localtime()`
*   `date()`
*   `strtotime()`
*   `mktime()`
*   `checkdate()`

# 获取日期()

getdate()函数返回一个数组，其中包含以下本地时间值。

```
<?php
var_dump(getdate());
```

```
/app/94 DateTime/index.php:3:
array (size=11)
  'seconds' => int 18
  'minutes' => int 21
  'hours' => int 21
  'mday' => int 19
  'wday' => int 6
  'mon' => int 11
  'year' => int 2022
  'yday' => int 322
  'weekday' => string 'Saturday' (length=8)
  'month' => string 'November' (length=8)
  0 => int 1668892878
```

数组键代表:

`seconds`、`minutes`和`hours`分别代表秒、分和小时的数字表示

`mday`表示一个月中的第几天(从 1 到 31)

`wday`用数字表示一周中的某一天，其中`0`是星期天，`6`是星期六

`mon`是月份的数字表示

`year`是 4 位数的年份

`yday`是当前日期的数字表示，从`0`到`365`

`weekday`是星期几的文本表示，即`Saturday`

`month`是月份的文本表示，即`October`

`0`是自 Unix 纪元以来的秒数

您可以将`getdate()`结果存储在一个变量中，然后调用您需要的值。

```
<?php

$current_date_time = getdate();
echo $current_date_time['weekday'];
```

```
Saturday
```

# 本地时间()

`localtime()`返回一个与 C 函数调用返回的结构相同的数组。

*   “TM _ sec”——表示从`0`到`59`的秒数
*   “TM _ min”——表示从`0`到`59`的分钟数
*   “TM _ hour”——表示从`0`到`23`的小时数
*   “TM _ mday”——表示一个月中从`1`到`31`的某一天
*   “TM _ mon”——表示一年中的月份，从`0`(一月)到`11`(十二月)
*   “TM _ year”—表示自 1900 年以来的年份
*   “TM _ wday”——代表一周中的某一天，`0`(周日)到`6`(周六)
*   “TM _ yday”——代表一年中的某一天，从`0`到`365`
*   “TM _ isdst”——如果夏令时有效，则为正值，`0`如果不有效，则为负值。

不幸的是，这些键没有显示出来。所以你需要知道索引值代表什么。

```
<?php
var_dump(localtime());
```

```
Saturday
/app/94 DateTime/index.php:6:
array (size=9)
  0 => int 25  //tm_sec
  1 => int 31  //tm_min
  2 => int 21  //tm_hour
  3 => int 19  //tm_mday
  4 => int 10  //tm_mon
  5 => int 122 //tm_year
  6 => int 6   //tm_wday
  7 => int 322 //tm_yday
  8 => int 0   //tm_isdst
```

要获得当前年份，您需要将`1900`添加到年份中。

```
<?php
$localtime = localtime();

$current_year = $localtime[5] + 1900;
echo $current_year;
```

```
2022
```

# 日期()

这是最常用的功能。大多数时候，这是您获取大部分数据的地方。`date()`函数需要一个格式化字符串作为参数，接受哪些值？

`date()`参数字符串的可接受值。

`d`:表示一个月中的第几天，从 01 到 31(包括前导零)

`D`:从`Mon`到`Sun`用三个字母的文本格式表示一天

`j`:表示一个月中的第几天，2 位数，从 1 到 31(无前导零)

`l`(小写‘L’)从`Sunday`到`Saturday`的一周中某一天的完整文本表示

`N`一周中某一天的 ISO 8601 数字表示法`1`(周一)到`7`(周日)

`S`英文序数后缀为月中的第几天，2 个字符`st`、`nd`、`rd`或`th`。与`j`配合良好

`w`星期几的数字表示`0`(周日)到`6`(周六)

`z`一年中的某一天(从 0 开始)`0`到`365`

*周*—

`W` ISO 8601 一年中的周数，从星期一开始的周数示例:`42`(一年中的第 42 周)

*月*—

`F`一个月的完整文本表示，比如一月或三月`January`到`December`

`m`月份的数字表示，带前导零`01`到`12`

`M`一个月的简短文本表示，三个字母`Jan`到`Dec`

`n`月份的数字表示，无前导零`1`至`12`

`t`从`28`到`31`给定月份的天数

*年份*—

`L`是否闰年`1`如果是闰年，`0`否则。

`o` ISO 8601 周历年。其值与`Y`相同，但如果 ISO 周数(`W`)属于上一年或下一年，则使用该年。例子:`1999`或`2003`

`X`年份的扩展全数字表示，至少 4 位数，其中`-`表示公元前年，`+`表示公元年。例子:`-0055`、`+0787`、`+1999`、`+10191`

`x`如果需要，扩展的全数字表示，或者如果可能，标准的全数字表示(如`Y`)。至少四位数。公元前年以`-`为前缀。`10000`之后(含)的年份以`+`为前缀。例子:`-0055`、`0787`、`1999`、`+10191`

`Y`一年的完整数字表示，至少 4 位数，用`-`表示公元前年。例子:`-0055`、`0787`、`1999`、`2003`、`10191`

用两位数表示的年份示例:`99`或`03`

*时间*—

`a`小写的午前和午后`am`或`pm`

`A`大写的午前和午后`AM`或`PM`

`B`斯沃琪互联网时间`000`至`999`

`g`一小时的 12 小时格式，无前导零`1`至`12`

`G`无前导零的 24 小时制`0`到`23h`带前导零的 12 小时制`01`到`12`

`H`带前导零的 24 小时制格式`00`到`23`

`i`分钟，前导零`00`至`59`

`s`带前导零的秒`00`到`59`

`u`微秒。请注意， [date()](https://www.php.net/manual/en/function.date.php) 将始终生成`000000`，因为它采用一个 int 参数，而如果 [DateTime](https://www.php.net/manual/en/class.datetime.php) 是用微秒创建的，DateTime::format()则支持微秒。示例:`654321`

`v`毫秒。相同的注释适用于`u`。示例:`654`

*时区* -

`e`时区标识符示例:`UTC`、`GMT`、`Atlantic/Azores`

`I`(大写 I)日期是否是夏令时`1`如果是夏令时，否则`0`。`O`小时和分钟之间不含冒号的格林威治时间(GMT)差异示例:`+0200`

`P`与格林威治时间(GMT)的差异，小时和分钟之间用冒号表示例如:`+02:00`

`p`与`P`相同，但返回`Z`而不是`+00:00`(从 PHP 8.0.0 开始可用)示例:`Z`或`+02:00`

`T`时区缩写，如果已知的话；否则 GMT 偏移。例子:`EST`、`MDT`、`+05`

`Z`时区偏移量，以秒为单位。UTC 以西时区的偏移量始终为负，而 UTC 以东时区的偏移量始终为正。`-43200`至`50400`

*完整日期/时间* -

ISO 8601 日期 2004-02-12T15:19:21+00:00

`r`[RFC 2822](http://www.faqs.org/rfcs/rfc2822)/[RFC 5322](http://www.faqs.org/rfcs/rfc5322)格式化日期示例:`Thu, 21 Dec 2000 16:01:07 +0200`

`U`自 Unix 纪元以来的秒数(1970 年 1 月 1 日 00:00:00 GMT)另请参见 [time()](https://www.php.net/manual/en/function.time.php)

要以下面的格式打印出当前日期，`November 19, 2022`，您需要使用格式化程序字符串:`F d, Y`。如果您需要的是`11/19/22`，您需要像这样格式化您的格式化程序字符串:`m/d/y`。

```
<?php
echo date("F d, Y"); // November 19, 2022
echo "<br>";

echo date("m/d/y"); // 11/19/22 
echo "<br>";
```

你可以发挥你的想象力，用你喜欢的格式做任何事情。

# strtotime()

这是一个有趣的函数，因为它将人类可读的日期字符串转换成 Unix 时间戳。`date()`函数有第二个参数。这是 unix 时间戳。如果您没有在那里放置任何东西，它将使用`time()`函数获取当前的 unix 时间戳作为缺省值。但是，您可以放置另一个值并格式化它。

假设你有下面这个字符串:`October 10 2022 5:00am`。当我们试图将它转换成 unix 时间戳时，你认为这可行吗？

```
$human_readable_datetime = "October 10 2022 5:00am";
$unix_timestamp = strtotime($human_readable_datetime);
var_dump($unix_timestamp);
```

```
/app/94 DateTime/index.php:21:int 1665374400
```

我们得到了一个数字时间戳，但是它是正确的吗？让我们检查一下日期时间格式。

```
<?php
$human_readable_datetime = "October 10 2022 5:00am";
$unix_timestamp = strtotime($human_readable_datetime);

echo date("F d, Y | g:ia", $unix_timestamp);
```

```
October 10, 2022 | 5:00am
```

我们得到相同的信息，但是由于我们的格式化器字符串:`F d, Y | g:ia`，所以格式不同。

相当酷。然后，您可以要求用户提交日期和时间，而不要求特定的格式。`strtotime()`函数将把它转换成 unix，然后你可以用`date()`函数格式化它。

# mktime()

如果您想更具体地了解如何创建 unix 时间戳，可以使用`mktime()`函数来代替`strtotime()`函数。这包含一个按特定顺序排列的参数列表。

```
mktime(
    int $hour,
    ?int $minute = null,
    ?int $second = null,
    ?int $month = null,
    ?int $day = null,
    ?int $year = null
): int|false
```

这些都是具有共同约束的数值。例如，`second`必须在`0`和`59`之间。`year`可以是 2 位数或 4 位数。年度的当前有效范围在`1901`和`2038`之间。

如果我们想创建`October 10 2022 5:00am`，我们需要做以下事情:

```
<?php
$made_time = mktime(5, 00, 00, 10, 10, 2022);
echo date("F d, Y | g:ia", $made_time);
echo "<br>";
```

```
October 10, 2022 | 5:00am
```

# 检查日期()

我们要介绍的最后一个函数是`checkdate()`函数。`checkdate()`函数接受 3 个参数:`month`、`day`和`year`。如果日期正确，则返回`true`，否则返回`false`。

```
<?php
var_dump( checkdate(11, 20, 2023) );
var_dump( checkdate(13, 20, 2023) );
var_dump( checkdate(02, 29, 2023) ); // great way to check for leap-year
var_dump( checkdate(02, 29, 2024) ); // great way to check for leap-year
```

```
/app/94 DateTime/index.php:28:boolean true
/app/94 DateTime/index.php:29:boolean false
/app/94 DateTime/index.php:30:boolean false
/app/94 DateTime/index.php:31:boolean true
```

如你所见，2023 年不是闰年，但 2024 年是。我们在下一篇文章中查看`DateTime`对象时再见。

![](img/9485d5d35191c06a3d815d6a875bc20d.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。