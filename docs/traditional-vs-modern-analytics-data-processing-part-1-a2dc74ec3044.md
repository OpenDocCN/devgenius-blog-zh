# 传统与现代分析数据处理(第一部分)

> 原文：<https://blog.devgenius.io/traditional-vs-modern-analytics-data-processing-part-1-a2dc74ec3044?source=collection_archive---------6----------------------->

文章首发@ own your data . ai。

在过去的几年里，我们见证了数据领域工具的爆炸式增长，称为“现代数据堆栈”，专业人士正在选择他们的立场，要么指向传统方法的封闭来源，要么指向现代方法的缺失部分。在本文中，我将详细介绍这些差异，作为一名数据工程师，我认为这些差异是最重要的:写错误与读错误和静默错误。

![](img/65faf4f787dc75657d8122c5c13b312a.png)

从概念上讲，我们处理数据的方式没有任何改变。无论是传统方式还是现代方式，我们在一个分析平台中都有几乎相同的数据层:

![](img/9373f82991b460cc4d901b386e20b676.png)

在哪里，

*   数据源是您想要从中获取数据的任何操作系统或第三方应用程序。您可以通过 API、事件流、批量加载、CDC 等检索数据；
*   着陆区或数据湖，你的分析平台的入口；从源系统检索的数据以*文件* *格式*1:1 放置的区域；
*   raw/interface 层是来自源系统的数据再次 1:1 放置的区域，但是这里应该实现检查(有时直接从 CDC 加载)；
*   集成层是集成发生的地方:例如创建实体解析、进行参考数据管理或任何其他与您的业务案例相关的集成；
*   消费层，我只把它放在一个盒子里:它是报告层(语义/度量层)，你的用户与之交互的层。为什么不直接使用集成层？因为那一层遵循技术(比如历史)和功能需求，而消费层只遵循功能(业务)。

从着陆区到原始层再到集成层再到消费层，这就是我们所说的数据转换管道(ETL ),基本上就是奇迹发生的地方:在这个数据流程中，我们实施转换，并确保我们读取和写入的内容具有良好的质量水平。为什么这么多层？因为我们想要一个数据非易失性的(它不会改变)、时变的(它是有版本的)、面向主题的(我们谈论客户而不是处理他们的应用程序)和集成的(我们连接来自多个系统的数据)的领域。

从处理的角度来看，这是传统方式与现代方式之间最大的争议。在传统方式中，当出现问题时(写错误)，我们停止加载数据，而在现代方式中，我们加载数据，然后检查是否有问题(读错误)。许多现代工具以速度更快而自豪，但它们推动了读取类型处理的错误，这意味着我们只在数据已经被业务用户或运营应用程序使用后才听到或观察到数据有问题。

现在，经过多年的随叫随到和每天修复数据，我对读取错误非常怀疑。我认为这是其他数据专业人士反对现代堆栈的原因。但是，虽然我同情他们，但我确实认为两个世界的结合比其中任何一个单独存在都要好。让我们想象一个世界，在这个世界中，我们在写入时有错误，但我们也有可观察性——这是任何操作的梦想！

数据管道中的错误通常是因为数据质量问题。它的最大挑战是它的高分类级别:对于一个业务用例来说，一个记录或字段可能质量不好，但是对于另一个业务用例来说，情况是否也是如此呢？因此，如果数据没有通过一个业务用例的检查，您会停止它还是让它流动？谁来决定呢？当然，不是今天的行动风云人物！

# 设置

为了展示写错误和读错误之间的区别，我将使用一个简单的标准用例:有订单的客户。

![](img/1e336fc25b6a61101833f06fb45d228f.png)

让我们先看看集成层中的表。我们看到，在 dim_customers 中，我们有 3 组密钥，它们是什么？

*   PK:主键，它在整个表中是惟一的，用于检索单个记录
*   SK:代理键，它在集成层中唯一地标识一个客户
*   NK:自然键，它在数据源层唯一地标识一个客户(因此 SK 是 NK 和源系统的组合)

根据订单信息，我们将创建一个销售事实表，其中包含日和客户级别的聚合指标。这都是假数据，所以忽略噪音！

至于技术设置，我会试用 Mage.ai，一个专门用于 E2E 数据处理的工具。它有一个相当酷的用户界面，所以让我们看看你还可以用它实现什么。我还将使用 Postgres 15.0，因为它是最近发布的，并且附带了合并操作，需要缓慢变化的维度(在本文的第 2 部分中)。

# 初始负载

在 Faker 和 random 的帮助下，我生成了一些虚拟数据:

```
def generate_random_customer():
    records = []
    faker = Faker()
    for i in range(1, 10):
        records.append([
            i,
            faker.email(),
            faker.first_name(),
            faker.last_name()
        ])
    df = pd.DataFrame(records, columns=['id', 'email_address', 'first_name', 'last_name'])
    return df def generate_random_orders():
    records = []
    faker = Faker()
    for i in range(1, 30):
        records.append([
            i,
            faker.date_time_between('-10d', '-1d').strftime("%Y-%m-%d %H:%M:%S"),
            random.randint(1, 11),
            random.choice(['NEW', 'DELIVERED']),
            random.randrange(10000) / 100,
            'EUR',
            [{'product_id': i, 'amount': random.randint(1, 3)}
                for i in random.sample(range(10, 30), random.randint(1, 4))]
        ])
    df = pd.DataFrame(records, columns=[
        'id', 'order_timestamp', 'customer_id', 'order_status', 'order_amount', 'order_currency', 'products'
    ])
    return df
```

我运行`docker-compose up`,开始了:我们运行了 Mage，我可以在我的管道中使用上面的代码:

```
from pandas import DataFrame
from demo_error_read_write.data_setup.generate_fake_data import generate_random_ordersif 'data_loader' not in globals():
    from mage_ai.data_preparation.decorators import data_loader
if 'test' not in globals():
    from mage_ai.data_preparation.decorators import test @data_loader
def load_data(**kwargs) -> DataFrame:
    """
    Template code for loading data from any source. Returns:
        DataFrame: Returned pandas data frame.
    """
    # Specify your data loading logic here return generate_random_orders()
```

在 Mage 的 UI 中，我可以在执行时观察我的数据信息:

![](img/bda30156f0104242b9a37eb6b285ab8d.png)

# 从原始数据到原始数据，什么是数据契约？

如果您在数据领域，您可能已经听到人们谈论数据契约。对于在这个领域工作了相当长时间的人来说，数据契约也不是什么新鲜事。我们过去称之为数据交付协议，因为大多数交付都是文件交付，成批交付。我们有 Word 文档(我不是在开玩笑！)解释每个文件结构、字段，提供可以在模式中配置的所有属性(长度、为空性、键或链接等)。

那里发生了什么？随着 ELT 的兴起，这样的契约不再实现:每个人都将数据倾倒在数据湖中，由数据团队来决定模式是什么以及如何处理数据。这就是为什么你会看到像 AWS Glue Crawler/Pandas/Spark/dbt(在刷新模式下)这样的工具自己进行模式发现。这一点都不坏，我认为它们是强大的工具，当涉及到实验或从不可信的第三方加载数据时。但是，当我们谈论集成来自一个公司可能拥有的系统的数据时，当这些系统最了解他们的数据时，为什么不与他们讨论并达成协议呢？

长话短说，看起来数据契约正在重返这个世界，我现在比以往任何时候都更喜欢它:我们在分析平台上消费的资源如今都有开放的模式(REST、GraphQL API 或事件流)。但是模式定义足以保证数据质量吗？在业务定义中，您的数据的功能特征是什么，或者您的数据要转换成什么？想象一个字段上的可接受的值列表或两个表之间缺少的键。我不会在这里撒谎，无论是以传统的方式，还是以现代的方式，在加载数据之前没有人实现这些，因此我将只查看与模式相关的检查。

我们的来源是一个运营系统，在其中创建客户和管理订单。操作系统对用户输入的数据进行验证:每个字段都有一个定义，在保存数据时进行验证。虽然运营系统在我们的环境中是值得信任的一方，但任何 IT 系统的规则都是“不要相信任何人”,数据验证应该在输入时进行。让我们来看一下数据的定义:

![](img/e865d9adaf06e425eb2b56188505069d.png)

# 模式自动发现

让我们看看熊猫和 spark 生成的是什么模式。首先使用 Mage 中的数据提取器步骤将虚拟数据加载到 txt 文件中:

```
from mage_ai.io.file import FileIO
from pandas import DataFrame
import osif 'data_exporter' not in globals():
    from mage_ai.data_preparation.decorators import data_exporter @data_exporter
def export_data_to_file(df: DataFrame, **kwargs) -> None:
    """
    Template for exporting data to filesystem. Docs: https://github.com/mage-ai/mage-ai/blob/master/docs/blocks/data_loading.md#fileio
    """ filepath = os.getenv('DATA_OUTPUT_LANDING_ZONE')
    filepath = f"{filepath}/customers.txt"
    FileIO().export(df, filepath, 'csv', **{'sep': '\t'})
```

## 熊猫

为了将 pandas 加载到 Postgres，我使用了来自 Mage 的数据导出器步骤:

```
from mage_ai.data_preparation.repo_manager import get_repo_path
from mage_ai.io.config import ConfigFileLoader
from mage_ai.io.postgres import Postgres
from pandas import DataFrame
from os import pathif 'data_exporter' not in globals():
    from mage_ai.data_preparation.decorators import data_exporter @data_exporter
def export_data_to_postgres(df: DataFrame, **kwargs) -> None:
    """
    Template for exporting data to a PostgreSQL database.
    Specify your configuration settings in 'io_config.yaml'. Docs: https://github.com/mage-ai/mage-ai/blob/master/docs/blocks/data_loading.md#postgresql
    """
    schema_name = 'postgres'  # Specify the name of the schema to export data to
    table_name = 'raw_customers_pandas'  # Specify the name of the table to export data to
    config_path = path.join(get_repo_path(), 'io_config.yaml')
    config_profile = 'default' with Postgres.with_config(ConfigFileLoader(config_path, config_profile)) as loader:
        loader.export(
            df,
            schema_name,
            table_name,
            index=False,  # Specifies whether to include index in exported table
            if_exists='replace',  # Specify resolution policy if table name already exists
        )
```

数据加载后，我可以在 Pandas 和 Postgres 中检查模式的外观。为此，我使用了 Mage 中可用的 Scratch 文件，我觉得这是一个很酷的特性，因为我不需要去我的终端:

```
import pandas as pd
import os
from sqlalchemy import create_enginefilepath = os.getenv('DATA_OUTPUT_LANDING_ZONE')
filepath = f"{filepath}/customers.txt"df = pd.read_csv(filepath, sep='\t')print('Pandas dataframe')
print(' ')
df.info(verbose=True)conn = create_engine("postgresql://postgres:postgres@postgres_db:5432/postgres")df_pg_catalog = pd.read_sql("select column_name, is_nullable, data_type, character_maximum_length, character_octet_length, numeric_precision, numeric_scale \
    from information_schema.columns where table_name='raw_customers_pandas'", con=conn.connect())df_pg_size = pd.read_sql("select pg_total_relation_size(quote_ident(table_name)) as table_size \
    from information_schema.tables where table_name = 'raw_customers_pandas'", con=conn.connect())
print("              ")
print(f"As saved in the db : {df_pg_size['table_size'].values[0]} bytes")
df_pg_catalog
```

那么数据看起来怎么样呢？

![](img/50a92ee00e5c91033abc47681afa65e7.png)![](img/5930410dd5aaf9c6f5ce3dc74ed89c44.png)

我们看到所有的字符串字段都被转换成文本，customer 的 id 是 bigint，而 orders 的 id 是 smallint，尽管它们都是随机整数！

## 发动

Mage 与用于 pyspark 的 AWS EMR 集成，但对于演示，我使用本地 spark 引擎，并计划使用 delta 格式:

```
import os
from demo_error_read_write.data_setup.utils import get_delta_spark_session
from pathlib import Pathfilepath = os.getenv('DATA_OUTPUT_LANDING_ZONE')
filename_in= f"{filepath}/customers.txt"
filename_out = f"{filepath}/delta/customers"spark_session = get_delta_spark_session()df_in = spark_session.read.option(
        'delimiter', '\t').option(
        'header', 'true').option('inferschema', 'true').csv(filename_in)print(f" Schema at input: {df_in.schema}")df_in.write.mode("overwrite").format("delta").save(filename_out)
df_out = spark_session.read.format('delta').load(filename_out)print(f" Schema at output: {df_out.schema}")print(f"Size {sum(file.stat().st_size for file in Path(filename_out).rglob('*'))} bytes")
```

客户输出:

```
root
 |-- id: integer (nullable = true)
 |-- email_address: string (nullable = true)
 |-- first_name: string (nullable = true)
 |-- last_name: string (nullable = true)root
 |-- id: integer (nullable = true)
 |-- email_address: string (nullable = true)
 |-- first_name: string (nullable = true)
 |-- last_name: string (nullable = true)Size 3314 bytes
```

订单输出:

```
root
 |-- id: integer (nullable = true)
 |-- order_timestamp: timestamp (nullable = true)
 |-- customer_id: integer (nullable = true)
 |-- order_status: string (nullable = true)
 |-- order_amount: double (nullable = true)
 |-- order_currency: string (nullable = true)
 |-- products: string (nullable = true)root
 |-- id: integer (nullable = true)
 |-- order_timestamp: timestamp (nullable = true)
 |-- customer_id: integer (nullable = true)
 |-- order_status: string (nullable = true)
 |-- order_amount: double (nullable = true)
 |-- order_currency: string (nullable = true)
 |-- products: string (nullable = true)Size 5626 bytes
```

它看起来比 pandas 好一点，但是要考虑到惟一性、长度和可空性不会被自动检查。推断模式也是一个缓慢的过程，尤其是当它涉及大量数据时。

# 模式管理

既然我们已经看到了 pandas 和 spark 的自动发现模式，那么模式验证呢？我们如何确保我们收到的数据是正确的？当然，我们可以开发所有具有自动发现功能的管道，并抱以最大的希望，但是，在实践中，这并不可行，而且数据会在开发过程中遭到破坏。同时，了解自动发现是基于当前数据，有时是基于数据样本，这一点很好！

## 而不是熊猫自动发现

如果我们看看 pandas 的上下文，我的用例非常简单:我有一些数据，我想把它上传到一个数据库系统(可以是 Postgres、RedShift、Oracle、Snowflake 等)。当然，使用 pandas 我可以做一些初步的数据探索，但是为什么不利用数据库来定义我的模式呢？所有数据库系统都提供模式管理，那么为什么不使用它呢？在选择数据处理工具时，管理模式的能力应该是一个重要的因素。

```
from sqlalchemy import create_engine
import pandas as pd conn = create_engine("postgresql://postgres:postgres@postgres_db:5432/postgres")
conn = conn.connect()
filepath = os.getenv('DATA_OUTPUT_LANDING_ZONE')
filepath = f"{filepath}/customers.txt"df = pd.read_csv(filepath, sep='\t')conn.execute("drop table raw_customers_managed")conn.execute("""create table raw_customers_managed(
    id integer primary key,
    email_address varchar(320) not null unique,
    first_name varchar(100),     
    last_name varchar(100),
    inserted_datetime timestamp not null default current_timestamp 
)
""")df[['id', 'email_address', 'first_name', 'last_name']].to_sql('raw_customers_managed', con=conn, if_exists='append', index=False)df_pg_catalog = pd.read_sql("select column_name, is_nullable, data_type, character_maximum_length, character_octet_length, numeric_precision, numeric_scale \
    from information_schema.columns where table_name='raw_customers_managed' order by ordinal_position", con=conn.connect())df_pg_size = pd.read_sql("select pg_total_relation_size(quote_ident(table_name)) as table_size \
    from information_schema.tables where table_name = 'raw_customers_managed'", con=conn.connect())
print("              ")
print(f"As saved in the db : {df_pg_size['table_size'].values[0]} bytes")
df_pg_catalogfrom sqlalchemy import create_engine
import pandas as pd 
import json filepath = os.getenv('DATA_OUTPUT_LANDING_ZONE')
filepath = f"{filepath}/orders.txt"df = pd.read_csv(filepath, sep='\t')conn.execute("drop table if exists raw_orders_managed")conn.execute("""create table raw_orders_managed(
    id integer primary key,
    order_timestamp timestamp not null,
    customer_id integer not null,
    order_status varchar(10) not null,
    order_amount decimal(10,2) not null,
    order_currency varchar(3) not null,
    products jsonb,
    inserted_datetime timestamp not null default current_timestamp 
)
""")df['products'] = df['products'].apply(json.dumps)df[['id', 'order_timestamp', 'customer_id', 'order_status', 'order_amount', 'order_currency', 'products']].to_sql('raw_orders_managed', con=conn, if_exists='append', index=False)df_pg_catalog = pd.read_sql("select column_name, is_nullable, data_type, character_maximum_length, character_octet_length, numeric_precision, numeric_scale \
    from information_schema.columns where table_name='raw_orders_managed' order by ordinal_position", con=conn.connect())df_pg_size = pd.read_sql("select pg_total_relation_size(quote_ident(table_name)) as table_size \
    from information_schema.tables where table_name = 'raw_orders_managed'", con=conn.connect())
print("              ")
print(f"Orders As saved in the db : {df_pg_size['table_size'].values[0]} bytes")
df_pg_catalog
```

结果是:

![](img/97753bf48a3bf125a018c9ab19e4394a.png)![](img/69544450cd4c972bc2a14bb95b7795f9.png)

那么这里有什么问题呢？

1.  我们看到，与简单载荷相比，尺寸增加了很多。这是因为现在我们有了主键和唯一键的索引
2.  每当我们试图加载模式验证失败的数据时，都会在加载时引发一个错误。

为了在模式自动发现中复制上述行为，我们必须在写入数据后实现数据验证步骤。这意味着，如果验证失败(让我们以唯一性验证为例)，我们需要回滚并清理数据，然后才能理解为什么会有重复的记录，并与业务/源系统连接以纠正它们。因此，如果您使用数据库系统作为存储，请利用模式来避免额外的验证步骤。

## 而不是火花自动发现

那么，如果我们想使用德尔塔/冰山/胡迪格式的数据湖屋平台呢？幸运的是，无论是在数据框定义中，还是在数据目录中，它们都提供了模式管理。让我们看看数据框一:

```
import os
from demo_error_read_write.data_setup.utils import get_delta_spark_session
from pathlib import Path
from pyspark.sql.types import StructType,StructField, StringType, IntegerType filepath = os.getenv('DATA_OUTPUT_LANDING_ZONE')
filename_in= f"{filepath}/customers.txt"
filename_out = f"{filepath}/delta/customers/schema"spark_session = get_delta_spark_session()schema = StructType([ \
    StructField("id",IntegerType(),True), \
    StructField("email_address",StringType(),True), \
    StructField("first_name",StringType(),True), \
    StructField("last_name", StringType(), True)
  ])df_in = spark_session.read.option(
        'delimiter', '\t').option(
        'header', 'true').schema(schema).csv(filename_in)df_in.printSchema()df_in.write.mode("overwrite").format("delta").save(filename_out)
df_out = spark_session.read.format('delta').load(filename_out)df_out.printSchema()
print(f"Size {sum(file.stat().st_size for file in Path(filename_out).rglob('*'))} bytes")
```

导致:

```
customers
root
 |-- id: integer (nullable = true)
 |-- email_address: string (nullable = true)
 |-- first_name: string (nullable = true)
 |-- last_name: string (nullable = true)root
 |-- id: integer (nullable = true)
 |-- email_address: string (nullable = true)
 |-- first_name: string (nullable = true)
 |-- last_name: string (nullable = true)Size 3272 bytesorders
root
 |-- id: integer (nullable = true)
 |-- order_timestamp: timestamp (nullable = true)
 |-- customer_id: integer (nullable = true)
 |-- order_status: string (nullable = true)
 |-- order_amount: decimal(10,0) (nullable = true)
 |-- order_currency: string (nullable = true)
 |-- products: array (nullable = true)
 |    |-- element: struct (containsNull = true)
 |    |    |-- product_id: integer (nullable = true)
 |    |    |-- amount: integer (nullable = true)root
 |-- id: integer (nullable = true)
 |-- order_timestamp: timestamp (nullable = true)
 |-- customer_id: integer (nullable = true)
 |-- order_status: string (nullable = true)
 |-- order_amount: decimal(10,0) (nullable = true)
 |-- order_currency: string (nullable = true)
 |-- products: array (nullable = true)
 |    |-- element: struct (containsNull = true)
 |    |    |-- product_id: integer (nullable = true)
 |    |    |-- amount: integer (nullable = true)Size 5710 bytes
```

但是这里有什么问题呢？您不能强制长度、null 或 unique 约束，因此您仍然需要单独检查它们。此外，并不是在所有情况下模式强制都会导致错误:如果您没有在后面使用数据目录，您可能会得到空值而不是数据！

惟一约束是一个有趣的话题:分布式计算不会强制惟一性约束，因为它们需要在一个地方的所有数据来验证惟一性约束。所以这是使用 Spark 或 Snowflake 时需要注意的事情，这是你需要自己做的检查！

> 好的，好的，那么错误在哪里呢？
> 
> *一个疲惫的读者*

以下是《法师》中的快乐流:

![](img/e90fd0239f36a8f3a085b43c7966faa6.png)

至于错误流，我将简单地通过将 id 伪生成器从整数改为字符串来破坏数据。

# 顾客流量

![](img/ec7c4f2a506ea992dbe3d59930748cc7.png)

# 写入时出错

加载到 Postgres 上的托管架构失败，数据类型不匹配:

```
sqlalchemy.exc.DataError: (psycopg2.errors.InvalidTextRepresentation) invalid input syntax for type integer: "a"LINE 1: ...id, email_address, first_name, last_name) VALUES ('a', 'carr...

[SQL: INSERT INTO raw_customers_managed (id, email_address, first_name, last_name) VALUES (%(id)s, %(email_address)s, %(first_name)s, %(last_name)s)][parameters: ({'id': 'a', 'email_address': 'carrieyork@miller.com', 'first_name': 'Randy', 'last_name': 'Villanueva'}, {'id': 'c', 'email_address': 'douglas55@gmail.com', 'first_name': 'Cynthia', 'last_name': 'Avila'}, {'id': 'a', 'email_address': 'rgarcia@figueroa-johnson.biz', 'first_name': 'Michael', 'last_name': 'Miller'}, {'id': 'c', 'email_address': 'panderson@figueroa-montgomery.com', 'first_name': 'Jenna', 'last_name': 'Hughes'}, {'id': 'b', 'email_address': 'jeanhenderson@landry.net', 'first_name': 'Matthew', 'last_name': 'Hernandez'}, {'id': 'c', 'email_address': 'jbenson@jones.biz', 'first_name': 'Kara', 'last_name': 'Morales'}, {'id': 'b', 'email_address': 'caleberickson@carpenter-jackson.biz', 'first_name': 'David', 'last_name': 'Sanchez'}, {'id': 'c', 'email_address': 'swansoncameron@rios-brewer.com', 'first_name': 'Brian', 'last_name': 'Stone'}, {'id': 'c', 'email_address': 'sullivanjennifer@gmail.com', 'first_name': 'Kelly', 'last_name': 'Moore'})]
```

# 读取时出错

它们在我们查询数据时出现，例如:

```
select * from raw_customers_pandas where id=1;UndefinedFunction: operator does not exist: text = integer
LINE 3: select * from raw_customers_pandas where id=1;
                                                   ^
HINT:  No operator matches the given name and argument types. You might need to add explicit type casts.
```

# 无声错误

它们与 Spark 相关，不会在写入或读取时出现，它们只是隐藏在数据中:

```
df = spark_session.read.format('delta').load(filename_out)
df.printSchema()
df.show() |-- id: integer (nullable = true)
 |-- email_address: string (nullable = true)
 |-- first_name: string (nullable = true)
 |-- last_name: string (nullable = true)+----+--------------------+----------+---------+
|  id|       email_address|first_name|last_name|
+----+--------------------+----------+---------+
|null| julia98@hotmail.com|      Chad|    Patel|
|null|shannonferguson@y...|      Lisa| Morrison|
|null|elizabeth05@willi...|    Hailey|  Nichols|
|null| michael67@yahoo.com|     Jacob|   Macias|
|null|tracywilson@miles...|    Connie|    Smith|
|null|perezdylan@gmail.com|   Michael|     West|
|null|allencarl@rosario...|    Rachel|Rodriguez|
|null| dustin48@harris.org|   Matthew|   Riddle|
|null|ricenicole@yahoo.com|   Brandon|   Miller|
+----+--------------------+----------+---------+
```

# 订单流程

![](img/30119350aa10198bb366b8837781fba6.png)

# 写入时出错

加载托管 postgres 表失败，数据类型不匹配:

```
sqlalchemy.exc.DataError: (psycopg2.errors.InvalidTextRepresentation) invalid input syntax for type integer: "c"LINE 1: ..., order_amount, order_currency, products) VALUES ('c', '2022...
```

# 读取时出错

现在我们有两种错误:

*   当我们在期望整数的 id 上过滤时
*   当我们将该表与 raw_customers_pandas 表连接时

```
select * from raw_orders_pandas where id=1;
UndefinedFunction: operator does not exist: text = integer
LINE 3: select * from raw_orders_pandas where id=1;
                                                ^
HINT:  No operator matches the given name and argument types. You might need to add explicit type casts.select *
from raw_orders_pandas ord 
inner join raw_customers_pandas cust on (ord.customer_id = cust.id);UndefinedFunction: operator does not exist: smallint = text
LINE 4: ...oin raw_customers_pandas cust on (ord.customer_id = cust.id)...
                                                             ^
HINT:  No operator matches the given name and argument types. You might need to add explicit type casts.
```

# 无声错误

同样，由于数据类型不匹配，id 为空。

```
root
 |-- id: integer (nullable = true)
 |-- order_timestamp: timestamp (nullable = true)
 |-- customer_id: integer (nullable = true)
 |-- order_status: string (nullable = true)
 |-- order_amount: decimal(10,0) (nullable = true)
 |-- order_currency: string (nullable = true)
 |-- products: array (nullable = true)
 |    |-- element: struct (containsNull = true)
 |    |    |-- product_id: integer (nullable = true)
 |    |    |-- amount: integer (nullable = true)+----+
|  id|
+----+
|null|
|null|
|null|
```

# 结论

在本文中，我们介绍了从着陆区向界面层加载数据的不同方法。我们观察到，通过实现模式管理，我们可以避免数据清理和回滚活动。

使用 Spark 时，我们会遇到静默错误，这是由于在写入时缺乏模式执行(即使它是声明的！)，但这可以通过数据目录来实现。对于分布式系统，我们还会遇到无声的错误，尤其是当涉及到唯一性约束时。

传统的数据处理方式与所使用的数据库系统的功能密切相关。为了提供至少相同级别的准确性，现代方法要求在*标记*数据准确以供进一步使用之前实施一套验证规则。

![](img/0efd93c58cb9cfeb0f31b90eb975906c.png)

剩下的问题是:我们该如何处理功能验证？关于这一点，在我的下一篇文章！

# 关于法师

Mage.ai 是现代数据栈中发展最快的工具之一。这是一个巨大的产品，提供 E2E 数据产品开发，涵盖开发和执行数据管道所需的所有步骤。虽然我不能说在生产环境中如何使用它，并且我观察到在使用它时出现了一些小问题，但它非常有前途——它正在积极开发中。只有在一个界面中拥有(几乎)所有东西的事实，才使它成为进一步检查的工具。

我喜欢的是:

*   直观了解您正在实施和执行的作业类型
*   支持多种类型的步骤:导出器、加载器、转换器(尽管我认为导出器=加载器，只是源——目的地被交换)
*   探索性数据分析开箱即用，自动为您完成各种图表和统计
*   能够使用 Python 或 SQL
*   能够将临时代码转换成实际代码，这是一个了不起的特性！

# 关于 Postgres 15

正如我所说的，我正在使用新的 Postgres，但是在这篇文章中我并没有怎么使用它。关于 Postgres，有几件事值得一提:

*   如果您不处理大量数据，Postgres 可能是一个不错的解决方案
*   它提供了数组和 json 字段
*   它具有 RDBMS 系统的所有优点
*   当您的源系统也是 Postgres 时，它提供了现成的逻辑复制 CDC

请继续关注下一篇文章:我们深入到缓慢变化的维度，以及读取错误或无声错误如何影响我们的用户！

# 证明文件

1.  [法师官网](https://www.mage.ai/)
2.  [法师 GitHub](https://github.com/mage-ai/mage-ai)
3.  [Postgres 信息图式](https://www.postgresql.org/docs/current/infoschema-columns.html)
4.  [三角洲湖模式实施](https://www.databricks.com/blog/2019/09/24/diving-into-delta-lake-schema-enforcement-evolution.html)
5.  [Github 的文章](https://github.com/acirtep/demo-mage-ai)