# 火箭计划

> 原文：<https://blog.devgenius.io/scheduling-with-rocketry-5aa5b1bed520?source=collection_archive---------9----------------------->

![](img/f6347349bbb8b21bbec91aaa5412e7ea.png)

这是火箭学教程系列的第二部分。上次我做了一个关于火箭学的介绍性教程，你可以在这里找到它。如果对火箭学不熟悉，我建议先看一下。

在本教程中，我们讨论 Rocketry 的调度选项以及如何使用它们。我们还将介绍如何创建自定义条件。

# 计划的基础

Rocketry 是一个基于语句的调度程序。这意味着任务的启动取决于一个可能为真或为假的陈述。这个语句实际上可以是任何内容，也可以是使用逻辑运算符的几个语句的组合。

下面是一个简单的例子:

```
from rocketry import Rocketry
from rocketry.conds import true, false

app = Rocketry()

@app.task(true)
def do_constantly():
    ...

@app.task(false)
def do_never():
    ...

if __name__ == "__main__":
    app.run()
```

第一个任务将持续运行，因为它的启动条件总是*真*，而第二个任务将永远不会运行。通常情况下，您不希望任务在运行后立即再次运行。几个条件，如*每日*，依赖于任务运行历史，以确保当任务已经运行时它们不再为*真*。我们稍后将回到这些类型的条件，让我们回到基础。

因为条件只是布尔语句(真或假)，所以您还可以对它们使用逻辑运算符(AND、or、NOT):

```
@app.task(true & (false | ~false))
def do_constantly():
    ...
```

上述任务持续运行，因为结果条件仅仅是*真*。Rocketry 使用按位运算符作为逻辑运算符。以下是定义:

*   和运算符:a & b
*   OR 运算符:a | b
*   非运算符:~a

逻辑运算符使您能够扩展内置条件和自定义条件。您可以用它们创建非常复杂的调度策略。

# 调度选项

Rocketry 有大量用于各种目的的内置调度条件。这些条件中的许多都有进一步约束它们的附加选项。这些条件可以分为几类:

*   基于时间:每小时运行一次，当 10 秒钟过去时，等等。
*   任务相关:在另一个任务之后运行
*   自定义:基于纯自定义逻辑运行
*   以上的组合

此外，基于时间的调度可以分为两个部分:

*   固定时间(即。一天跑一次，一周跑一次等等。)
*   浮动时间(即。10 秒钟后运行)

并且固定时间调度可以进一步分成两部分:

*   依赖于运行(任务是否已在给定时间段内运行)
*   无状态(当前时间是否在给定时间段内)

我们会逐一查看。

## 基于时间的调度

在其他框架中，cron 风格的调度通常用于基于时间的调度。Cron 是一种 UNIX 调度语言，用于在指定的时间间隔内运行任务。Rocketry 还支持 cron 风格的调度:

```
from rocketry.conds import cron

@app.task(cron("0 10 * * *"))
def do_cron():
    ...
```

这个特定的任务每天上午 10 点运行一次。您可以使用[https://crontab.guru/](https://crontab.guru/)对 cron 调度进行更多的修改。

我个人觉得 cron 很难读懂，也经常很难维护。如果你像我一样，你可能更喜欢火箭技术的其他基于时间的条件:

```
from rocketry.conds import hourly, daily, weekly, monthly

@app.task(hourly)
def do_hourly():
    ... # Runs once an hour

@app.task(daily)
def do_daily():
    ... # Runs once a day

@app.task(weekly)
def do_weekly():
    ... # Runs once a week

@app.task(monthly)
def do_monthly():
    ... # Runs once a month
```

此外，这些条件也有额外的方法来约束它们为真时的时间段:之间的*、*处的*/*处的*、*之后的*、*之前的*以及从*开始的*:*

```
from rocketry.conds import daily

@app.task(daily.between("10:00", "13:00"))
def do_between():
    # Runs once when time is between 10 AM and 1 PM
    ...

@app.task(daily.at("10:00"))
def do_at():
    # Runs once when time is between 10 AM and 11 AM
    ...

@app.task(daily.after("10:00"))
def do_after():
    # Runs once when time is after 10 AM
    ...

@app.task(daily.before("13:00"))
def do_before():
    # Runs once when time is before 1 PM
    ...

@app.task(daily.starting("10:00"))
def do_starting():
    # Runs once a day but the day starts at 10 AM in 
    # this context
    ...
```

Rocketry 还支持浮动时间调度。例如，如果您希望在任务上次运行后经过特定时间后运行该任务:

```
from rocketry.conds import every

@app.task(every("2 hours 30 minutes"))
def do_every():
    ...
```

## 无状态的基于时间的条件

也有不依赖于任务的基于时间的条件。如果当前时间是给定的，则为真，否则为假。以下是一些例子:

```
from rocketry.conds import time_of_hour, time_of_day, time_of_week, time_of_month

@app.task(time_of_hour.between("15:00", "45:00"))
def do_between():
    # Runs constantly when it is between 15 minutes past 
    # and 15 minutes to full hour
    ...

@app.task(time_of_day.at("10:00"))
def do_at():
    # Runs constantly when time is between 10 AM and 11 AM
    ...

@app.task(time_of_week.after("Monday"))
def do_after():
    # Runs constantly when the current week day is 
    # after Monday (including Monday)
    ...

@app.task(time_of_month.before("20th"))
def do_before():
    # Runs constantly when current day of month is 
    # before 20th day
    ...
```

这些条件在与其他条件结合时更有用。它们可用于进一步限制任务运行的时间。

## 任务相关性

Rocketry 还支持设置任务依赖关系:一个任务将在另一个任务完成时运行。火箭学将它们直接应用到条件力学中:

```
from rocketry.conds import after_success

@app.task()
def do_first():
    ...

@app.task(after_success(do_first))
def do_second():
    ...
```

当任务 *do_first* 成功时，任务 *do_second* 将运行。还有 *after_failure* 和 *after_finish* 条件。

如果您想在几个任务成功后运行您的任务，您可以使用这个条件和 OR 操作符，或者您可以使用 *after_all_success* 来节省键入:

```
from rocketry.conds import after_success

@app.task()
def do_a():
    ...
@app.task()
def do_b():
    ...
@app.task(after_all_success(do_a, do_b))
def do_second():
    ...
```

此外，还有 *after_all_fail* 和 *after_all_finish 以及所有这些选项的任何*变体(即 *after_any_fail* )。

## 自定义条件

在某些时候，你可能会意识到你无法从现有的调度条件中找到一个合适的。使用 Rocketry 创建自定义条件也很容易:

```
from pathlib import Path

@app.cond()
def path_exists(file):
    return Path(file).exists()

@app.task(path_exists("myfile.csv"))
def do_with_file():
    ...
```

上面我们创建了一个给定文件存在时为真的条件。然后我们创建了一个任务，当 *myfile.csv* 存在时运行。请注意，该特定任务将不断重新运行，直到文件被删除。您可能希望将此与其他条件结合起来，例如每日*。*

*您还可以使用任务本身、一些会话参数或调度程序本身来定义条件的逻辑。然而，这超出了本教程的范围，我们将在下一次讨论。*

# *结论*

*现在我们已经知道了所有基本的条件类型，我将结合我们所学的内容，给大家留下一些复杂的例子:*

```
*from rocketry.conds import hourly, daily, weekly, time_of_day

@app.task(weekly.on("Monday") | weekly.on("Tuesday"))
def do_twice_a_week():
    ...

@app.task(hourly & ~time_of_day.between("10:00", "12:00"))
def do_hourly_but_not_on_lunch():
    ...

@app.task(path_exists("myfile.csv") & daily.after("10:00"))
def do_daily_when_file_exists():
    ... # The condition "path_exists" is from a previous example*
```

*我希望你喜欢这个教程。如果你希望让我振作起来，帮助我专注于开发火箭技术并围绕它创造内容，你可以在 Github 上给我买一杯咖啡:[https://github.com/sponsors/Miksus](https://github.com/sponsors/Miksus)。*

*火箭学的链接:*

*   *文件:[https://rocketry.readthedocs.io/](https://rocketry.readthedocs.io/)*
*   *来源: [https://github.com/Miksus/rocketry](https://github.com/Miksus/rocketry)*