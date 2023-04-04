# 停止在 PHP 中使用日期时间

> 原文：<https://blog.devgenius.io/stop-using-datetime-in-php-66df3d731875?source=collection_archive---------0----------------------->

## PHP 中为什么需要使用不可变的 DateTime？

![](img/46ddf5208a74a2e46a4b851f5a588fed.png)

Julia Taubitz 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在 PHP 中，我们通常在大多数日期操作中使用 DateTime 函数。PHP 日期时间是可变的日期。可变日期可能是混乱的来源，它们会在您的代码中产生意想不到的错误。在这篇博客中，我们讨论可变日期问题和可能的解决方案。

## 日期时间问题

在下面的例子中，我们将使用 DateTime 对象显示今天和明天的日期。

```
<?php
  $today_date = new DateTime();
  $tomorrow_date = $today_date->modify('+1 day');
  echo "Today: " . $today_date->format('Y-m-d');
  echo "<br>";
  echo "Tomorrow: " . $tomorrow_date->format('Y-m-d');
?>
```

如果你执行上面的代码，你将会得到今天和明天相同的日期。因为`modify`返回 DateTime 的更新实例。由于可变性，它会将$today_date 对象更改为$tomorrow_date。所以 echo 语句返回相同的日期。

[](https://balajidharma.medium.com/the-syntax-highlighting-highlight-js-is-now-available-on-medium-58f672595691) [## 用语法荧光笔尝试代码块的中等新功能

### 介质上的语法高亮显示

balajidharma.medium.com](https://balajidharma.medium.com/the-syntax-highlighting-highlight-js-is-now-available-on-medium-58f672595691) 

可能是日期时间上的`modify`函数的问题。让我们试试其他日期时间函数。下一个例子我们尝试`DateTime->add()`方法。

```
<?php
  $today_date = new DateTime();
  $tomorrow_date = $today_date->add(new DateInterval('P1D'));
  echo "Today: " . $today_date->format('Y-m-d')."\n";
  echo "Tomorrow: " . $tomorrow_date->format('Y-m-d');
?>
```

上面的示例也返回相同的日期。因此，如果使用可变的 DateTime，您将获得相同的实例。

[](/how-to-find-the-number-of-days-between-two-dates-in-php-1404748b1e84) [## 如何在 PHP 中找到两个日期之间的天数

### 获取两个日期之间的天数

blog.devgenius.io](/how-to-find-the-number-of-days-between-two-dates-in-php-1404748b1e84) 

## 如何解决这个问题

![](img/b3444ece3d6eb0be858642c67e156256.png)

詹姆斯·柯文在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

一个简单的修复方法是将$today_date echo 语句移到`modify()`、`add()`方法之前。

```
<?php
  $today_date = new DateTime();
  echo "Today: " . $today_date->format('Y-m-d');

  $tomorrow_date = $today_date->modify('+1 day')."\n";
  echo "Tomorrow: " . $tomorrow_date->format('Y-m-d');
?>
```

但是我们始终不能使用上面的修复方法。因为有时我们需要给视图赋值。这两个时间将具有相同的日期。

## PHP 对象克隆

我们可以通过使用 PHP [对象克隆](https://www.php.net/manual/en/language.oop5.cloning.php)解决这个问题。现在尝试使用 clone 关键字来克隆 DateTime 对象。然后修改或添加您的更改。

```
<?php
  $today_date = new DateTime();
  $tomorrow_date = (clone $today_date)->modify('+1 day');
  echo "Today: " . $today_date->format('Y-m-d')."\n";
  echo "Tomorrow: " . $tomorrow_date->format('Y-m-d');
?>
```

对象克隆会使您的代码更加复杂，并引入不必要的噪声。也很难一直呼叫克隆人。这个问题不仅仅针对`modify()`和`add()`功能。您将在下面的`DateTime`函数中面临同样的问题。

*   `sub`
*   `setDate`
*   `setISODate`
*   `setTime`
*   `setTimezone`
*   `modify`
*   `add`

## **日期时间不可变**

`DateTimeImmutable`类的行为与 [DateTime](https://www.php.net/manual/en/class.datetime.php) 相同，只是在调用 [DateTime::modify()](https://www.php.net/manual/en/datetime.modify.php) 等修改方法时会返回新的对象。PHP 5.5 引入了 DateTimeImmutable 类。

> 每次修改对象时，不可变对象都会创建对象的副本

DateTime 和 DateTimeImmutable 从`[DateTimeInterface](https://www.php.net/manual/en/class.datetimeinterface.php)`接口实现。

让我们试试`DateTimeImmutable`我们的例子。

```
<?php
  $today_date = new DateTimeImmutable();
  $tomorrow_date = $today_date->modify('+1 day');
  echo "Today: " . $today_date->format('Y-m-d')."\n";
  echo "Tomorrow: " . $tomorrow_date->format('Y-m-d');
?>
```

上面的代码将打印正确的今天和明天的日期。代替克隆，这个 DateTimeImmutable 实例是我们问题的简单而完美的解决方案。

add()函数的另一个例子

```
<?php
  $today_date = new DateTimeImmutable();
  $tomorrow_date = $today_date->add(new DateInterval('P1D'));
  echo "Today: " . $today_date->format('Y-m-d')."\n";
  echo "Tomorrow: " . $tomorrow_date->format('Y-m-d');
?>
```

## 日期时间库

PHP [Carbon](https://carbon.nesbot.com/) 是最好的日期时间库之一。碳也提供了`CarbonImmutable`物体。所以如果你使用 carbon 库，就使用不可变对象。

```
<?php
  $mutable = Carbon::now();
  $modifiedMutable = $mutable->add(1, 'day');

  echo $mutable->isoFormat('Y-M-D')."\n";
  echo $modifiedMutable->isoFormat('Y m D')."\n";

  $immutable = CarbonImmutable::now();
  $modifiedImmutable = CarbonImmutable::now()->add(1, 'day');

  echo $immutable->isoFormat('Y-M-D')."\n";
  echo $modifiedImmutable->isoFormat('Y m D')."\n";
?>
```

## 结论

因此，考虑使用不可变的日期时间对象来避免可变日期时间的某些类型的错误。如果任何特定需求使用可变对象，否则使用`DateTimeImmutable`

感谢您的阅读。

敬请关注更多内容！

*跟我来*[【balajidharma.medium.com】](https://balajidharma.medium.com/)*。*