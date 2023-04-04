# 时间序列数据的 Postgres

> 原文：<https://blog.devgenius.io/postgres-for-time-series-data-8ec3a849a14?source=collection_archive---------5----------------------->

## 如何用 Docker 建立时间刻度数据库？

![](img/4c4d409a8e8cdced1ab65524008fa15f.png)

由[乌列尔 SC](https://unsplash.com/@urielsc26?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**TimescaleDB** 是一个开源的时序数据关系数据库。它使用完整的 SQL，但是以以前为 NoSQL 数据库保留的方式扩展。

在本文中，我们将介绍时间刻度数据库。我们还将向您展示如何使用 Docker 轻松建立时间刻度数据库。这些步骤如下:

1.  **时标基础知识**
2.  **技术要求**
3.  **带 Docker 的时标 db**
4.  **例子** **用 Python**
5.  **结论**
6.  **有用链接**

# 🤔时标 b 基础

TimescaleDB 使用 PostgreSQL 和最大的开发人员和管理工具生态系统。此外，TimescaleDB 的查询速度比 PostgreSQL、InfluxDB 和 MongoDB 快 10-100 倍。您可以每秒写入数百万个数据点，在单个节点上存储数百 TB，在多个节点上存储数 Pb。

## 什么是时间序列数据？

时间序列数据是总结系统、过程或行为如何随时间变化的数据。这些数据具有以下特征:

*   以时间为中心的:数据记录总是有一个时间戳
*   **仅追加**:数据几乎完全是仅追加的
*   **最近的**:新数据通常是关于最近的时间间隔

与标准关系数据等其他数据相比，时间序列数据的一个关键区别在于，对数据的更改是插入，而不是覆盖。

你会在不同的领域发现时间序列，例如在计算机系统监控、金融交易系统、物联网和商业智能环境中。

## 时标的主要概念 b

**超表**是 PostgreSQL 表，具有处理时间序列数据的特殊功能。您可以在同一个数据库中使用 hypertables 和常规 PostgreSQL 表。为时间序列选择超表，为关系数据选择常规 PostgreSQL 表。

为了更好地说明这一点，举个例子:要记录一段时间内的股价，可以使用一个超表和一个常规表来记录每只股票的股票代码和名称。

如果对技术实现感兴趣，可以在 [Hypertable 和 chunk architecture](https://docs.timescale.com/timescaledb/latest/overview/core-concepts/hypertables-and-chunks/hypertable-architecture/) 下深入研究。

# ✅技术要求

您将需要以下先决条件:

*   您的机器上必须安装最新版本的 Docker。如果您尚未安装，请按照[说明](https://docs.docker.com/get-docker/)进行操作。
*   您的计算机上必须安装最新版本的 Docker Compose。请遵循[的指示](https://docs.docker.com/compose/install/)。
*   访问 bash (macOS、Linux 或 Windows)。
*   一个你选择的 Python 包管理器，像[康达](https://docs.conda.io/en/latest/miniconda.html)

# 带 Docker 的🖥时标 b

首先，您应该检查是否正确安装了 Docker 和 Docker Compose。打开您选择的终端并输入以下命令:

```
$ docker --version
# Example output: $ Docker version 20.10.21
```

如果安装正确，则输出 Docker 版本。您可以为您的 Docker Compose 安装检查相同的内容。

```
$ docker-compose --version
# Example output: $ Docker Compose version v2.12.2
```

是啊。一切正常。现在，我们可以从 Docker 组合栈开始。

## 使用 Docker 编写进行设置

有几种[方法](https://docs.timescale.com/install/latest/)来设置时间刻度 DB。在本文中，我们将重点关注基于容器的选项。堆栈包含两个服务， [TimescaleDB](https://hub.docker.com/r/timescale/timescaledb) 和 [Adminer](https://hub.docker.com/_/adminer/) 。Adminer 是一个全功能的数据库管理工具。

**Docker 合成堆栈(docker-compose.yml 文件):**

```
version: "3.8"

services:
  timescaledb:
    image: timescale/timescaledb:latest-pg14
    container_name: timescale
    hostname: timescaledb
    restart: always
    ports:
      - ${TIMESCALEDB_PORT}:5432
    volumes:
      - ./${TIMESCALEDB_DATA_STORE}:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: ${TIMESCALEDB_PASSWORD}
      POSTGRES_USER: ${TIMESCALEDB_USER}
      POSTGRES_DB: ${TIMESCALEDB_DB}
  adminer:
    image: adminer:4.8.1
    container_name: adminer
    restart: always
    ports:
      - ${ADMINER_PORT}:8080
```

建议将变量分成一个。环境文件。它导致一个更好的概述。

**。环境文件:**

```
# timescaledb
TIMESCALEDB_PORT=5432
TIMESCALEDB_DATA_STORE=timescaledb/
TIMESCALEDB_PASSWORD=timescaledb
TIMESCALEDB_USER=timescale
TIMESCALEDB_DB=timescale_database

# adminer
ADMINER_PORT=8087
```

打开您选择的终端。您可以使用以下命令启动 docker-compose 堆栈:

```
$ docker compose up -d
```

标志-d 意味着容器作为守护进程运行。在这种模式下，终端不输出任何日志。您可以使用以下命令查看特定容器的日志:

```
$ docker compose logs --follow <container_name>
```

日志显示容器的状态。如果一切正常，那么您可以将 TimescaleDB 与 Adminer 一起使用。

您可以通过位于 localhost:8087 的 Adminer UI 登录。您可以在。env 文件(见上文)。

![](img/7f829b115f2dcceb4ded3e9f5c87c41d.png)

管理员—登录菜单

# 🤓**例子** **用 Python**

以下解释基于来自 TimescaleDB 的[快速入门 Python 示例](https://docs.timescale.com/timescaledb/latest/quick-start/python/)。

## 连接到数据库

以下 Python 代码建立了与数据库的连接。

```
import psycopg2

# Structure of the connection string: 
# "postgres://username:password@host:port/dbname"
CONNECTION = "postgres://timescale:timescaledb@localhost:5432/timescale_database"
conn = psycopg2.connect(CONNECTION)
```

## 创建表格

现在，我们创建一个名为**传感器**和列 *id* 、*类型*和*位置*的关系表。

```
# create relational table
query_create_sensors_table = "CREATE TABLE sensors (id SERIAL PRIMARY KEY, type VARCHAR(50), location VARCHAR(50));"

cursor = conn.cursor()
cursor.execute(query_create_sensors_table)
conn.commit()
cursor.close()
```

我们还创建了一个名为 **sensor_data** 的超表。请注意，超表包含强制时间列。接下来，用 SELECT 语句将表 **sensor_data** 转换成超表。注意，您需要将超表的表名和时间列的名称指定为两个参数。最后，您必须提交您的更改并关闭光标。

```
# create sensor data hypertable
query_create_sensordata_table = """CREATE TABLE sensor_data (
                                        time TIMESTAMPTZ NOT NULL,
                                        sensor_id INTEGER,
                                        temperature DOUBLE PRECISION,
                                        cpu DOUBLE PRECISION,
                                        FOREIGN KEY (sensor_id) REFERENCES sensors (id)
                                        );"""

query_create_sensordata_hypertable = "SELECT create_hypertable('sensor_data', 'time');"

cursor = conn.cursor()
cursor.execute(query_create_sensordata_table)
cursor.execute(query_create_sensordata_hypertable)
conn.commit() # commit changes to the database to make changes persistent
cursor.close()
```

## 插入数据

在下面的例子中，我们将名为 ***传感器*** 的元组列表插入到名为**传感器**的关系表中。

```
 # insert rows into TimescaleDB
SQL = "INSERT INTO sensors (type, location) VALUES (%s, %s);"
sensors = [('a', 'floor'), ('a', 'ceiling'), ('b', 'floor'), ('b', 'ceiling')]
cursor = conn.cursor()
for sensor in sensors:
    try:
        data = (sensor[0], sensor[1])
        cursor.execute(SQL, data)
    except (Exception, psycopg2.Error) as error:
        print(error.pgerror)
conn.commit()
```

使用 psycopg2 就足以将行插入到超表中。然而，为了更快的性能，我们使用了 *pgcopy* 。为此，安装带有 pip 的 pgcopy，并将其作为导入指令添加。

```
# install pgcopy
pip install pgcopy
```

现在，我们将时间序列数据插入到超表中。

```
 from pgcopy import CopyManager

cursor = conn.cursor()

# for sensors with ids 1-4
for id in range(1, 4, 1):
    data = (id,)
    # create random data
    simulate_query = """SELECT generate_series(now() - interval '24 hour', now(), interval '5 minute') AS time,
                       %s as sensor_id,
                       random()*100 AS temperature,
                       random() AS cpu
                    """
    cursor.execute(simulate_query, data)
    values = cursor.fetchall()

    # column names of the table you're inserting into
    cols = ['time', 'sensor_id', 'temperature', 'cpu']

    # create copy manager with the target table and insert
    mgr = CopyManager(conn, 'sensor_data', cols)
    mgr.copy(values)

# commit after all sensor data is inserted
# could also commit after each sensor insert is done
conn.commit()
```

最后，我们检查管理员是否一切正常。在第一个截图中，您可以看到添加了数据的**传感器**表。

![](img/2ee4c66aa110bca4d0d226d201253898.png)

管理员-传感器表

第二个截图显示了添加了时间序列数据的 **sensor_data** hypertable。

![](img/af6e03a85dd046cbec44a54ac30b5bd1.png)

管理员—传感器数据超表

# 🎬结论

在本文中，我们看到了如何用 TimescaleDB 和 Adminer 设置 Docker 堆栈。在这种情况下，我们学习了如何连接到时间序列数据库以及如何插入时间序列数据。TimescaleDB 的优点是可以为关系数据和时间序列数据创建表。

你喜欢这篇文章吗？成为[中等会员](https://tinztwins.medium.com/membership)继续无限学习。当您使用该链接时，我们将收取一小部分会员费。您无需支付额外费用。

**不要错过我们接下来的故事:**

[](https://tinztwins.medium.com/subscribe) [## 每当 Tinz Twins 发布时收到一封电子邮件。

### 每当 Tinz Twins 发布时收到一封电子邮件。注册后，如果您还没有，您将创建一个中型帐户…

tinztwins.medium.com](https://tinztwins.medium.com/subscribe) 

非常感谢你的阅读。如果你喜欢这篇文章，请随意分享。关注我们了解更多内容。祝你今天过得愉快！

![](img/1924008e75e30f4baed4775b04724600.png)

# 🔍有用的链接

*   [文档时间表 B](https://docs.timescale.com/timescaledb/latest/overview/)
*   [快速入门:Python 和 TimescaleDB](https://docs.timescale.com/timescaledb/latest/quick-start/python/)
*   管理员