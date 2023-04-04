# 如何在 PHP 中找到两个日期之间的天数

> 原文：<https://blog.devgenius.io/how-to-find-the-number-of-days-between-two-dates-in-php-1404748b1e84?source=collection_archive---------0----------------------->

## 获取两个日期之间的天数

![](img/4969249f8eb6bbe62db32fe27c99cf36.png)

[埃斯特扬森斯](https://unsplash.com/@esteejanssens?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在这篇博客中，我们将探索用 PHP 获取两个日期之间的天数的不同方法。

## 方法 1-使用 DateTime::diff

在这个方法中，我们将创建两个[日期时间](https://www.php.net/manual/en/class.datetimeinterface.php)对象。并通过使用 [diff](https://www.php.net/manual/en/datetime.diff.php) 函数找出差异。

DateTime::diff 返回两个 DateTime 对象之间的差异

```
<?php
  $origin = new DateTime('2020-11-13');
  $target = new DateTime('2020-11-25');
  $interval = $origin->diff($target);
  echo $interval->format('%R%a days'); // Output: +12 days
?><?php
  $origin = new DateTime('now');
  $target = new DateTime('+4days');
  $interval = $origin->diff($target);
  echo $interval->format('%R%a days'); // Output: +4 days
?>
```

## 方法 2-使用 date_diff()函数

我们将使用 PHP [Date/Time](https://www.php.net/manual/en/ref.datetime.php) 的内置 [date_diff](https://www.php.net/manual/en/function.date-diff.php) 函数来找出两个日期之间的差异。这是第一种方法的过程风格。

函数的作用是:返回两个日期时间对象之间的差值。 [date_create](https://www.php.net/manual/en/function.date-create) 函数用于创建日期时间对象。

下面的例子将返回两个日期之间的天数。

```
<?php
  $origin = date_create('2020-11-13');
  $target = date_create('2020-11-25');
  $interval = date_diff($origin, $target);
  echo $interval->format('%R%a days');
?>// Output: +12 days
```

在上面的例子中，我们使用了带有破折号日期格式的年、月和日。更多有效格式请参考 PHP [日期格式](https://www.php.net/manual/en/datetime.formats.date.php)。

## 方法 3 —使用`strtotime()`功能

当使用夏令时区时，建议使用前两种方法来获得两个日期之间的准确差异。

`[strtotime()](https://www.php.net/manual/en/function.strtotime)`将把英文文本日期时间描述解析成一个 Unix 时间戳(自 1970 年 1 月 1 日 00:00:00 GMT 以来的秒数)。

```
<?php

  $days = (strtotime('2020-11-25') - strtotime('2020-11-13')) / (60 * 60 * 24); 
  echo floor($days); // Output: 12

  $days = (strtotime('+4days') - strtotime('now')) / (60 * 60 * 24); 
  echo floor($days); // Output: 4?>
```

60 * 60 * 24 是 1 天 86400 秒。

## 为什么不能用 strtotime()来求日期差？

![](img/5fd29d9f07c6ca7c9cced0e79eb92793.png)

[杰森·斯特鲁尔](https://unsplash.com/ja/@jasonstrull?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

在夏令时，时间会增加或减少。所以`strtotime` diff 的结果会返回错误的值。

在下面的例子中，我们将比较不同时区的`strtotime`和 DateTime 的结果。

我们已经知道`strtotime`将在没有时区或非夏令时区返回正确的值。

```
<?php
  // without timezone
  $days  = (strtotime('2020-03-15') - strtotime('2020-03-08')) / (60 * 60 * 24);
  echo floor($days); // Output: -7
  echo "<br>";

  $origin = new DateTime('2020-03-15');
  $target = new DateTime('2020-03-08');
  $interval = $origin->diff($target);
  echo $interval->format('%R%a days'); // Output: -7 days?><?php
  // With timezone
  $timezone = 'Asia/Kolkata';
  date_default_timezone_set($timezone);
  $days  = (strtotime('2020-03-15') - strtotime('2020-03-08')) / (60 * 60 * 24);
  echo floor($days); // Output: -7
  echo "<br>";

  $origin = new DateTime('2020-03-15');
  $target = new DateTime('2020-03-08');
  $interval = $origin->diff($target);
  echo $interval->format('%R%a days'); // Output: -7 days?>
```

“亚洲/加尔各答”没有日光节约时间。所以结果是一样的。

下一个例子，我们将使用“美国/纽约”时区测试相同的日期。

```
<?php
  $timezone = 'America/New_York';
  date_default_timezone_set($timezone);
  $days  = (strtotime('2020-03-15') - strtotime('2020-03-08')) / (60 * 60 * 24);
  echo floor($days); // Output: -6
  echo "<br>";

  $origin = new DateTime('2020-03-15');
  $target = new DateTime('2020-03-08');
  $interval = $origin->diff($target);
  echo $interval->format('%R%a days'); // Output: -7 days?>
```

夏令时从 2020 年 3 月 8 日周日开始。所以，你得到了 2020 年 3 月 8 日到 2020 年 3 月 15 日之间“美国/纽约”时区的错误天数。

## 结论

避免使用`strtotime`来寻找两个日期之间的差异。所以总是使用`DateTime`对象，你有一个更复杂的场景意味着使用 PHP [Carbon](https://carbon.nesbot.com/) 库。

感谢您的阅读。

敬请关注更多内容！

*跟我来*[***balajidharma.medium.com***](https://balajidharma.medium.com/)。