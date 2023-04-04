# 从 Excel 到 PostgreSQL 数据库的数据流。Pyspark

> 原文：<https://blog.devgenius.io/dag-flow-from-excel-to-postgresql-db-with-pyspark-293eb203b2a0?source=collection_archive---------2----------------------->

# **目标:**

使用 Airflow scheduler 提供了三条到 PostgreSQL 数据库的路径。
探索这些路线的选择和挑战。

1) PostgreSQL 运算符，在 Airflow 中内部建立连接。

2)使用 Python 操作符和 psycopg2 库将数据写入数据库表。

3)使用通过 helper 函数调用的 pyspark 会话，然后将数据写入数据库。将使用 JDBC 驱动程序。

三者中最简单的是使用 Pyspark 会话。([更简单的选择是不使用气流。在这里了解一下](https://medium.com/@kamaljp/pyspark-pipeline-multi-table-excel-worksheet-to-ml-pipeline-and-prediction-storage-to-database-75b79b8468fe)上游任务可以将 excel 文件转换成 csv 格式并写入目标文件夹。相同的代码是[在回购](https://github.com/Kamalabot/dagflowPostgres)中共享。在你开始问之前，管道有多快？这是运行时间数据。

![](img/110e1cc21aee82f87dc0f6a463b6341d.png)

显示两个任务的 DAG 流程

上面是两个任务的时间线。t1 将 excel 文件转换为 csv 文件，并将其写入目标。t2 获取该 csv 并将其写入 PostgreSQL 服务器。该图直接取自气流。

![](img/b99ffb0605b7f0f79fbc21fb4b83bd52.png)

将 excel 文件转换为 csv

第二张图是将 excel 文件转换为 csv 文件的时间表。几乎不需要 12 秒。Pyspark 用了更多的时间。这是因为，它正在创建 spark 会话，使用 JDBC 驱动程序读取 CSV 文件并将其写入 PG 数据库。您可以看到气流调度程序记录了所有这些内容。

数据工程很复杂，就像你的生活伴侣或爱人。你需要爱它，优雅地处理它，然后带着它走向下一个复杂的问题。(数据工程师在一些公司被称为数据战士是有原因的)

但是以 pyspark 为特色的内部气流，看起来还是很复杂…告诉我们为什么要用它？

# 我为什么使用 Pyspark:

因为，PostgreSQL 操作符由于某种原因没有安装在我的本地系统中。以下是我采取的故障诊断步骤。

0.重新安装 PostgreSQL 提供程序，甚至尝试安装 Amazon PostgreSQL 提供程序。不起作用。

1.  创建了新的**虚拟环境并安装了 Airflow** 及其提供商。很顺利。

```
> python3 -m venv airflow-env
> source airflow-env/bin/activate
> pip install --upgrade pip
> pip install apache-airflow apache-airflow-providers-postgres
```

2)启动气流网络服务器。没问题，一切从零开始

3)尝试为虚拟环境内的气流设置默认数据库。*它与父文件系统中已有的气流冲突。*

4)想到用 Dockers 做气流 Web 服务器+调度器。我的 2gb 的机器卡住了，然后重新启动。

5)已检查重启是否正常工作。没有。

# **目标和失败的重要性:**

当我决定使用气流将 XL 文件中的数据移动到 PostgreSQL 数据库时，**成功条件**是 XL 数据应该在 PostgreSQL 数据库中，并且应该通过气流完成。

无论我使用 Bash 操作符、Ssh 操作符还是其他操作符，都不是一个条件。此外，我是在 PostgresOperator 失败后才知道这些运算符的。所以失败很重要。
另一个目标是我必须学习。如果 PostgreSQL 操作符起作用，我可能仍然会尝试使用 Pyspark。但不会研究如何使用其他运算符。让我们继续前进。

# 将 PG 连接到气流的命令行:

```
airflow connections add 'my_new_db' --conn-uri 'postgresql://postgres:password@127.0.0.1:5432/database
```

上述连接字符串在 Airflow 服务器和 PG 数据库以及 PostgreSQL 操作符之间创建连接。我没有使用这条路线，但是在这里分享它，因为通过 Airflow GUI 创建连接可能会令人困惑。

气流 UI，可能有点混乱。原来模式意味着表名。大多不言自明。

![](img/f6e3e18c31b0e8ac814ef328b69ea17f.png)

像 PostgreSQL / SQL 这样的连接类型会在 pip 安装完库并重启机器后出现。不仅仅是气流服务器。经过两个小时的修改，我发现了一个错误，并找到了一条替代路线

# 在进入代码之前:

在气流的情况下，还有一些额外的摔跤要做。dag 脚本必须位于 airflow 安装的 DAG 文件夹中，通常是/home/user _ { your NAME }/airflow/DAGs。然后是
气流实例。

```
> airflow db init ==> Will start the native database server and 
                      allow airflow to use it. 
> airflow webserver -p 8081 ==> Will start the Airflow Webserver where 
                          the Dags are displayed. 
> airflow scheduler ==> Will start the scheduler, which finds the airflow 
                        webserver and attaches itself (or the opposite way, 
                          not sure)
```

调度程序和 web 服务器必须相互通信。在我的系统中，调度程序一直处于离线状态。我不得不手动终止调度程序并重启它。这些日志可用于排除此类故障。

**DAG 脚本可能会有导入错误。(和 Postgres 操作符错误一样，我之前说过)**支持模块文件必须在 Dag 脚本所在的同一个文件夹中。那是在/home/user _ {你的名字}/airflow/dags/里面。

可以编写另一个 DAG，以便根据触发器仅将文件从您正在编码的文件夹复制到 airflow dags 文件夹。(一个练习可能是！！)

![](img/11248eb619553464590de38d244f1e22.png)

新的 Dag 需要几分钟才能在 Airflow 服务器上显示出来。我想数据库读取新的 dag 会有一个时间延迟，所以一直刷新页面，直到 Dag 显示出来，或者在顶部弹出一个错误。

# 气流需要功能:

我花了一些时间才意识到用于创建 dag 的 dag_script.py 是 python 脚本。变量可以像我们在 python 脚本中一样声明。请小心，因为 DAG 代码可以通过 docker 或 github repo 转移到完全不同的环境中。如果你见过 Kaggle 笔记本，那些由 dockers[https://github.com/Kaggle/docker-python](https://github.com/Kaggle/docker-python)创造的紧凑环境。

最后，一些代码…符合 dag _ scriptthat，它创建 dag(有向无环图)来移动数据。

```
import os
import warnings
warnings.filterwarnings('ignore')
from dagHelper import *
import configparser
from airflow import DAG
from airflow.utils.dates import days_ago
from airflow.operators.python import PythonOperator

#The import line creating the error is below
#from airflow.providers.postgres.operators.postgres import PostgresOperator

#The import functions and variables declations are not shared for brevity
with DAG(
        dag_id='Excel_CSV_Spark_writer',
        default_args=args,
        schedule_interval='0 0 * * *',
        start_date=days_ago(2)
) as dag:
    t1 = PythonOperator(
            task_id='t1',
            python_callable=transformXL,
            op_kwargs={
                'fileLocation':sourceName,
                'fileDestination':newDest,
                'worksheet':'StoreData'
                }
            )

    t2 = PythonOperator(
            task_id='t2',
            python_callable=transformDB,
            op_kwargs={
                'fileLocation':newDest,
                'tableName':tableName,
                'config':config}
        )
    t1 >> t2
if __name__ == "__main__":
    dag.cli()
```

这看起来很简单。Python-callable 是隐藏在 dagHelper.py 文件/模块中的函数。这就是两个主要的支持功能。光彩照人。

```
 import pyspark
from pyspark.sql import SparkSession
from pyspark.sql.functions import *
import pandas as pd
#transformation function
def transformXL(fileLocation,fileDestination,worksheet = 'DataSource'):

    sourceDF = pd.read_excel(fileLocation,worksheet,header=0)

    frameCols = sourceDF.columns
    dropColumns = []

    for x in frameCols:
        if x.split(':')[0] == 'Unnamed':
            dropColumns.append(x)
    sourceDF.drop(dropColumns,inplace=True,axis=1)

    #Formatting the columns headers to 
    frameCols = sourceDF.columns
    cols = []
    for x in frameCols:
        temp = x.replace(" ",'_')
        temp = temp.replace("-","_")
        temp = temp.replace(":","_")
        cols.append(temp)

    sourceDF.columns = cols

    sourceDF.to_csv(fileDestination,index=False)

def transformDB(fileLocation,tableName,config):
    #Starting spark with database connectivity
    #database connection data
    db ='dabases' 
    user ='postgres' 
    passwd = password 
    port = 5432
    host ='localhost' 

    spark = SparkSession.builder.appName("KPI"). \
            config('spark.jars','/usr/share/java/postgresql-42.2.26.jar'). \
            getOrCreate()
    sparkread = spark.read
    sparkcon = spark.sparkContext

    sourceDF = sparkread.csv(fileLocation,inferSchema=True,
                            header=True)
    print(f"jdbc:postgresql://{host}:{port}/{db}")
    sourceDF.write.format("jdbc") \
        .option("url",f"jdbc:postgresql://{host}:{port}/{db}") \
        .option("dbtable",f"{tableName}") \
        .option("user",f"{user}") \
        .option("password",f"{passwd}") \
        .option("driver","org.postgresql.Driver") \
        .save(mode='overwrite')
```

# 嘿，它们只是 python 函数:

这就是我写了自己的气流 DAGs 后的感受。在气流的情况下，DAG 在服务器中运行，它与本地
文件系统进行文件通信，并从模块中读取函数。单任务 dag 很容易，但是 Pyspark 环境下的 DAG 写入数据库要花很多时间。因为我没有给出正确的数据库表名。

有些错误的发生是因为我们不知道或不记得一个概念。当多个系统交互时，理解与服务器、文件系统、spark 上下文和数据库服务器相关的概念是很重要的。否则，理解错误将成为一个挑战。更糟的情况会阻止我们前进。这就是所有的人…