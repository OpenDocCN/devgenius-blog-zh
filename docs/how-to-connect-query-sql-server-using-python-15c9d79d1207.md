# 如何使用 Python 连接和查询 SQL Server

> 原文：<https://blog.devgenius.io/how-to-connect-query-sql-server-using-python-15c9d79d1207?source=collection_archive---------3----------------------->

使用 Python、SQL Server

![](img/cda5d628e28a53cf485c98ecb4387c54.png)

Python SQL Server 连接

今天我将讲述如何使用 python 连接到 SQL 数据库。这是在 [ETL 系列](https://medium.com/dev-genius/how-to-build-an-etl-pipeline-with-python-1b78407c3875)中出现的一个常见问题。所以，我决定报道它，如果观众面临连接问题，就把他们引向它。

此 SQL Server 安装程序将使您能够:

*   通过 Python 建立与 SQL Server 数据库的连接
*   查询 SQL Server 数据库
*   检索数据并保存到数据帧中

完整的代码可以在 [GitHub](https://github.com/hnawaz007/pythondataanalysis/blob/main/ETL%20Pipeline/Connect%20to%20SQL%20Server%20with%20Python.ipynb) 上找到。相关视频教程可在 [YouTube](https://www.youtube.com/watch?v=zdezE6TWSQQ) 上找到。

这个前提条件很少。我们需要安装和配置 SQL Server 数据库。我已经在这里报道了这个[。此外，我们将使用 Jupyter 笔记本来排除数据库连接故障。Jupyter 笔记本电脑的安装包含在此处](https://www.youtube.com/watch?v=e5mvoKuV3xs)的视频[中。](https://www.youtube.com/watch?v=B0G-44dqHRM&t=0s)

对于 SQL Server，我们使用 SQL Server Native Client 11。我们可以从这个[链接](https://www.microsoft.com/en-us/download/details.aspx?id=36434)下载。只需下载安装程序，用默认设置安装即可。我们可以检查注册表编辑器，以确保安装了 SQL Server Native Client。注册表项将位于以下位置

```
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Native Client 11.0\CurrentVersion
```

如果设置完成，那么让我们启动一个 Jupyter 笔记本。为了连接到 SQL Server 数据库，我们使用 pyodbc 库。我们可以使用 pip 命令安装它。

```
Pip install pyodbc
```

这将在我们的机器上安装库。现在，我们可以将它导入到笔记本中。此外，让我们导入 pandas 库，这样我们就可以读取数据并将其保存在一个类似表格的结构中。

```
import pyodbc
import pandas as pd
```

我们定义保存数据库凭证和细节的变量。我们使用 pyodbc 中的 connect 函数来建立数据库连接。

```
#database username. Use config file or env vars
uid = "etl" 
#database password. Use config file or env vars
pwd = "demopass" 
driver = "{SQL Server Native Client 11.0}"
server = "localhost" # or your computer name
database = "AdventureWorksDW2019;"
```

在 connect 函数中，我们连接变量来创建一个连接字符串。因为我们安装了 SQL Server Express 版本，所以它有一个默认的实例名 *SQLExpress* 。如果您有普通版或开发版，那么您不必在服务器名称后定义实例。

```
conn = pyodbc.connect('DRIVER=' + driver + ';SERVER=' + server + '\SQLEXPRESS' + ';DATABASE=' + database + ';UID=' + uid + ';PWD=' + pwd)
```

我们需要一个 SQL 查询来测试我们与上述 SQL Server 数据库的连接。让我们发出一个简单的 select 语句，从数据库中检索一些数据。

```
sql = "SELECT * FROM [dbo].[DimProductCategory]"
```

现在我们可以使用 pandas read SQL 查询函数来查询数据库。我们拥有这项功能所需的所有组件。我们为它提供 SQL 变量；这是我们保存在变量中的 SQL 查询和数据库连接。

```
df = pd.read_sql_query(sql, conn)
df.head()
```

我们用 shift+enter 键执行笔记本单元格。如果这个单元执行时没有错误，那么恭喜您，您的设置已经完成，您可以继续构建 etl 管道了。

最常见的错误是缺少 Native client。请确保它已安装。其次将 TCP/IP 协议禁用。要解决这个问题，我们可以打开 SQL Server 配置管理器并选择网络配置。确保 TCP/IP 协议已启用。如果没有，那么右击它并启用它。进行此更改后，请确保重新启动 SQL Server 服务。

**结论**

*   我们讲述了如何使用 Python 建立到 SQL Server 数据库的连接。
*   我们定义数据库细节并通过 pyodbc 建立连接。
*   我们查询了数据库，并以熊猫数据框的形式检索了数据。
*   完整的代码可以在[这里找到](https://github.com/hnawaz007/pythondataanalysis/blob/main/ETL%20Pipeline/Connect%20to%20SQL%20Server%20with%20Python.ipynb)