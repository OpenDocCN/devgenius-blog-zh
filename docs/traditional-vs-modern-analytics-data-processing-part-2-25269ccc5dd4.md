# 传统与现代分析数据处理(下)

> 原文：<https://blog.devgenius.io/traditional-vs-modern-analytics-data-processing-part-2-25269ccc5dd4?source=collection_archive---------5----------------------->

文章首发 [@ownyourdata.ai](https://ownyourdata.ai/wp/traditional-vs-modern-analytics-data-processing-part-2/) 。

在我之前的[文章](https://ownyourdata.ai/wp/traditional-vs-modern-analytics-data-processing-part-1/)中，我写了什么是读取错误、写入错误或静默错误，以及数据验证是我们观察到的传统和现代分析数据处理之间最大差异的步骤之一。

我们看到现代数据堆栈中的工具都带有模式自动发现功能，但是这可能会在读取时产生错误，甚至是无声的错误。一个简单的解决方案是实现模式管理，并利用底层存储系统(数据库或数据目录)。模式是基础设施，在另一篇[文章](https://ownyourdata.ai/wp/programmatic-schema-management/)中，我展示了用于模式管理的 Alembic 案例(以及其他选项)。

但是在某些情况下，每个处理步骤之后都需要数据验证步骤，这样数据就不会被破坏:

![](img/5d8cec479972e0d119057f166929135f.png)

# 从原始层到集成层

在上一篇文章中，我们看到了从着陆区到 raw 的数据处理是如何受到没有适当的模式管理的影响的。在本文中，我们将浏览从原始到集成层的负载，并进一步使用 Mage 进行实验。关于我们将使用的数据模型的提醒:

![](img/8dbfc57f20bb8009f32607eca20865e0.png)

# 模式和加载模式

对于这个演示，我们将遵循每日批量加载方法，这意味着源系统每天在某个时刻导出一次数据:这称为截止，通常是数据契约的一部分。这意味着在某个文件中交付的记录与截止时间该记录在该系统中的版本相关。因此，截止值是一个重要字段，将与数据一起存储在原始层中。

根据存储和处理层，可以启用主键、外键和唯一键。在任一层都具备这些条件，可以确保交付和数据处理都不会影响数据的完整性。

但是，值得一提的是，使用这样的键可能会降低数据加载的速度，而且成本可能会很高。根据使用情况，拥有这样的密钥的成本可能是合理的。在传统方式中，我们在模式级别定义键，其中一些键在运行时会被禁用和重新启用。这个决定是基于性能指标做出的，通常，小维度表会保留它们的键。

在现代数据处理中，人们甚至可能不定义关键字，而是定义在加载完成后要执行的检查。与此同时，现代存储和处理解决方案依赖于分布式系统，因此，为了在加载之前进行验证，数据不会立即处于同一位置(分布式系统的极度简化)。

在下图中，为维度和事实表都定义了键，为了便于演示，我将保持启用它们:

![](img/53d0839dff88e516442b09399ccabc6c.png)

在我们在 Mage 中实现上述流程之前，让我们创建两个流程，它们生成假数据并将其写入一个文件(模拟来自源系统的批量交付):

![](img/828337aca27dc16e42469c2ba1e9e59b.png)![](img/788fc0ba6b7edb58711833e780d8d2da.png)

上述流程将生成 2 个文件，它们将遵循数据合同中规定的命名约定:

*   客户-{截止日期}。战斗支援车
*   订单-{截止日期}。战斗支援车

在本文中，我们将按照传统和现代的方式在 Mage 中实现 load up until dim _ customer。我们将使用正确的和损坏的数据运行流，并检查差异。

# 数据加载器

Mage 中的数据加载器是从源中检索数据并将其加载到 dataframe 中的步骤，data frame 将作为下一步的输入。我们将使用文件加载器，它将从着陆区读取数据并返回数据帧中的数据。

```
@data_loader
def load_data_from_file(*args, **kwargs):
    """
    Template for loading data from filesystem.
    Docs: https://github.com/mage-ai/mage-ai/blob/master/docs/blocks/data_loading.md#fileio
    """
    filepath = f'/app/demo_error_read_write/data_setup/landing_zone/customers-{kwargs["cut_off_date"]}.csv'
    return FileIO().load(filepath, **{'sep': '\t'}) 
```

你遵守截止日期吗？它被配置为管道级别的变量，因此它可以用于在某次运行中检索相应的数据！但是，在生产设置中，截止日期将默认为昨天(对于每日计划运行)。

# 变形金刚(电影名)

是转换数据的步骤，Mage 提供了相当多的现成内容(忘记一次又一次搜索 pandas 的聚合语法吧！).我们将同时使用 Python 和 SQL 转换器。将截止日期添加到数据帧的示例:

```
@transformer
def transform_df(df: DataFrame, *args, **kwargs) -> DataFrame:
    """
    Template code for a transformer block.
    Add more parameters to this function if this block has multiple parent blocks.
    There should be one parameter for each output variable from each parent block.
    Args:
        df (DataFrame): Data frame from parent block.
    Returns:
        DataFrame: Transformed data frame
    """
    # Specify your transformation logic here
    df["cut_off_date"] = kwargs["cut_off_date"]
    return df
```

# 数据导出器

数据导出器是将数据导出(写入)到目标的步骤。我们将使用它来写入 Postgres，但是我们不能使用开箱即用的 Mage 连接器:所有的表都包含由数据库加载的审计列，而不是由数据处理步骤([特性请求](https://github.com/mage-ai/mage-ai/issues/1397))加载的审计列。因此，我们将使用 pandas.to_sql 方法:

```
@data_exporter
def export_data(df, **kwargs):
    """
    Exports data to some sourceArgs:
        df (DataFrame): Data frame to export to
    Output (optional):
        Optionally return any object and it'll be logged and
        displayed when inspecting the block run.
    """
    conn = create_engine("postgresql://postgres:postgres@postgres_db:5432/postgres")
    conn = conn.connect()

    df[['id', 'email_address', 'first_name', 'last_name', 'cut_off_date']].to_sql(
        'raw_customer', schema='postgres',con=conn, if_exists='append', index=False) 
```

# 尺寸缓慢变化

缓变维度(SCD)的概念来自维度建模，指的是(猜测的)缓慢变化的维度数据。在本演示中，我们将为客户数据实施 SCD type 2，以捕获其版本。即使在现代堆栈上，我们有时间旅行，那个版本与记录的技术时间戳相关。在本演示中，我们将在截止日期构建有效期间隔，以便在某一时刻及时检索源系统中的记录版本。

让我们回顾一下 dim_customer 键:

*   PK:主键，它在整个表中是惟一的，用于检索单个记录
*   SK:代理键，它在集成层中唯一地标识一个客户
*   NK:自然键，它在数据源层唯一地标识一个客户(因此 SK 是 NK 和源系统的组合)

为了简化起见，我们将把 customer_sk 和 customer_nk 映射到源系统中记录的主键。

为了构建 SCD，我们需要:

*   从 raw 获取处理截止日期的所有记录
*   将记录与 dim 进行比较，并标记它们以便插入或更新
*   如果 dim 中已经存在该记录，请从 dim 中关闭该记录

让我们用 SQL 来实现它:

```
select
        rc.id as customer_sk,
        rc.id as customer_nk,
        'ONE' as source_system,
        rc.email_address,
        rc.first_name,
        rc.last_name,
        rc.cut_off_date as valid_from,
        '9999-12-31'::date as valid_to,
         null as changed_fields
    from raw_customer rc
    left join dim_customer dc on (
        rc.id = dc.customer_nk and dc.valid_to='9999-12-31')
    where rc.cut_off_date=???
    and dc.customer_pk is null
```

我们首先在 raw 和 dim 之间进行反连接。通过这样做，我们为以前从未加载过的客户标记插入记录。然后，我们构建现有客户的新版本，并将其标记为插入，确保仅当新版本与当前版本不同时才加载新版本:

```
select
        rc.id as customer_sk,
        rc.id as customer_nk,
        'ONE' as source_system,
        rc.email_address,
        rc.first_name,
        rc.last_name,
        rc.cut_off_date as valid_from,
        '9999-12-31'::date as valid_to,
        array[
           case when rc.email_address <> dc.email_address then 'email_address' end, 
           case when rc.first_name <> dc.first_name then 'first_name' end,
           case when rc.last_name <> dc.last_name then 'last_name' end
        ] as changed_fields
    from raw_customer rc
    inner join dim_customer dc on (
        rc.id = dc.customer_nk)
    where rc.cut_off_date=???
    and dc.valid_to='9999-12-31'
    and array[rc.email_address, rc.first_name, rc.last_name] <> array[dc.email_address, dc.first_name, dc.last_name]
```

最后，在 Postgres 15 中，我们可以使用 MERGE INTO 操作来插入上述记录并关闭现有记录:

```
MERGE into postgres.dim_customer tgt
using (
    select
        rc.id as customer_sk,
        rc.id as customer_nk,
        'ONE' as source_system,
        rc.email_address,
        rc.first_name,
        rc.last_name,
        rc.cut_off_date as valid_from,
        '9999-12-31'::date as valid_to,
        null as changed_fields
    from raw_customer rc
    left join dim_customer dc on (
        rc.id = dc.customer_nk and dc.valid_to='9999-12-31')
    where rc.cut_off_date=???
    and dc.customer_pk is null
    union all
        select
        rc.id as customer_sk,
        rc.id as customer_nk,
        'ONE' as source_system,
        rc.email_address,
        rc.first_name,
        rc.last_name,
        rc.cut_off_date as valid_from,
        '9999-12-31'::date as valid_to,
        array[
           case when rc.email_address <> dc.email_address then 'email_address' end, 
           case when rc.first_name <> dc.first_name then 'first_name' end,
           case when rc.last_name <> dc.last_name then 'last_name' end
        ] as changed_fields
    from raw_customer rc
    inner join dim_customer dc on (
        rc.id = dc.customer_nk)
    where rc.cut_off_date=???
    and dc.valid_to='9999-12-31'
    and array[rc.email_address, rc.first_name, rc.last_name] <> array[dc.email_address, dc.first_name, dc.last_name]
) src
on (tgt.customer_sk = src.customer_sk and tgt.valid_to='9999-12-31')
when matched then update set valid_to = ???-1day, updated_datetime= current_timestamp
when not matched then
insert (customer_sk, customer_nk, source_system, email_address, first_name, last_name, valid_from, valid_to)
Values (src.customer_sk, src.customer_nk, src.source_system, src.email_address, src.first_name, src.last_name, src.valid_from, src.valid_to);
```

为了展示现代的方式，我决定使用 dbt。Mage 提供了对 dbt 模型的支持，但是因为 dbt 快照仍然是一个[特性请求](https://github.com/mage-ai/mage-ai/issues/1399),所以我决定用子进程运行`dbt snapshot`(不要在家里这样做！).

```
{% snapshot dim_customer_dbt %}
{{
        config(
          target_schema='public',
          strategy='check',
          unique_key='customer_nk',
          updated_at='cut_off_date',
          check_cols=['email_address', 'first_name', 'last_name'],
        )
    }}
    select id as customer_nk,
           'ONE' as source_system,
           id as customer_sk,
           email_address,
           first_name,
           last_name,
           cut_off_date
    from raw_customers_pandas
{% endsnapshot %}
```

以上是 SCD 的实现，针对带有`unique_key`的记录，基于`cut_off_date`检查`check_cols`中配置的字段。

将这些部分放在一起，我们有下面两个流程:

![](img/256af679799a1e3d209c229f79d9a606.png)![](img/a690fa0e8ca0a293d768ba3d3b43d162.png)

# 使用正常数据执行

对于这个演示，我已经生成了两个日期的 OK 数据:

*   2022–11–16
*   2022–12–13

通过在每个管道设置截止日期，执行将拾取对应于截止日期的文件并对其进行处理。现代和传统的最终数据看起来都很相似:

```
postgres=# select count(*) from dim_customer;
 count 
-------
    18
(1 row)

postgres=# select * from dim_customer where customer_nk=1;
 customer_pk | customer_sk | customer_nk | source_system |        email_address        | first_name | last_name |            changed_fields            |     valid_from      |      valid_to       |     inserted_datetime      |      updated_datetime      
-------------+-------------+-------------+---------------+-----------------------------+------------+-----------+--------------------------------------+---------------------+---------------------+----------------------------+----------------------------
          19 |           1 |           1 | ONE           | corey86@jordan-gonzales.com | Jose       | Simon     | {email_address,first_name,last_name} | 2022-11-16 00:00:00 | 2022-12-12 00:00:00 | 2022-12-13 21:21:25.573795 | 2022-12-13 21:21:44.394117
          28 |           1 |           1 | ONE           | smithpeter@gmail.com        | Cynthia    | Chapman   |                                      | 2022-12-13 00:00:00 | 9999-12-31 00:00:00 | 2022-12-13 21:22:00.963905 | 2022-12-13 21:22:00.963905
(2 rows)s
```

```
postgres=# select count(*) from dim_customer_dbt;
 count 
-------
    18
(1 row)

postgres=# select * from dim_customer_dbt where customer_nk=1;
 customer_nk | source_system | customer_sk |        email_address        | first_name | last_name | cut_off_date |            dbt_scd_id            | dbt_updated_at | dbt_valid_from| dbt_valid_to 
-------------+---------------+-------------+-----------------------------+------------+-----------+--------------+----------------------------------+----------------+----------------+--------------
           1 | ONE           |           1 | corey86@jordan-gonzales.com | Jose       | Simon     | 2022-11-16   | 98f453404558f384b1b54911bad40993 | 2022-11-16     | 2022-11-16    | 2022-12-13
           1 | ONE           |           1 | smithpeter@gmail.com        | Cynthia    | Chapman   | 2022-12-13   | 66e094744fa8f4a4a9d0ed6b2439ba52 | 2022-12-13     | 2022-12-13    | 
(2 rows)
```

那么关键的区别是什么呢？

1.  在 dbt 快照中，我们没有审计信息。你可以在这篇[文章](https://ownyourdata.ai/wp/a-way-to-ensure-auditability-in-data-processing/)中找到更多关于它的信息。
2.  在 dbt 快照中，主键是`dbt_scd_id`列:unique_key 和 cut_off_date 的组合。如果用不同的数据重新处理相同的日期，这可能会产生问题(*未测试)
3.  在 dbt 快照中，最近的记录具有空值`valid_to`。这意味着用户应该总是在有效时间间隔内应用合并过滤。最佳实践是选择一个遥远的未来日期，这样无论用户想要检索哪个版本，都可以使用相同的 SQL where 子句(date between)。(下面的例子)
4.  我认为字段`dbt_updated_at`具有误导性，因为它包含截止日期，而不是 dbt 更新的时间戳(与可审计性相关)
5.  用传统的方法，我们可以计算哪些字段在驱动更新(字段`changed_fields`)。在某些情况下，跟踪每个字段的变化是很重要的，如果你是这种情况，我建议你检查一下 Data Vault 和 Anchor modelling，它们可以解决这个需求。

```
postgres=# select count(*) from dim_customer_dbt where current_timestamp between cast(dbt_valid_from as date) and cast(dbt_valid_to as date);
 count 
-------
     0
(1 row)

postgres=# select count(*) from dim_customer where current_timestamp between valid_from and valid_to;
 count 
-------
     9
(1 row)
```

我还认为 dbt 生成的 SQL 不必要地复杂(一个原因是 Postgres 15 中引入了 MERGE):

```
demo_mage_pg   |          create temporary table "dim_customer_dbt__dbt_tmp215301328009"
demo_mage_pg   |          as (
demo_mage_pg   |            with snapshot_query as (
demo_mage_pg   |            select id as customer_nk,
demo_mage_pg   |                   'ONE' as source_system,
demo_mage_pg   |                   id as customer_sk,
demo_mage_pg   |                   email_address,
demo_mage_pg   |                   first_name,
demo_mage_pg   |                   last_name,
demo_mage_pg   |                   cut_off_date
demo_mage_pg   |            from raw_customers_pandas
demo_mage_pg   |            ),
demo_mage_pg   |        
demo_mage_pg   |            snapshotted_data as (
demo_mage_pg   |        
demo_mage_pg   |                select *,
demo_mage_pg   |                    customer_nk as dbt_unique_key
demo_mage_pg   |        
demo_mage_pg   |                from "postgres"."public"."dim_customer_dbt"
demo_mage_pg   |                where dbt_valid_to is null
demo_mage_pg   |        
demo_mage_pg   |            ),
demo_mage_pg   |        
demo_mage_pg   |            insertions_source_data as (
demo_mage_pg   |        
demo_mage_pg   |                select
demo_mage_pg   |                    *,
demo_mage_pg   |                    customer_nk as dbt_unique_key,
demo_mage_pg   |                    cut_off_date as dbt_updated_at,
demo_mage_pg   |                    cut_off_date as dbt_valid_from,
demo_mage_pg   |                    nullif(cut_off_date, cut_off_date) as dbt_valid_to,
demo_mage_pg   |                    md5(coalesce(cast(customer_nk as varchar ), '')
demo_mage_pg   |                 || '|' || coalesce(cast(cut_off_date as varchar ), '')
demo_mage_pg   |                ) as dbt_scd_id
demo_mage_pg   |        
demo_mage_pg   |                from snapshot_query
demo_mage_pg   |            ),
demo_mage_pg   |        
demo_mage_pg   |            updates_source_data as (
demo_mage_pg   |        
demo_mage_pg   |                select
demo_mage_pg   |                    *,
demo_mage_pg   |                    customer_nk as dbt_unique_key,
demo_mage_pg   |                    cut_off_date as dbt_updated_at,
demo_mage_pg   |                    cut_off_date as dbt_valid_from,
demo_mage_pg   |                    cut_off_date as dbt_valid_to
demo_mage_pg   |        
demo_mage_pg   |                from snapshot_query
demo_mage_pg   |            ),
demo_mage_pg   |        
demo_mage_pg   |            insertions as (
demo_mage_pg   |        
demo_mage_pg   |                select
demo_mage_pg   |                    'insert' as dbt_change_type,
demo_mage_pg   |                    source_data.*
demo_mage_pg   |        
demo_mage_pg   |                from insertions_source_data as source_data
demo_mage_pg   |                left outer join snapshotted_data on snapshotted_data.dbt_unique_key = source_data.dbt_unique_key
demo_mage_pg   |                where snapshotted_data.dbt_unique_key is null
demo_mage_pg   |                   or (
demo_mage_pg   |                        snapshotted_data.dbt_unique_key is not null
demo_mage_pg   |                    and (
demo_mage_pg   |                        (snapshotted_data."email_address" != source_data."email_address"
demo_mage_pg   |                or
demo_mage_pg   |                (
demo_mage_pg   |                    ((snapshotted_data."email_address" is null) and not (source_data."email_address" is null))
demo_mage_pg   |                    or
demo_mage_pg   |                    ((not snapshotted_data."email_address" is null) and (source_data."email_address" is null))
demo_mage_pg   |                ) or snapshotted_data."first_name" != source_data."first_name"
demo_mage_pg   |                or
demo_mage_pg   |                (
demo_mage_pg   |                    ((snapshotted_data."first_name" is null) and not (source_data."first_name" is null))
demo_mage_pg   |                    or
demo_mage_pg   |                    ((not snapshotted_data."first_name" is null) and (source_data."first_name" is null))
demo_mage_pg   |                ) or snapshotted_data."last_name" != source_data."last_name"
demo_mage_pg   |                or
demo_mage_pg   |                (
demo_mage_pg   |                    ((snapshotted_data."last_name" is null) and not (source_data."last_name" is null))
demo_mage_pg   |                    or
demo_mage_pg   |                    ((not snapshotted_data."last_name" is null) and (source_data."last_name" is null))
demo_mage_pg   |                ))
demo_mage_pg   |                    )
demo_mage_pg   |                )
demo_mage_pg   |        
demo_mage_pg   |            ),
demo_mage_pg   |        
demo_mage_pg   |            updates as (
demo_mage_pg   |        
demo_mage_pg   |                select
demo_mage_pg   |                    'update' as dbt_change_type,
demo_mage_pg   |                    source_data.*,
demo_mage_pg   |                    snapshotted_data.dbt_scd_id
demo_mage_pg   |        
demo_mage_pg   |                from updates_source_data as source_data
demo_mage_pg   |                join snapshotted_data on snapshotted_data.dbt_unique_key = source_data.dbt_unique_key
demo_mage_pg   |                where (
demo_mage_pg   |                    (snapshotted_data."email_address" != source_data."email_address"
demo_mage_pg   |                or
demo_mage_pg   |                (
demo_mage_pg   |                    ((snapshotted_data."email_address" is null) and not (source_data."email_address" is null))
demo_mage_pg   |                    or
demo_mage_pg   |                    ((not snapshotted_data."email_address" is null) and (source_data."email_address" is null))
demo_mage_pg   |                ) or snapshotted_data."first_name" != source_data."first_name"
demo_mage_pg   |                or
demo_mage_pg   |                (
demo_mage_pg   |                    ((snapshotted_data."first_name" is null) and not (source_data."first_name" is null))
demo_mage_pg   |                    or
demo_mage_pg   |                    ((not snapshotted_data."first_name" is null) and (source_data."first_name" is null))
demo_mage_pg   |                ) or snapshotted_data."last_name" != source_data."last_name"
demo_mage_pg   |                or
demo_mage_pg   |                (
demo_mage_pg   |                    ((snapshotted_data."last_name" is null) and not (source_data."last_name" is null))
demo_mage_pg   |                    or
demo_mage_pg   |                    ((not snapshotted_data."last_name" is null) and (source_data."last_name" is null))
demo_mage_pg   |                ))
demo_mage_pg   |                )
demo_mage_pg   |            )
demo_mage_pg   |        
demo_mage_pg   |            select * from insertions
demo_mage_pg   |            union all
demo_mage_pg   |            select * from updates
demo_mage_pg   |        
demo_mage_pg   |          );
```

# 使用损坏的数据执行

对于本演示，损坏的文件将包含:

*   有一些更新的旧记录
*   id 不正确的新记录
*   具有正确 id 的新记录
*   重复记录

```
id	email_address	first_name	last_name
8	phicks@dean-williams.com	Amanda	Lewis
8	phicks@dean-williams.com	Duplicated	Lewis
c	oholland@gmail.com	Christine	Williams
10	edoyle@doyle.net	Michael	Wagner
b	zacharyjordan@sutton.net	Lisa	Perez
```

# 传统方式

由于模式管理，传统的加载管道从将数据加载到 raw 的步骤开始就失败了。dim_customer 中的数据未被修改，因此数据未被刷新。

![](img/7ae00174e67b63a20172d67758eae153.png)

```
sqlalchemy.exc.DataError: (psycopg2.errors.InvalidTextRepresentation) invalid input syntax for type integer: "c"
LINE 1: ...-williams.com', 'Amanda', 'Lewis', '2022-12-14'),('c', 'ohol...
                                                             ^
[SQL: INSERT INTO public.raw_customer (id, email_address, first_name, last_name, cut_off_date) VALUES (%(id)s, %(email_address)s, %(first_name)s, %(last_name)s, %(cut_off_date)s)]
```

根据不同的用例，人们可能希望加载正确的记录，而忽略不正确的记录。下面介绍几种方法:

1.  主动:通过预先检查哪些记录符合数据契约，将错误的记录存放在错误表中
2.  被动:从原始文件中删除错误的记录(在备份后)并重新开始该过程

# 现代方式

默认情况下，现代管道在加载到 raw 时不会失败，因为 id 在模式发现中被视为文本。但是 dbt 运行确实失败了:

![](img/c48db0f254d273361f283ec8521245b9.png)

```
05:11:36  Running with dbt=1.3.1
05:11:36  [WARNING]: Configuration paths exist in your dbt_project.yml file which do not apply to any resources.
There are 1 unused configuration paths:
- models.demo.example
```

```
05:11:36  Found 1 model, 0 tests, 1 snapshot, 0 analyses, 289 macros, 0 operations, 0 seed files, 4 sources, 0 exposures, 0 metrics
05:11:36  
05:11:36  Concurrency: 1 threads (target='dev')
05:11:36  
05:11:36  1 of 1 START snapshot public.dim_customer_dbt .................................. [RUN]
05:11:36  1 of 1 ERROR snapshotting public.dim_customer_dbt .............................. [ERROR in 0.12s]
05:11:36  
05:11:36  Finished running 1 snapshot in 0 hours 0 minutes and 0.28 seconds (0.28s).
05:11:36  
05:11:36  Completed with 1 error and 0 warnings:
05:11:36  
05:11:36  Database Error in snapshot dim_customer_dbt (snapshots/dim_customer_dbt.sql)
05:11:36    operator does not exist: smallint = text
05:11:36    LINE 71: ...apshotted_data on snapshotted_data.dbt_unique_key = source_d...
05:11:36                                                                  ^
05:11:36    HINT:  No operator matches the given name and argument types. You might need to add explicit type casts.
05:11:36  
05:11:36  Done. PASS=0 WARN=0 ERROR=1 SKIP=0 TOTAL=1
```

当第一次执行 dbt 快照时，用`create as`创建了表，Postgres 决定将数据类型`smallint`分配给 ID 列，这将在某个点由于超出范围而开始中断。

```
postgres=# update dim_customer_dbt set customer_nk = 32768 where customer_nk=9;
ERROR:  smallint out of range
```

为了修复流程，我们现在需要更正 load to raw 步骤中的数据，即使错误发生在 dbt 步骤中。我们可以:

1.  按照我们传统的方式管理原始表的模式
2.  在加载步骤之前，对我们在`add_cut_off_date`中准备的数据添加验证步骤

在`@test`装饰器的帮助下，我们可以在每个步骤中定义验证规则:

```
@test
def validate_data(df) -> None:
    """
    Validate 
    """
    from pandas.api.types import is_integer_dtype
    assert df is not None, 'The output is undefined'
    assert is_integer_dtype(df.id)
```

现在，流程在`add_cut_off_date`中的验证步骤失败:

![](img/8e55f9ae9ebc35770446b377a092e725.png)

带有(不太详细的)错误:

```
[add_cut_off_date] FAIL: validate_ (block: add_cut_off_date)
[add_cut_off_date] --------------------------------------------------------------
[add_cut_off_date] Traceback (most recent call last):
[add_cut_off_date]   File "/usr/local/lib/python3.10/site-packages/mage_ai/data_preparation/models/block/__init__.py", line 1110, in run_tests
[add_cut_off_date]     func(*outputs)
[add_cut_off_date]   File "<string>", line 36, in validate_
[add_cut_off_date] AssertionError
```

让我们清除数据错误的记录，再试一次！

输入数据:

```
id	email_address	first_name	last_name
8	phicks@dean-williams.com	Amanda	Lewis
8	phicks@dean-williams.com	Duplicated	Lewis
10	edoyle@doyle.net	Michael	Wagner
```

传统管道在原始阶段就失败了

```
sqlalchemy.exc.IntegrityError: (psycopg2.errors.UniqueViolation) duplicate key value violates unique constraint "pk_raw_customer_id"
DETAIL:  Key (id, cut_off_date)=(8, 2022-12-14) already exists.
```

而现代管道运行顺利。让我们看看表格中的数据:

```
postgres=# select customer_nk, email_address, first_name, last_name, dbt_valid_from, dbt_valid_to from dim_customer_dbt where customer_nk=8;
 customer_nk |      email_address       | first_name | last_name | dbt_valid_from | dbt_valid_to 
-------------+--------------------------+------------+-----------+----------------+--------------
           8 | qkirby@yahoo.com         | Eric       | Lopez     | 2022-11-16     | 2022-12-13
           8 | mariamartin@hotmail.com  | Thomas     | Gould     | 2022-12-13     | 2022-12-14
           8 | phicks@dean-williams.com | Amanda     | Lewis     | 2022-12-14     | 
           8 | phicks@dean-williams.com | Duplicated | Lewis     | 2022-12-14     | 
(4 rows)
```

重复数据到达 dim 表，破坏了数据完整性。为了防止这种情况，我们还需要添加一个唯一性检查:

```
@test
def validate_(df) -> None:
    """
    Validate 
    """
    from pandas.api.types import is_integer_dtype
    assert df is not None, 'The output is undefined'
    assert is_integer_dtype(df.id)
    assert df.id.is_unique
```

在这种设置下，数据准备步骤将失败，并出现以下情况:

```
[add_cut_off_date] FAIL: validate_ (block: add_cut_off_date)
[add_cut_off_date] --------------------------------------------------------------
[add_cut_off_date] Traceback (most recent call last):
[add_cut_off_date]   File "/usr/local/lib/python3.10/site-packages/mage_ai/data_preparation/models/block/__init__.py", line 1110, in run_tests
[add_cut_off_date]     func(*outputs)
[add_cut_off_date]   File "<string>", line 37, in validate_
[add_cut_off_date] AssertionError
```

虽然错误跟踪没有说太多，但是检查代码我们就知道哪个验证步骤失败了:

![](img/49a35a6c26d3cb95f3111353dee71d76.png)

或者简单地向断言步骤添加描述！

```
assert is_integer_dtype(df.id), 'The id is not of type integer'
assert df.id.is_unique, 'The id is not unique'
```

这将在错误中生成额外的信息:

```
[add_cut_off_date]   File "<string>", line 37, in validate_
[add_cut_off_date] AssertionError: The id is not unique
[add_cut_off_date]
```

# 结论

在本文中，我们介绍了如何在 Mage 中建立两条数据管道:一条是使用模式自动发现和 dbt 的现代方式，另一条是使用模式管理和合并操作的传统方式。我们看到，在现代方法中，错误发生在读取时或被抑制，除非我们在过程中配置验证规则。

无论是现代的还是传统的数据处理，由于错误的数据传递(或检索)而导致的错误都需要联系并解决源系统上的问题。数据处理平台不能取代团队之间、数据生产者和数据消费者之间的协作，即使现代方式使数据处理变得灵活、敏捷并符合软件开发实践，协作仍然是数据领域中需要解决的领域。

本系列的下一篇文章将讨论如何处理订单数据，以及数据验证(或缺少数据验证)可能对其产生的影响。在那之前，你可以自己尝试一下，代码是[这里是](https://github.com/acirtep/demo-mage-ai)。

证明文件

1.  [缓慢变化的维度](https://en.wikipedia.org/wiki/Slowly_changing_dimension)
2.  [Postgres 15 合并](https://www.postgresql.org/docs/current/sql-merge.html)
3.  [DBT 快照](https://docs.getdbt.com/docs/build/snapshots)