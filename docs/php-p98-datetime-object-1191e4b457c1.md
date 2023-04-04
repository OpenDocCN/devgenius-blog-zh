# PHP — P98:日期时间对象

> 原文：<https://blog.devgenius.io/php-p98-datetime-object-1191e4b457c1?source=collection_archive---------7----------------------->

![](img/d8e848d93a868723a505bfb31d624fc6.png)

在前一篇文章中，我们研究了 PHP 中一些最常用的日期和时间函数。我们没有涉及的是时间操纵。如何给当前日期时间加上 2 周或者两个日期之间有多少天？这就是 DateTime 对象的亮点。

[](/php-p97-date-and-time-introduction-8174c21a447e) [## PHP — P97:日期和时间简介

### 这是另一个人们觉得可怕的事情:日期时间。为什么我们不把这个话题也淡化一下。让我们…

blog.devgenius.io](/php-p97-date-and-time-introduction-8174c21a447e) 

# 日期时间对象

是 PHP 内置的一个类。这意味着您可以实例化它，而无需做任何特殊的事情。

```
<?php
$datetime = new DateTime();
var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:3:
object(DateTime)[1]
  public 'date' => string '2022-11-20 19:33:12.650004' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

我们得到的回答非常简单。它给了我们一个具有一些集合属性的对象，比如`date`、`timezone_type`和`timezone`。`date`是当前的日期和时间。

我注意到的第一件事是时区对我来说不正确。要改变它，您可以用`DateTimeZone`类创建一个新的时区，然后使用`DateTime`对象中的`setTimezone`方法来改变它。

您可以访问 PHP 文档以获得支持时区的完整列表。

[](https://www.php.net/manual/en/timezones.php) [## 支持的时区列表

### 在这里你可以找到 PHP 支持的时区的完整列表，这些时区是用来和例如…

www.php.net](https://www.php.net/manual/en/timezones.php) 

```
<?php
$datetime = new DateTime();

$timezone = new DateTimeZone("America/New_York");
$datetime->setTimezone($timezone);

var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:7:
object(DateTime)[1]
  public 'date' => string '2022-11-20 14:37:21.736311' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'America/New_York' (length=16)
```

这次，我得到了正确的`timezone`。

现在我们已经正确地配置了一切，如果我们想以特定的格式显示当前的日期和时间，我们可以。阅读我以前关于所有可用格式字符串的文章。

```
<?php
$datetime = new DateTime();

$timezone = new DateTimeZone("America/New_York");
$datetime->setTimezone($timezone);

echo $datetime->format("F d, Y");
```

```
November 11, 2022
```

它从日期字符串中提取当前日期，然后使用正确的格式对其进行格式化。如果我们想改变日期而不使用默认的日期呢？我们可以在创建它的时候在构造函数中传递它。

```
<?php
$datetime = new DateTime("11/5/2022 5:00 AM");

var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:6:
object(DateTime)[1]
  public 'date' => string '2022-11-05 05:00:00.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

我们也可以用`modify`方法修改`date`属性。它接受我们的标准格式化程序字符串。

```
<?php
$datetime = new DateTime("11/5/2022 5:00 AM");
var_dump($datetime);

$datetime->modify("11/10/2022 7:00 AM");
var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:5:
object(DateTime)[1]
  public 'date' => string '2022-11-05 05:00:00.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)

/app/95 DateTime Object/index.php:9:
object(DateTime)[1]
  public 'date' => string '2022-11-10 07:00:00.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

我们也可以使用`setDate`和`setTime`方法来修改日期和时间。

```
<?php
$datetime = new DateTime("11/5/2022 5:00 AM");
var_dump($datetime);

$datetime->setDate(2022, 11, 20);
var_dump($datetime);

$datetime->setTime(14, 20, 30);
var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:30:
object(DateTime)[1]
  public 'date' => string '2022-11-05 05:00:00.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)

/app/95 DateTime Object/index.php:32:
object(DateTime)[1]
  public 'date' => string '2022-11-20 05:00:00.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)

/app/95 DateTime Object/index.php:34:
object(DateTime)[1]
  public 'date' => string '2022-11-20 14:20:30.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

关于`setDate`、`setTime`甚至`setTimezone`方法有一件有趣的事情。它们每个都返回被实例化的`DateTime`对象。

我知道我们到现在还没有涉及到链接，但是它非常简单。当 then 方法返回它所属的对象时，这意味着您可以链接其他方法。

```
<?php
$datetime = new DateTime("11/5/2022 5:00 AM");
var_dump($datetime->setDate(2022, 11, 20));
```

你觉得这会是什么样子？理论上它应该只是我们实例化的`DateTime`对象，对吗？

```
/app/95 DateTime Object/index.php:40:
object(DateTime)[3]
  public 'date' => string '2022-11-20 14:20:30.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

没错。现在，让我们看看如何链接这些方法。

```
<?php
$datetime = new DateTime("11/5/2022 5:00 AM");
$datetime->setDate(2022, 11, 20)
    ->setTimezone(new DateTimeZone("America/New_York"))
    ->setTime(14, 20, 30);

var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:41:
object(DateTime)[3]
  public 'date' => string '2022-11-20 14:20:30.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'America/New_York' (length=16)
```

请记住，这些方法都返回一个对象。重要的是把这些方法按顺序排好。例如，让我们将`setTimezone`方法移到`setTime`方法之后。你认为我们会得到同样的结果吗？

```
<?php
$datetime = new DateTime("11/5/2022 5:00 AM");
$datetime->setDate(2022, 11, 20)
    ->setTime(14, 20, 30)
    ->setTimezone(new DateTimeZone("America/New_York"));
var_dump($datetime);
```

结果并不直观。我们没有得到我们刚刚公布的时间。

```
/app/95 DateTime Object/index.php:41:
object(DateTime)[3]
  public 'date' => string '2022-11-20 09:20:30.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'America/New_York' (length=16)
```

我们怎么得到 9:20:30 的？因为一旦我们改变时区，它就会转换时间。所以，之前设置好你的时区。

# 日期时间操作

这是日期时间对象开始显示其真实颜色的地方。这些操作将在两个对象之间进行。我们可以比较两个日期，看看一个日期是在另一个日期之后、之前还是相同。

```
<?php
$datetime1 = new DateTime("11/5/2022");
$datetime2 = new DateTime("11/4/2022");
var_dump($datetime1 > $datetime2);
var_dump($datetime1 < $datetime2);
var_dump($datetime1 == $datetime2);
```

```
/app/95 DateTime Object/index.php:46:boolean true
/app/95 DateTime Object/index.php:47:boolean false
/app/95 DateTime Object/index.php:48:boolean false
```

如您所见，在第一个操作中，我们看到第一个日期大于第二个日期。

现在，两个日期/时间之间的差异。这就是我们来这里的原因。似乎很复杂。但事实并非如此。

```
<?php
$datetime_from = new DateTime("01/20/1978 4:00:32");
$datetime_to   = new DateTime("11/20/2022 22:01:00");

$difference = $datetime_to->diff($datetime_from);
var_dump($difference);
```

只要记住`from`和`to`的日期。你会想要从你的`to`物体中减去。换句话说，这就是你要用来调用你的`diff`方法的那个。

```
/app/95 DateTime Object/index.php:55:
object(DateInterval)[7]
  public 'y' => int 44
  public 'm' => int 10
  public 'd' => int 0
  public 'h' => int 18
  public 'i' => int 0
  public 's' => int 28
  public 'f' => float 0
  public 'weekday' => int 0
  public 'weekday_behavior' => int 0
  public 'first_last_day_of' => int 0
  public 'invert' => int 1
  public 'days' => int 16375
  public 'special_type' => int 0
  public 'special_amount' => int 0
  public 'have_weekday_relative' => int 0
  public 'have_special_relative' => int 0
```

你会立刻得到一些很酷的数字。你可以看到这两个日期相差`44`年、`10`月、`18`小时和`28`秒。它还告诉我们两个日期之间的总天数，即`16,375`。

我们不必将结果存储在单独的对象中。我们可以继续。

```
<?php
$datetime_from = new DateTime("01/20/1978 4:00:32");
$datetime_to   = new DateTime("11/20/2022 22:01:00");
var_dump($datetime_to->diff($datetime_from)->format("The number of years passed is: %Y"));
```

```
/app/95 DateTime Object/index.php:
60:string 'The number of years passed is: 44' (length=33)
```

我们还需要能够轻松地对日期进行加减运算。如果我们想给一个日期加上 10 天或者减去 1 年零 65 天呢？

`add`方法代表一个`DateInterval`对象。`DateInterval`对象需要一个字符串传递给构造函数。它遵循以下格式:P(代表期间)数量(代表所需的数量)和类型(年、月、小时等)。你总是以一个`P`开头，后面跟着余数:`P1Y`表示一段`1`年。`P2Y3H`指 2 年零 3 小时的时间段。这些是可用的选项

`Y`年数。

`M`月数。

`D`天数。

`H`小时数。

`I`分钟数。

`S`秒数。

`F`微秒数。

```
<?php
$datetime = new DateTime("11/23/2022 22:01:00");
$datetime->add(new DateInterval("P10D"));
var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:65:
object(DateTime)[6]
  public 'date' => string '2022-12-03 22:01:00.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

同样，让我们从例子中减去 1 年零 65 天。

```
<?php
$datetime = new DateTime("11/23/2022 22:01:00");
$datetime->sub(new DateInterval("P1Y65D"));
var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:70:
object(DateTime)[3]
  public 'date' => string '2021-09-19 22:01:00.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

如果您需要减去时间，您需要在您的`DateInterval`字符串前面加上一个`PT`，例如`PT10H`。

```
<?php
$datetime = new DateTime("11/23/2022 22:01:00");
$datetime->sub(new DateInterval("PT10H"));
var_dump($datetime);
```

```
/app/95 DateTime Object/index.php:75:
object(DateTime)[6]
  public 'date' => string '2022-11-23 12:01:00.000000' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

好了，你是日期时间专家。还有其他我们不会涉及的操作，因为大部分时间这是你将要生活的地方。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/a68f81f8b176a6fcfee05d98c11589a2.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 主管，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。