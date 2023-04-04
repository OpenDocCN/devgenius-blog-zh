# 火箭学导论

> 原文：<https://blog.devgenius.io/introduction-to-rocketry-7650e2bb38be?source=collection_archive---------2----------------------->

![](img/b88b6028672ef815d624dc4f94d487dd.png)

rockerry 是一个基于语句的 Python 调度引擎。它有数百个调度选项，可以用简单的逻辑根据自定义条件任意扩展。Rocketry 适用于小型或大型自动化项目。它可以与现有的应用程序集成，也可以作为大型系统的自动化后端。

去年夏天我最初写了[一篇展示火箭技术的文章](https://medium.com/itnext/red-engine-insanely-powerful-scheduler-7d9d8e84b58b)(当时被称为红色引擎)，后来[在有了更多进展后重写了这篇文章](https://medium.com/itnext/new-paradigm-on-scheduling-cf2e55950a0d)。现在这个库的特性更加稳定了，我想也许是时候为这个框架提供合适的教程了，所以我决定从这个介绍开始。

但是让我们开始吧。通常，调度框架需要三个组件:

*   调度:任务何时运行，
*   执行:您的任务将如何运行，
*   日志记录:持久性

在本教程中，我们将逐一介绍这些方面以及它们在火箭学中的工作原理。我们从一个最小的工作示例开始，浏览 Rocketry 的基本调度，然后讨论任务执行和日志记录。

# 创建调度程序

下面是一个简单调度程序的示例:

```
from rocketry import Rocketry
from rocketry.conds import daily

app = Rocketry()

@app.task(daily)
def do_things():
    ... # Put your code here

if __name__ == "__main__":
    app.run()
```

这里发生了什么？

*   首先，我们进口我们需要的东西，
*   启动了火箭应用程序，
*   创建了一个每天执行一次的函数任务 *do_things*
*   在脚本的主块中启动应用程序

您可以在函数中执行您的任务需要执行的任何操作。接下来我们将讨论调度是如何工作的。

# 任务调度

如前所述，Rocketry 是一个基于语句的调度程序。每个任务都是使用语句或条件开始的。如果任务的启动条件为真，任务将启动；如果启动条件为假，任务将等待。这些条件也可以使用基本逻辑(AND、OR、NOT)任意组合。

Rocketry 有大量用于各种目的的内置调度条件。这些选项可以分为几类:

*   基于时间:每小时、每天、每周等运行一次。
*   任务相关:在另一个任务之后运行
*   自定义:基于纯自定义逻辑运行
*   以上的组合

这个话题非常广泛，我们不会在本文中深入探讨。接下来我将展示这些选项。在本文中，我将只向您展示一些示例，但是您可以从文档中阅读更多内容。

我会给你一些例子，你可以试试:

```
from rocketry.conds import hourly, daily, every, after_success

@app.task(hourly)
def do_hourly():
    ...

@app.task(daily.at("10:00"))
def do_daily_at_ten():
    ...

@app.task(every("2 hours"))
def do_after_every_two_hours():
    ...

@app.task(after_success(do_hourly))
def do_after_another():
    ...
```

您还可以创建自定义条件(并将它们与其他条件组合在一起):

```
from rocketry.conds import daily
from pathlib import Path

@app.cond()
def path_exists(file):
    return Path(file).exists()

@app.task(daily & path_exists("myfile.csv"))
def do_daily():
    ...
```

当 *myfile.csv* 存在时，上述任务每天运行一次。你可以在[的下一篇教程](/scheduling-with-rocketry-5aa5b1bed520)中读到更多关于排班的内容。

# 任务执行

调度框架的另一个基本特征是任务执行；任务如何运行。在这种情况下，我们主要讨论并行性。一个简单的调度器可能只允许一个任务在一个给定的时间运行，而更复杂的框架允许一次运行多个任务，称为并行执行。火箭技术让你决定方法。

运行任务有三种执行选项:

*   **主**:同步磨合
*   **异步**:异步运行
*   **线程**:在单独的线程中运行
*   **进程**:在单独的进程中运行

您可以自由地尝试适合您情况的最佳选项。执行很容易设置:

```
@app.task(execution="main")
def do_sync():
    ...

@app.task(execution="async")
async def do_async():
    ... # This runs using async

@app.task(execution="thread")
def do_thread():
    ... # This runs in a thread process

@app.task(execution="process")
def do_process():
    ... # This runs in a separate process
```

如果异步、线程和多处理对你来说是新的，[我写了一个实用的介绍来解释这些](https://medium.com/itnext/practical-guide-to-async-threading-multiprocessing-958e57d7bbb8)。下面总结了每种方法的有用之处:

*   main:对维护任务有用
*   async:如果库支持，对 IO 绑定的任务有用
*   线程:对于 IO 绑定的任务很有用
*   进程:对于 CPU 受限的任务很有用

此外，如果使用异步任务，任务函数应该定义为 *async* ，并且还应该在任务中使用 await。线程和异步任务并发运行，但不是并行的(除了一些 IO 绑定的操作)，而流程任务可以真正并行运行。这听起来似乎流程执行是最好的选择，但是您也应该注意到，从资源角度来说，它是最昂贵的。

您还可以设置默认执行，这样就不需要在每个任务中都指定它:

```
from rocketry import Rocketry

app = Rocketry(execution="main")

@app.task()
def do_things():
    ... # This uses default execution
```

# 任务日志记录

为了在系统中保持持久性，任务日志记录在调度程序中是必不可少的。如果调度程序崩溃或重启，系统应该能够从它之前离开的地方恢复。如果没有可靠的日志记录，调度程序可能会过于频繁地运行一些任务。

Rocketry 的任务日志是建立在日志库(Python 的标准库)之上的。它还使用 Red Bird 的存储库模式使日志可读，并在同一 API 下统一不同类型的数据存储，如 CSV 文件、SQL 数据库或 MongoDB。

默认情况下，Rocketry 使用内存中的列表作为日志记录存储。这对于测试很有用，但不建议用于生产，因为日志会在重启时被清除，并且日志会累积内存。

设置存储库的推荐方法是使用应用程序设置装饰器:

```
from rocketry import Rocketry
from rocketry.args import TaskLogger
from rocketry.log import MinimalRecord

from redbird.repos import MemoryRepo

app = Rocketry()

@app.setup()
def setup_app(task_logger=TaskLogger()):
    repo = MemoryRepo(model=MinimalRecord)
    task_logger.set_repo(repo)

...
```

下面是 *setup_app* 中发生的事情:

*   我们使用 Rocketry 的动态参数来获取任务日志
*   我们使用 *MinimalRecord* 作为存储日志的模型，创建一个内存存储库(数据存储)
*   我们将这个存储库设置为任务记录器中的主存储库

我们不会深入讨论 Rocketry 的参数机制，但是 *task_logger* 将获得一个日志适配器作为值。这个适配器是日志库的 logger 的包装器，具有一些额外的功能，比如从日志中读取，以及为日志记录添加数据存储的一种方便的方法。该功能在调度应用程序的启动序列中运行。

数据存储还有其他选项，如 CSV、MongoDB 和 SQL:

```
# CSV
from redbird.repos import CSVFileRepo

@app.setup()
def setup_app(task_logger=TaskLogger()):
    repo = CSVFileRepo(filename="logs.csv", model=MinimalRecord)
    task_logger.set_repo(repo)
```

```
# SQL
from redbird.repos import SQLRepo

@app.setup()
def setup_app(task_logger=TaskLogger()):
    repo = SQLRepo(conn_string="sqlite://", table="mylogs", if_missing="create", model=MinimalRecord, id_field="created")
    task_logger.set_repo(repo)
```

```
# MongoDB
from redbird.repos import MongoRepo

@app.setup()
def setup_app(task_logger=TaskLogger()):
    repo = MongoRepo(uri="mongodb://localhost:27017", database="rocketry", collection="mylog", model=MinimalRecord)
    task_logger.set_repo(repo)
```

选择你需要的那个。注意，我们必须用 SQLRepo 传递 *id_field* ，因为它被用作主键，并且 SQLAlchemy 在反射中需要它。

# 结论

在本系列的下一部分中，我们将更深入地研究 Rocketry 中的调度。[你可以在这里找到下一个教程](/scheduling-with-rocketry-5aa5b1bed520)。

我希望这篇介绍能帮助你开始使用火箭学。将来我可能会为这个框架的各个方面再做几个教程。我也很想知道人们用它创造了什么，人们对它有什么想法，所以请随意在 Rocketry 的 Github 页面上发表讨论。

火箭学的链接:

*   文件:[https://rocketry.readthedocs.io/](https://rocketry.readthedocs.io/)
*   源代码:[https://github.com/Miksus/rocketry](https://github.com/Miksus/rocketry)

如果你喜欢火箭技术，你可以在 Github 上给我买一杯咖啡来资助我的开发:[https://github.com/sponsors/Miksus](https://github.com/sponsors/Miksus)