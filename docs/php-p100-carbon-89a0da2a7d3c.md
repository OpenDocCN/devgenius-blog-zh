# PHP — P100:碳

> 原文：<https://blog.devgenius.io/php-p100-carbon-89a0da2a7d3c?source=collection_archive---------9----------------------->

![](img/6fd4cb0949a372b9929edaa53226a48d.png)

PHP 上的第 100 篇帖子。哇哦。好了，别再自我安慰了。让我们来看看碳。碳是什么？还记得我们讨论过的`DateTime`物体吗？是 98 后。嗯，`Carbon`是一个 PHP 依赖项，它扩展了`DateTime`类。它有很多简化的界面，使用起来非常简单。

[](https://dinocajic.medium.com/php-p98-datetime-object-1191e4b457c1) [## PHP — P98:日期时间对象

### 在前一篇文章中，我们研究了 PHP 中一些最常用的日期和时间函数。我们…

dinocajic.medium.com](https://dinocajic.medium.com/php-p98-datetime-object-1191e4b457c1) [](https://dinocajic.medium.com/php-p99-dependency-management-intro-fe42877964d4) [## PHP — P99:依赖管理简介

### 我们已经写了将近 100 篇文章，还没有讨论 PHP 中的依赖关系。我们结束了上一篇关于…的文章

dinocajic.medium.com](https://dinocajic.medium.com/php-p99-dependency-management-intro-fe42877964d4) 

# 安装碳依赖性

在你安装 Carbon 之前，你需要确保你已经安装了`composer`。Composer 是一个依赖管理器，也是上一篇文章的主题。

一旦安装了`composer`，运行下面的命令。

```
composer require nesbot/carbon
```

这将引入并创建`vendor`目录，添加所有必要的依赖项，包括`Carbon`，并创建`composer.json`文件。

```
{
    "require": {
        "nesbot/carbon": "^2.63"
    }
}
```

`vendor`目录也包含了`autoload.php`文件，它自动为你`requires`所有的依赖项。你只需要在你的文件中`require`一次`autoload.php`就可以了。您必须使用`use Carbon\Carbon`，因为`Carbon`位于它自己的名称空间(虚拟目录)中。

```
<?php // index.php
require 'vendor/autoload.php';

use Carbon\Carbon;

echo Carbon::now();
```

```
index.php

2022-11-25 23:04:53
```

也可以用`composer.json`文件安装。只需创建上面列出的`composer.json`文件，并在目录中运行`composer install`。

每当需要更新依赖关系时，运行`composer update`。

# 实例化碳对象

有几种方法可以实例化`Carbon`。上面我们已经看了一种方式:`Carbon::now()`。让我们看看其他的例子:

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

// Current Date and Time
$now = Carbon::now();
var_dump($now);
```

```
/app/97 Carbon/index.php:8:
object(Carbon\Carbon)[3]
  protected 'endOfTime' => boolean false
  protected 'startOfTime' => boolean false
  protected 'constructedObjectId' => string '000000002064ed5d00000000762b1998' (length=32)
  protected 'localMonthsOverflow' => null
  protected 'localYearsOverflow' => null
  protected 'localStrictModeEnabled' => null
  protected 'localHumanDiffOptions' => null
  protected 'localToStringFormat' => null
  protected 'localSerializer' => null
  protected 'localMacros' => null
  protected 'localGenericMacros' => null
  protected 'localFormatFunction' => null
  protected 'localTranslator' => null
  protected 'dumpProperties' => 
    array (size=3)
      0 => string 'date' (length=4)
      1 => string 'timezone_type' (length=13)
      2 => string 'timezone' (length=8)
  protected 'dumpLocale' => null
  protected 'dumpDateProperties' => null
  public 'date' => string '2022-11-25 23:09:41.281069' (length=26)
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
```

如你所见，你得到了整个物体。

```
<?php
// ...

// Current Date and Time at a Specific Timezone
$timezone = new DateTimeZone(" America/New_York");
$now_at_eastern_timezone = Carbon::now( $timezone );
var_dump($now_at_eastern_timezone);
```

您得到了相同的对象，但是您会注意到最后一个属性`timezone`被设置为`America/New_York`。

```
object(Carbon\Carbon)[4]
  // ...
  public 'timezone' => string 'America/New_York' (length=16)
```

我们不必创建当前日期和时间。我们可以实例化昨天或明天。

```
<?php
// ...

// Yesterday at 00:00:00
$yesterday = Carbon::yesterday();
var_dump($yesterday);
```

```
<?php
// ...

// Tomorrow at 00:00:00
$tomorrow = Carbon::tomorrow();
var_dump($tomorrow);
```

我们也可以用`yesterday`和`tomorrow`设置一个特定的时区。

```
<?php
// ...

// Yesterday at 00:00:00 in America/New_York
$timezone = new DateTimeZone(" America/New_York");
$yesterday = Carbon::yesterday( $timezone );
var_dump($yesterday);
```

如果我们想实例化到一个特定的日期呢？

```
<?php
// ...

// Instantiate to a specific date at the current time
$year = 2005;
$month = 10;
$day = 15;
$timezone = "America/New_York";

$specific_date = Carbon::createFromDate($year, $month, $day, $timezone);
var_dump($specific_date);
```

您也可以保留当前日期，但实例化到特定时间。

```
<?php
// ...

// Instantiate to a specific time today
$hour = 5;
$minute = 19;
$second = 35;
$timezone = "America/New_York";

$specific_time = Carbon::createFromTime($hour, $minute, $second, $timezone);
var_dump($specific_date);
```

我想你知道我们下一步要去哪里:具体的日期和时间。

```
<?php
// ...

// Instantiate to a specific date and time
$year = 2005;
$month = 10;
$day = 15;
$hour = 5;
$minute = 19;
$second = 35;
$timezone = "America/New_York";

$specific_date_and_time = Carbon::create($year, $month, $day, $hour, $minute, $second, $timezone);
var_dump($specific_date_and_time);
```

有相当多的方法来创建碳对象，但这涵盖了你将使用的大多数方法。

# 吸气剂

是时候访问一些属性了。由于`Carbon`使用的命名约定，这非常简单。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$now = Carbon::now();

var_dump($now->year);
var_dump($now->yearIso);
var_dump($now->month);
var_dump($now->day);
var_dump($now->hour);
var_dump($now->minute);
var_dump($now->second);
var_dump($now->micro);
var_dump($now->microsecond);
var_dump($now->timestamp);               // seconds since the Unix Epoch
var_dump($now->englishDayOfWeek);        // the day of week in English
var_dump($now->shortEnglishDayOfWeek);   // the abbreviated day of week in English
var_dump($now->englishMonth);            // the month in English
var_dump($now->shortEnglishMonth);       // the abbreviated month in English
var_dump($now->milliseconds);
var_dump($now->millisecond);
var_dump($now->milli);
var_dump($now->week);                    // 1 through 53
var_dump($now->isoWeek);                 // 1 through 53
var_dump($now->weekYear);                // year according to week format
var_dump($now->isoWeekYear);             // year according to ISO week format
var_dump($now->dayOfYear);               // 1 through 366
var_dump($now->age);                     // does a diffInYears() with default parameters
var_dump($now->offset);                  // the timezone offset in seconds from UTC
var_dump($now->offsetMinutes);           // the timezone offset in minutes from UTC
var_dump($now->offsetHours);             // the timezone offset in hours from UTC
var_dump($now->timezone);                // the current timezone
var_dump($now->tz);                      // alias of $timezone
var_dump($now->dayOfWeek);               // 0 (for Sunday) through 6 (for Saturday)
var_dump($now->dayOfWeekIso);            // 1 (for Monday) through 7 (for Sunday)
var_dump($now->weekOfYear);              // ISO-8601 week number of year, weeks starting on Monday
var_dump($now->daysInMonth);             // number of days in the given month
var_dump($now->latinMeridiem);           // "am"/"pm" (Ante meridiem or Post meridiem latin lowercase mark)
var_dump($now->latinUpperMeridiem);      // "AM"/"PM" (Ante meridiem or Post meridiem latin uppercase mark)
var_dump($now->timezoneAbbreviatedName); // the current timezone abbreviated name
var_dump($now->tzAbbrName);              // alias of $timezoneAbbreviatedName
var_dump($now->dayName);                 // long name of weekday translated according to Carbon locale, in english if no translation available for current language
var_dump($now->shortDayName);            // short name of weekday translated according to Carbon locale, in english if no translation available for current language
var_dump($now->minDayName);              // very short name of weekday translated according to Carbon locale, in english if no translation available for current language
var_dump($now->monthName);               // long name of month translated according to Carbon locale, in english if no translation available for current language
var_dump($now->shortMonthName);          // short name of month translated according to Carbon locale, in english if no translation available for current language
var_dump($now->meridiem);                // lowercase meridiem mark translated according to Carbon locale, in latin if no translation available for current language
var_dump($now->upperMeridiem);           // uppercase meridiem mark translated according to Carbon locale, in latin if no translation available for current language
var_dump($now->noZeroHour);              // current hour from 1 to 24
var_dump($now->weeksInYear);             // 51 through 53
var_dump($now->isoWeeksInYear);          // 51 through 53
var_dump($now->weekOfMonth);             // 1 through 5
var_dump($now->weekNumberInMonth);       // 1 through 5
var_dump($now->firstWeekDay);            // 0 through 6
var_dump($now->lastWeekDay);             // 0 through 6
var_dump($now->daysInYear);              // 365 or 366
var_dump($now->quarter);                 // the quarter of this instance, 1 - 4
var_dump($now->decade);                  // the decade of this instance
var_dump($now->century);                 // the century of this instance
var_dump($now->millennium);              // the millennium of this instance
var_dump($now->dst);                     // daylight savings time indicator, true if DST, false otherwise
var_dump($now->local);                   // checks if the timezone is local, true if local, false otherwise
var_dump($now->utc);                     // checks if the timezone is UTC, true if UTC, false otherwise
var_dump($now->timezoneName);            // the current timezone name
var_dump($now->tzName);                  // alias of $timezoneName
var_dump($now->locale);                  // locale of the current instance
```

```
/app/97 Carbon/getters.php:8:int 2022
/app/97 Carbon/getters.php:9:int 2022
/app/97 Carbon/getters.php:10:int 11
/app/97 Carbon/getters.php:11:int 25
/app/97 Carbon/getters.php:12:int 23
/app/97 Carbon/getters.php:13:int 41
/app/97 Carbon/getters.php:14:int 30
/app/97 Carbon/getters.php:15:int 724054
/app/97 Carbon/getters.php:16:int 724054
/app/97 Carbon/getters.php:17:int 1669419690
/app/97 Carbon/getters.php:18:string 'Friday' (length=6)
/app/97 Carbon/getters.php:19:string 'Fri' (length=3)
/app/97 Carbon/getters.php:20:string 'November' (length=8)
/app/97 Carbon/getters.php:21:string 'Nov' (length=3)
/app/97 Carbon/getters.php:22:int 724
/app/97 Carbon/getters.php:23:int 724
/app/97 Carbon/getters.php:24:int 724
/app/97 Carbon/getters.php:25:int 48
/app/97 Carbon/getters.php:26:int 47
/app/97 Carbon/getters.php:27:int 2022
/app/97 Carbon/getters.php:28:int 2022
/app/97 Carbon/getters.php:29:int 329
/app/97 Carbon/getters.php:30:int 0
/app/97 Carbon/getters.php:31:int 0
/app/97 Carbon/getters.php:32:int 0
/app/97 Carbon/getters.php:33:int 0
/app/97 Carbon/getters.php:34:
object(Carbon\CarbonTimeZone)[10]
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
/app/97 Carbon/getters.php:35:
object(Carbon\CarbonTimeZone)[11]
  public 'timezone_type' => int 3
  public 'timezone' => string 'Europe/London' (length=13)
/app/97 Carbon/getters.php:36:int 5
/app/97 Carbon/getters.php:37:int 5
/app/97 Carbon/getters.php:38:int 47
/app/97 Carbon/getters.php:39:int 30
/app/97 Carbon/getters.php:40:string 'pm' (length=2)
/app/97 Carbon/getters.php:41:string 'PM' (length=2)
/app/97 Carbon/getters.php:42:string 'GMT' (length=3)
/app/97 Carbon/getters.php:43:string 'GMT' (length=3)
/app/97 Carbon/getters.php:44:string 'Friday' (length=6)
/app/97 Carbon/getters.php:45:string 'Fri' (length=3)
/app/97 Carbon/getters.php:46:string 'Fr' (length=2)
/app/97 Carbon/getters.php:47:string 'November' (length=8)
/app/97 Carbon/getters.php:48:string 'Nov' (length=3)
/app/97 Carbon/getters.php:49:string 'pm' (length=2)
/app/97 Carbon/getters.php:50:string 'PM' (length=2)
/app/97 Carbon/getters.php:51:int 23
/app/97 Carbon/getters.php:52:int 53
/app/97 Carbon/getters.php:53:int 52
/app/97 Carbon/getters.php:54:int 4
/app/97 Carbon/getters.php:55:int 4
/app/97 Carbon/getters.php:56:int 1
/app/97 Carbon/getters.php:57:int 0
/app/97 Carbon/getters.php:58:int 365
/app/97 Carbon/getters.php:59:int 4
/app/97 Carbon/getters.php:60:int 203
/app/97 Carbon/getters.php:61:int 21
/app/97 Carbon/getters.php:62:int 3
/app/97 Carbon/getters.php:63:boolean false
/app/97 Carbon/getters.php:64:boolean true
/app/97 Carbon/getters.php:65:boolean true
/app/97 Carbon/getters.php:66:string 'Europe/London' (length=13)
/app/97 Carbon/getters.php:67:string 'Europe/London' (length=13)
/app/97 Carbon/getters.php:68:string 'en' (length=2)
```

# 安装员

也可以在对象实例化后设置属性。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$date = Carbon::now();

$date->timezone = "America/New_York";
$date->year = 2005;
$date->month = 6;
$date->day = 20;
$date->hour = 5;
$date->minute = 20;

// You can also prepend each property with set and pass the argument
$date->setYear(2010);

var_dump($date);
```

记得链吗？你也可以在碳物体上这样做。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$date = Carbon::now();

$date->timezone("America/New_York")
    ->year(2005)
    ->month(6)
    ->day(20)
    ->hour(5)
    ->minute(20);

var_dump($date);
```

# 字符串格式

就像我们在`DateTime`对象中使用的字符串格式一样，我们可以在这里做同样的事情。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$date = Carbon::now();

echo $date->format("F d, Y h:i:s");
```

```
November 25, 2022 11:55:04
```

我们还可以输出特定的格式，比如 JSON 格式。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$date = Carbon::now();

echo $date->toJSON();
```

```
2022-11-25T23:55:04.833120Z
```

# 比较

这才是我们真正感兴趣的。我们想比较两个日期。一个比另一个大吗？一个不等于另一个吗？再次，琐碎对于`Carbon`。这些操作中的每一个都将返回`true`或`false`。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$today    = Carbon::now();
$tomorrow = Carbon::tomorrow();

var_dump( $today->equalTo($tomorrow) ); // false
var_dump( $today->notEqualTo($tomorrow) ); // true
var_dump( $today->greaterThan($tomorrow) ); // false
var_dump( $today->greaterThanOrEqualTo($tomorrow) ); // false
var_dump( $today->lessThan($tomorrow) ); // true
var_dump( $today->lessThanOrEqualTo($tomorrow) ); // true
```

我们可以检查一个日期是否在两个日期之间。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$today    = Carbon::now();
$tomorrow = Carbon::tomorrow();
$date     = Carbon::create(2022, 11, 25, 10, 00, 00);

var_dump( $date->between($today, $tomorrow) );
var_dump( $date->isBetween($today, $tomorrow) ); // Same as between
```

还有很多其他的比较。我们可以对我们的对象执行大量不同的操作。

```
<?php
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);

require 'vendor/autoload.php';

use Carbon\Carbon;

$today    = Carbon::now();
$tomorrow = Carbon::tomorrow();
$date     = Carbon::create(2022, 11, 25, 10, 00, 00);

var_dump( $date->isFuture() );
var_dump( $date->isPast() );
var_dump( $date->isCurrentYear() );
var_dump( $date->isCurrentMonth() );
var_dump( $date->isNextYear() );
var_dump( $date->isLastYear() );
var_dump( $date->isLeapYear() );
var_dump( $date->isNextQuarter() );
var_dump( $date->isLastQuarter() );
var_dump( $date->isNextMonth() );
var_dump( $date->isLastMonth() );
var_dump( $date->isNextWeek() );
var_dump( $date->isLastWeek() );
var_dump( $date->isSameYear($tomorrow) );
var_dump( $date->isSameMonth($tomorrow) );
var_dump( $date->isSameQuarter($tomorrow) );
var_dump( $date->isSameHour($tomorrow) );
var_dump( $date->isSameMinute($tomorrow) );
var_dump( $date->isSameSecond($tomorrow) );
var_dump( $date->isWeekday() );
var_dump( $date->isWeekend() );
var_dump( $date->isMonday() );
var_dump( $date->isTuesday() );
var_dump( $date->isWednesday() );
var_dump( $date->isThursday() );
var_dump( $date->isFriday() );
var_dump( $date->isSaturday() );
var_dump( $date->isSunday() );
var_dump( $date->is("2022") );
var_dump( $date->is("October 10") );
var_dump( $date->is("October 10, 2022") );
var_dump( $date->isCurrentDay() );
var_dump( $date->isYesterday() );
var_dump( $date->isToday() );
var_dump( $date->isTomorrow() );
var_dump( $date->isSameHour($today) );
var_dump( $date->isSameMinute($today) );
var_dump( $date->isSameSecond($today) );
var_dump( $date->isCurrentHour() );
var_dump( $date->isCurrentMinute() );
var_dump( $date->isCurrentSecond() );
```

# 加法和减法

我们经常需要在现有日期的基础上增加/减少一些时间框架。我们可以很容易做到这一点。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$now = Carbon::now();

var_dump( $now );
var_dump( $now->addYear() );
var_dump( $now->addYears(10) );
var_dump( $now->subYear() );
var_dump( $now->subYears(2) );

var_dump( $now->addMonth() );
var_dump( $now->addMonths(2) );
var_dump( $now->subMonth() );
var_dump( $now->subMonths(25) );

var_dump( $now->addDay() );
var_dump( $now->addDays(5) );
var_dump( $now->subDay() );
var_dump( $now->subDays(92) );

var_dump( $now->addWeekday() );
var_dump( $now->addWeekdays(33) );
var_dump( $now->subWeekday() );
var_dump( $now->subWeekdays(9) );

var_dump( $now->addHour() );
var_dump( $now->addHours(3) );
var_dump( $now->subHour() );
var_dump( $now->subHours(2) );

var_dump( $now->addMinut() );
var_dump( $now->addMinuts(30) );
var_dump( $now->subMinut() );
var_dump( $now->subMinuts(22) );

var_dump( $now->addSecond() );
var_dump( $now->addSeconds(32) );
var_dump( $now->subSecond() );
var_dump( $now->subSeconds(275) );

var_dump( $now->addQuarter() );
var_dump( $now->addQuarters(6) );
var_dump( $now->subQuarter() );
var_dump( $now->subQuarters(2) );

var_dump( $now->addCentury() );
var_dump( $now->addCenturies(3) );
var_dump( $now->subCentury() );
var_dump( $now->subCenturies(3) );
```

# 两个日期之间的差异

除了上面所有的功能之外，你绝对会喜欢的一个功能是找出两个日期之间的差异的能力。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$yesterday   = Carbon::yesterday();
$future_date = Carbon::create(2025, 10, 10, 5, 30, 22);

var_dump( $future_date->diffInHours($yesterday) );
var_dump( $future_date->diffInMinutes($yesterday) );
var_dump( $future_date->diffInSeconds($yesterday) );
var_dump( $future_date->diffInDays($yesterday) );
var_dump( $future_date->diffInWeekdays($yesterday) );
var_dump( $future_date->diffInWeekendDays($yesterday) );
var_dump( $future_date->diffInWeeks($yesterday) );
var_dump( $future_date->diffInMonths($yesterday) );
var_dump( $future_date->diffInQuarters($yesterday) );
var_dump( $future_date->diffInYears($yesterday) );
```

```
/app/97 Carbon/difference.php:9:int 25205
/app/97 Carbon/difference.php:10:int 1512330
/app/97 Carbon/difference.php:11:int 90739822
/app/97 Carbon/difference.php:12:int 1050
/app/97 Carbon/difference.php:13:int 751
/app/97 Carbon/difference.php:14:int 300
/app/97 Carbon/difference.php:15:int 150
/app/97 Carbon/difference.php:16:int 34
/app/97 Carbon/difference.php:17:int 11
/app/97 Carbon/difference.php:18:int 2
```

还有一种人类可读的格式可以输出。

```
<?php
require 'vendor/autoload.php';

use Carbon\Carbon;

$yesterday   = Carbon::yesterday();
$future_date = Carbon::create(2025, 10, 10, 5, 30, 22);

var_dump( $future_date->diffForHumans($yesterday) );
```

```
/app/97 Carbon/difference.php:20:string '2 years after' (length=13)
```

# 常数

很难记住星期一是从`0`还是`1`开始？碳有一些内置的常数。

```
<?php

require 'vendor/autoload.php';

use Carbon\Carbon;

var_dump(Carbon::SUNDAY);             // int(0)
var_dump(Carbon::MONDAY);             // int(1)
var_dump(Carbon::TUESDAY);            // int(2)
var_dump(Carbon::WEDNESDAY);          // int(3)
var_dump(Carbon::THURSDAY);           // int(4)
var_dump(Carbon::FRIDAY);             // int(5)
var_dump(Carbon::SATURDAY);           // int(6)

var_dump(Carbon::YEARS_PER_CENTURY);  // int(100)
var_dump(Carbon::YEARS_PER_DECADE);   // int(10)
var_dump(Carbon::MONTHS_PER_YEAR);    // int(12)
var_dump(Carbon::WEEKS_PER_YEAR);     // int(52)
var_dump(Carbon::DAYS_PER_WEEK);      // int(7)
var_dump(Carbon::HOURS_PER_DAY);      // int(24)
var_dump(Carbon::MINUTES_PER_HOUR);   // int(60)
var_dump(Carbon::SECONDS_PER_MINUTE); // int(60)
```

而这些都是`Carbon`的基础。要查看更多关于碳的文档，请访问[官方文档](https://carbon.nesbot.com/docs/)。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/1165af3b5e31a66c00d8dcc044fc425e.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。