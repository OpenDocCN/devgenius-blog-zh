# 用 Dagster 构建 ETL 管道

> 原文：<https://blog.devgenius.io/build-etl-pipelines-with-dagster-4c5f2ac678db?source=collection_archive---------0----------------------->

**使用 Dagster，Python**

![](img/68c0dffe1a876223a729fefafa1b2ede.png)

使用 Dagster 的 ETL 管道

今天我们将介绍一个令人兴奋的新应用程序，名为 Dagster。它过去常常协调数据管道。在之前的一篇文章中，我们介绍了如何使用 Airflow 自动化 python 脚本。Dagster 改善了用户体验，并为我们提供了一个更好的日志记录和历史记录选项。Dagster 非常容易上手。它以 python 库的形式出现。我们将讨论 Dagster 的提取、转换和加载(ETL)流程编排。ETL 编排是构建 ETL 管道的常见过程。我们可以将编排定义为组织执行和监控 ETL 管道工作流的过程。今天，我们将使用 Dagster 来自动化我们之前使用 Airflow 开发的 Python ETL 管道。Dasgter 是一个相对较新但直观且健壮的工作流编排工具。缺少关于如何用 Python 和 Dagster 编写自己的数据管道的好材料。因此，我决定将我们基于 Python 的数据管道移植到 Dagster，以便向您概述设置和开发过程。

完整的代码可以在 [GitHub](https://github.com/hnawaz007/pythondataanalysis/tree/main/Dagster/etl) 上找到。代码演练可以在 [YouTube](https://www.youtube.com/watch?v=t8QADtYdWEI) 上找到。

**达格斯特简介**

ETL 管道具有相互依赖性，它们需要一个适当的系统来管理这些依赖性，以特定的顺序执行任务，检测和记录错误并生成警报。Dagster 通过有向无环图(DAG)向我们概述了 ETL 管道。它捕捉任务之间的关系和依赖关系。每个任务在 DAG 中按顺序列出，Dagster 按此顺序执行相应的任务，同时控制日志记录、调试以及处理故障和重试。我们将遵循表示为 DAG 的 ETL 管道。

Dagster 使我们能够:

*   构建结构良好、可测试的 ETL 管道
*   按计划运行 ETL 管道
*   以 Python 代码形式创建和管理脚本化数据管道
*   跟踪、监控和管理您的 ETL 管道

DAG 代表一组任务。它由依赖关系和关系组成，说明它们应该如何运行。Dag 被定义为一个 python 脚本，它将 DAG 的结构(任务及其依赖项)表示为代码。我们可以按计划运行 DAG，也可以手动触发它。此外，我们可以从一个失败中运行一个特定的任务，节省我们重新运行整个管道的时间。

**设置**

我们将在一个目录中建立一个虚拟环境，并在这个环境中安装 Dagster。

```
# create a virtual environment
python -m venv env
# activate virtual environment
env\Scripts\activate
# install dagster and dagit
pip install dagster dagit
# install additional libraries
pip install psycopg2
pip install pandas
# create a new project
dagster new-project etl
```

新生成的“etl”项目功能齐全，并带有默认的作业和时间表。这是其中文件和目录的分类。

![](img/3be55b33f01bfe1f472b461fb30b9a8e.png)

Dagster 项目目录概述

Dagster 是数据编排平台，Dagit 是 Dagster 的基于 web 的界面。通过 Dagit UI，我们可以查看 Dagster 对象并与之交互。

```
# run dagit in CLI
dagit
# open another CLI and run daemon
dagster-daemon run
```

Dagit 默认运行在端口 *3000* 上，我们可以导航到 *localhost:3000* 访问 Dagit UI *。* Dagster 需要一个存储库来存储运行时细节和日志。默认情况下，它会检查 *"DAGSTER_HOME"* 环境变量。确保设置一个系统环境变量。

**用 Dagster 构建 ETL 管道**

我们将重构我们的 Python ETL 管道脚本，使其与 Dagster 兼容。除了常规编程库，我们还将导入特定于 Dagster 的库(out、output、job 和 op)。然后，我们有一个日志库来记录信息或错误，以运行作业的详细信息。

让我们定义我们的源和目标数据库连接。我已经决定将连接转移到一个单独的文件 *db_con.py* 。这有两个功能:首先返回到 SQL Server 的连接。而第二个提供到 PostgreSQL 的连接。我们从顶部导入这个。

**源表&提取**

我们使用 op decorator 来定义 Dagster 中的任务。此任务将返回两个输出； *df* 和 *tbl* 。我们用关键字 out 在 decorator 之后定义它。我们定义输出的名称以及它们是否是必需的。在这种情况下，我们定义一个执行提取操作的函数。我们传递给它上下文。在上下文的帮助下，我们可以在 Dagster UI 中记录信息和错误。首先，我们从日志库中定义一个*日志记录器*的实例。我们将用这个*记录器*记录错误。然后，我们从 *db_con.py* 中用“ *get_sql_conn* ()”获得 SQL 服务器的连接细节。

我们获取想要从 SQL Server 的系统模式中提取数据的表。只需遍历表并查询它们。通过几行代码，我们查询了源代码并获得了作为 Pandas dataframe 的数据。我们记录信息和错误，如果有的话。然后我们用*返回两个输出，yield Output* 。

**负载**

在“*load _ dim _ product _ category*”函数中，我们记录信息和错误。然后我们用“ *get_postgres_creds* ”连接到 Postgres。使用这种连接，我们将数据帧写入表中。我们将这些数据保存到 Postgres 表中。我们调用“ *to_sql* ”函数，并在表名前面加上前缀“stg ”,表示这是一个临时表。我们已经完成了加载过程。

**工作**

我们在作业中导入上面定义的 op。我们创建一个作业文件" *run_etl* 。 *py* 并在其中导入 *etl.py* 操作符。我们把这个作业叫做“ *run_etl_job* ”。在 job 中，我们设置任务之间的依赖关系，并定义它们运行的顺序。我们运行提取函数并保存返回的输出。我们将这些输出传递给加载函数。我们将在 UI 中看到可视化表示。这就是我们如何用 Dagster 自动化我们的 ETL 管道。我们将作业文件导入存储库中，并在 *repository.py* 下调用作业。我们还可以导入该作业的时间表，并在时间表中对其进行排队。

**进度表**

我们将时间表定义为 python 代码。这个 etl 作业计划在每天早上十点运行作业。这是一个基于 cron 的计划，它接受标准的 cron 表达式。它还接受@每小时、@每天、@每周和@每月表达式。

**执行**

我们可以从 Dagit UI 中查看 etl 作业。我们可以选择该作业，它会显示该作业的 DAG。这项工作我们有两个操作员。我们可以单击单个操作员或 op，在右侧查看其详细信息。它告诉我们它有两个输出和它的平均执行时间。类似地，我们可以看到第二个操作的细节。在*发射台*下，我们可以手动触发作业。因此，我们将触发该作业并查看其执行情况。

![](img/dccc2e1e26ee9c5994a9b5ff84e6cdf6.png)

带有作业预览的 Dagit UI

我们可以从 Dagit UI 中查看 etl 作业。我们可以选择该作业，它会显示该作业的 DAG。这项工作我们有两个操作员。我们可以单击单个操作员或 op，在右侧查看其详细信息。它告诉我们它有两个输出和它的平均执行时间。类似地，我们可以看到第二个操作的细节。在*发射台*下，我们可以手动触发作业。因此，我们将触发该作业并查看其执行情况。

![](img/5c023d1bcd503fdcdd1021d5df3808dc.png)

Dagster 作业执行

**结论**

*   我们简要介绍了什么是 Dagster，以及如何使用 Dagster 实现数据管道的自动化。
*   我们展示了如何创建操作符和作业，以及如何定义每个操作符任务的运行顺序。
*   我们使用 Dagster、Python、Pandas 和 SQLAlchemy 实现了 ETL(提取、转换和加载)管道。
*   完整的代码可以在[这里](https://github.com/hnawaz007/pythondataanalysis/tree/main/Dagster/etl)找到