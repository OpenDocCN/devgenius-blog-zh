# 从本地 SQL Server 中提取数据，并使用 Airflow 将其加载到 Google 的 BigQuery 中

> 原文：<https://blog.devgenius.io/extract-data-from-on-premise-sql-server-and-load-it-to-googles-bigquery-with-airflow-6887cbf7802b?source=collection_archive---------3----------------------->

使用 Airflow、BigQuery、Python、SQL Server

![](img/8345b2b6f4377375636eeceedb87645c.png)

带气流的 ETL

今天，我们将开发一个 ETL 管道，它将在本地 SQL Server 和 Google 的 BigQuery 数据库之间移动数据。我们将使用 Apache Airflow 来协调这条管道。在此之前，我们已经在一系列文章中介绍了如何用 Python 开发 ETL 管道。这是一个使用 Apache Airflow 从 SQL Server 提取数据并加载到 Google cloud 中的 BigQuery 的请求。

我们将从这个会话的[中借用代码，并使其与 BigQuery 兼容。我们已经在本课程](/how-to-build-an-etl-pipeline-with-python-1b78407c3875)[中讲述了 SQL server 安装和数据库恢复。以防你想设置它。](https://www.youtube.com/watch?v=e5mvoKuV3xs)

如果你是视觉学习者，那么我在 [YouTube](https://www.youtube.com/watch?v=Nq8Td8h_szk.) 上有一个附带的视频，里面有完整代码的演示。

在之前，我们已经讨论了[阿帕奇气流，所以让我们直接进入 Dag。](/how-to-automate-etl-pipelines-with-airflow-62484ee5ef4c)

**气流 DAG**

我们将重构我们的 Python ETL 管道脚本，使其与 Airflow 兼容。除了我们的常规编程库，我们还将导入那些特定于 Airflow 的库(DAG、task 和 TaskGroup)。我们包含了一个来自 Google 的服务帐户，使用 Google cloud 的服务帐户来验证 BigQuery。另一个重要的库是 airflow.providers 的 MsSQLHook，我们将使用它来查询 SQL Server。我们在*管理>连接*下定义 SQL Server 的连接。我们将使用钩子来调用这个连接。

```
from airflow.decorators import dag, task 
from airflow.providers.microsoft.mssql.hooks.mssql import MsSqlHookfrom google.oauth2 import service_account
```

在 DAG 的功能下，我们定义我们的任务。在第一个任务 SQL 下划线提取中，我们在 hook 的帮助下获得了 SQL Server 的连接。我们将 SQL server 系统模式中的表名放入数据帧中。该任务以字典的形式返回表名。

检索 SQL Server 表

在第二个任务中，gcp 下划线 load。我们在服务帐户的帮助下定义 Google cloud 的凭证。我们从 json 文件中获取凭证。不要担心，我会告诉你如何生成这个文件以及谷歌云中的服务帐户。确保该文件在运行 Airflow 的系统上。我的 Airflow 实例运行在 docker 容器上，我已经将文件复制到容器中。让我们用谷歌云的信息再定义几个变量。我们需要我们的谷歌帐户的项目 id 以及数据集名称。

我们从传入的字典中获取表名，并用带有 *f* 字符串的表名开发一个 SQL 查询。我们检索 SQL Server 的连接，并将数据从 SQL Server 读入数据帧。我们使用 to_gbq 函数将数据帧持久化到 BigQuery。这个函数需要一些参数，比如目标表。这是数据集和表名"*" AdventureWorks。DimSalesTerritory* ”。我们在表名前面添加了 *src* ，因此实际名称将是*src _ dimsalestirect*。然后我们向它提供项目 id 和凭证。因此，该脚本可以定位我们的帐户，并有权限保存这些数据。如果这个表存在，我们就替换它。这是一个截断和加载 ETL 场景。

读取数据并将其加载到 BigQuery 中

让我们继续谷歌云设置。我们将需要一个谷歌帐户，所以一定要注册一个帐户。我们可以注册一个免费账户来熟悉谷歌的云产品。这使我们能够访问 BigQuery 和存储层。我们需要创造一些资源。首先是一个数据集，因此我们可以在其中创建表。

一旦启动 bigquery，您将在资源管理器下看到您的项目。最初它将是空的。我们可以单击项目右侧的省略号，并创建一个新的数据集。

![](img/361263bc31ea7e40211757e224cf8776.png)

项目下的 BigQuery 数据集

我们为此数据集提供一个名称，并选择一个位置。让我们选择*美国多个地区*。单击*创建数据集*按钮，我们将在项目下创建一个新的数据集。我们可以在代码中引用这个数据集，并在它下面创建表。

![](img/c9ebc4e84e145bc7fd29d5fa502c4d44.png)

创建新数据集

要获取项目详细信息，我们单击右上角的省略号，然后单击项目设置。这将显示项目 id。记下项目 id。我们的剧本需要这个。

![](img/47fa5e91169c9d9fbe7787f869aeafdd.png)

项目设置

我们需要的第三个资源是服务帐户。因此，让我们导航到 IAM & Admin 并选择服务帐户。我已经有一个了，但是让我们单击创建一个新的服务帐户。

![](img/1a205eba909e25782832c110fb9bd7c8.png)

IAM 服务帐户

我们将为此帐户提供一个名称。谷歌会自动给它分配一个 id。让我们也为该帐户提供一个描述，然后单击“创建并继续”。

![](img/b7967680912d14dbcb1cea1d0e9f3aff.png)

创建服务帐户

接下来，我们需要授予该帐户权限。因为我们主要关注 bigquery。我们将为它分配 bigquery 权限。我们搜索角色并将它们添加到此帐户。我们向该帐户添加了 Bigquery 管理员、bigquery 连接用户、bigquery 数据编辑器、bigquery 连接管理员角色。分配权限后，让我们单击继续并完成。

![](img/f2979a539ae5041a73ef9ac3af29915e.png)

分配角色和权限

服务帐户已创建。目前没有与此帐户相关联的密钥。现在我们可以为这个帐户生成密钥。这将为我们提供 json 文件，其中包含连接到 google cloud 所需的凭证。因此，我们将单击“actions”下的下拉菜单，选择“manage keys”。

![](img/b37f6952510b172d051fec543cf4386f.png)

创建访问键

在下一页中，我们单击*下的下拉列表，添加密钥*并选择创建新密钥。我们将在 json 中生成密钥。这将创建密钥并以 json 格式下载它们。请确保对此保密，因为它包含您的 google cloud 帐户凭据。

![](img/52ce3b026b0e0e08362f62a7992afc7b.png)

生成私钥

我们将这个文件复制到我们的 airflow 实例或者您编写 python 脚本的地方。为了连接到 google 云服务，它需要访问这个文件。现在我们准备将数据加载到 google bigquery 中。

让我们导航到 Airflow 服务器 url。我们的 DAG 中有两个任务。第一个任务从 SQL Server 获取表，第二个任务读取数据并将其加载到 BigQuery 中。让我们启动匕首。我们的 Dag 正在执行。它从 SQL server 中提取数据，并将数据加载到 BigQuery 数据库中。我们可以单击一个任务并查看它的日志。我们成功地将数据加载到 BigQuery。让我们导航到 bigquery 并刷新浏览器，看看这个表是否出现在我们的数据集中。页面加载后，我们将扩展数据集。我们的数据集中有一个新的 src 下划线 DimSalesTerritory。让我们查询这个表。我们将从该表中选择所有列并执行该查询。该查询返回数据。

我们已经成功地将内部 SQL Server 数据加载到 Google 的 BigQuery 数据库中。我们可以用一个 Dag 处理多个表，或者为每个表构建一个 Dag 来处理从源到目的地的数据。

还有一个建议，我们可以将数据加载到 google storage 中的一个桶中，这样如果任何其他服务或进程需要这些数据，它们可以从桶中访问这些数据，然后您可以将这些数据加载到 bigquery 中。

**结论**

*   我们开发了一个 Airflow DAG 来从 SQL Server 提取数据，并将其加载到 Google BigQuery。
*   我们展示了如何创建任务并定义每个任务的运行顺序。
*   我们使用 Airflow、Python 和 Pandas 实现了一个 ETL(提取和加载)管道。
*   完整的代码可以在[这里](https://github.com/hnawaz007/pythondataanalysis/blob/main/ETL%20Pipeline/gpc_extract_and_load.py)找到