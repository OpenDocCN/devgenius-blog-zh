# PostgreSQL 数据库中高 CPU 利用率故障排除:操作指南

> 原文：<https://blog.devgenius.io/troubleshooting-high-cpu-utilization-in-postgresql-databases-a-how-to-guide-3c1ca76d8db6?source=collection_archive---------2----------------------->

![](img/683634c6e83300fa7af4a1483ffd7f0b.png)

这篇文章最初发表在 JFrog 的博客上。

PostgreSQL 是世界上最流行的数据库之一。

中央处理器(CPU)使用率是检查 PostgreSQL DB(数据库)实例的关键指标之一。检查 CPU 利用率有助于了解数据库是否遇到了性能问题，如低效的 SQL 查询、缺少索引和争用。

在看到 PostgreSQL DB 实例的高 CPU 之后，找到问题的根本原因是非常重要的。查询可能写得很差，太频繁，或者很重。

这篇博客文章描述了可以在 PostgreSQL DB 实例上运行的有用的 SQL 查询，以调查高 CPU 利用率。它还将帮助您了解数据库实例中影响 CPU 使用的运行内容。

# SQL 查询#1 —连接摘要

PostgreSQL 数据库导致高 CPU 利用率的模式之一是大量的活动连接。以下 SQL 查询列出了:

1.  连接总数
2.  非空闲连接数
3.  最大可用连接数
4.  连接利用率百分比

```
select 
    A.total_connections, 
    A.non_idle_connections, 
    B.max_connections,
    round((100 * A.total_connections::numeric / B.max_connections::numeric), 2) connections_utilization_pctg
from
  (select count(1) as total_connections, sum(case when state!='idle' then 1 else 0 end) as non_idle_connections from pg_stat_activity) A,
  (select setting as max_connections from pg_settings where name='max_connections') B;
```

以下是 SQL 输出的一个示例:

```
total_connections | non_idle_connections | max_connections | connections_utilization_pctg 
-------------------+----------------------+-----------------+------------------------------
              3457 |                    3 | 9057            |                        38.17
```

在上面的输出示例中，PostgreSQL DB 实例当前有 3，457 个连接，到 DB 服务器的最大并发连接数是 9，057，这意味着 38.17%的连接槽被占用。非空闲连接的数量是 3。

# 对 SQL 查询#1 有什么建议？

对 SQL Query one 的建议是检查 PostgreSQL DB 实例上正在运行的会话，尝试使用 EXPLAIN 来识别和分析长时间运行、编写糟糕、过于频繁的查询。如果每个 CPU 内核的活动连接数超过一个，建议检查和调整与数据库一起工作的应用程序。

要检查最大连接数，请运行以下 SQL 命令:

```
show max_connections;
```

通常，默认值是 100 个连接。

如果连接数接近为给定 PostgreSQL 实例设置的最大连接数，建议分析应用程序活动和/或应用程序逻辑，减少到达数据库的连接数，调优 max_connections 参数，或纵向扩展数据库实例。

此外，我建议使用和/或调整连接池:外部连接池，如 PgBouncer，或者内部连接池，如 Java 的 HikariCP 或 Go 的 PGX。

参数 max_connections 定义了 PostgreSQL 数据库服务器的最大并发连接数。更改此参数需要在更改后重新启动 PostgreSQL DB 实例，以便新值生效。对于这个实例，建议将 max_connections PostgreSQL 参数设置为峰值负载时预期的最大连接数。

另一方面，max_connections 参数需要与 PostgreSQL DB 机器的可用资源保持一致。应该仔细调整它，以避免由于每个 PostgreSQL DB 连接分配 shared_buffer 内存块和非共享内存而导致的系统内存不足问题。

为了调优参数 max_connections 和其他关键的 PostgreSQL DB 实例参数，我建议使用免费的在线工具，例如位于[https://www.pgconfig.org](https://www.pgconfig.org/)的 PostgreSQL 配置生成器。

# SQL 查询#2 —每个数据库的非空闲连接分布

使用以下查询检查每个数据库的非空闲连接数分布，按降序排序:

```
select 
datname as db_name, 
count(1) as num_non_idle_connections 
from pg_stat_activity 
where state!='idle' 
group by 1 
order by 2 desc;
```

以下是 SQL 输出的一个示例:

```
db_name                    | num_non_idle_connections 
----------------------------+--------------------------
 my_db_1                    |                      133
 my_db_2                    |                        6
 my_db_3                    |                        3
```

在上面的示例中，最常见的非空闲会话正在 DB my_db_1 上运行。

# 对 SQL 查询#2 有什么建议？

在这种情况下，建议检查 PostgreSQL DB 实例上正在运行的 DB my_db_1 会话，尝试识别长时间运行、编写不当、过于频繁的查询。

# SQL 查询#3 —每个数据库和每个查询的非空闲连接的分布

检查每个数据库和每个查询的非空闲连接的分布，按降序排列，如下例所示:

```
select 
datname as db_name, 
query, 
count(1) as num_non_idle_connections 
from pg_stat_activity 
where state!='idle' 
group by 1, 2 
order by 3 desc;
```

结果集中的输出文本可能看起来太长。在这种情况下，修改后的查询版本会很有帮助，如下例所示:

```
select 
datname as db_name, 
substr(query, 1, 200) short_query, 
count(1) as num_non_idle_connections 
from pg_stat_activity 
where state!='idle' 
group by 1, 2 
order by 3 desc;
```

以下是 SQL 输出的一个示例:

```
db_name  |              short_query              | num_non_idle_connections 
----------+---------------------------------------+--------------------------
 db_1     | select * from table_1                |                       40
 db_1     | select * from table_2                |                        1
```

在这个例子中，连接到 db_1 的应用程序/客户机正在运行两个 PostgreSQL 查询。上面的查询从 table_1 中选择数据，它有 40 个非空闲连接。

# 对 SQL 查询#3 有什么建议？

在这种情况下，建议检查具有顶级非空闲连接的 SQL 查询。大量的非空闲连接可能表明无效、不可扩展的架构或工作负载，与系统资源不匹配。

# SQL 查询#4 —详细的非空闲会话

列出耗时超过五秒的非空闲 PostgreSQL 会话，按运行时降序排序，如下例所示:

```
select 
	now()-query_start as runtime, 
	pid as process_id, 
	datname as db_name, 
	client_addr,
	client_hostname,
	query
from pg_stat_activity
where state!='idle'
and now() - query_start > '5 seconds'::interval
order by 1 desc;
```

如果结果集看起来太宽，修改后的查询可以如下例所示运行:

```
select 
	now()-query_start as runtime, 
	pid as process_id, 
	datname as db_name, 
	client_addr,
	client_hostname,
	substr(query, 1, 200) the_query
from pg_stat_activity
where state!='idle'
and now() - query_start > '5 seconds'::interval
order by 1 desc;
```

以下是 SQL 输出的一个示例:

```
runtime  | process_id | db_name  |  client_addr   | client_hostname | query              
----------+------------+----------+----------------+-----------------+----------------
 00:12:34 |       7770 | db_1     | 192.168.12.208 |                 | select …
```

对于每个长时间运行的查询，结果集包含相应的运行时、进程 id、数据库名称、客户机地址和主机名。

# 对 SQL 查询#4 有什么建议？

在某些情况下，长时间运行的查询会导致高 CPU 利用率。在这些情况下，应该对结果集中获得的查询进行分析和适当的调优。

如果查询运行时间过长，导致 DB CPU 和其他资源的高负载，您可能希望显式终止它。要通过<process id="">终止 PostgreSQL 数据库会话，请运行以下命令:</process>

```
select pg_terminate_backend(<process_id>);
```

# SQL 查询#5 —运行频繁的 SQL 查询

PostgreSQL 数据库中 CPU 利用率高的根本原因可能不是必需的长时间运行的查询。快速但过于频繁的查询每秒运行数百次也会导致高 CPU 利用率。

要查找最常见的 PostgreSQL 查询，请运行以下 SQL 查询，如下例所示:

```
with
a as (select dbid, queryid, query, calls s from pg_stat_statements),
b as (select dbid, queryid, query, calls s from pg_stat_statements, pg_sleep(1))
select
        pd.datname as db_name, 
        substr(a.query, 1, 400) as the_query, 
        sum(b.s-a.s) as runs_per_second
from a, b, pg_database pd
where 
  a.dbid= b.dbid 
and 
  a.queryid = b.queryid 
and 
  pd.oid=a.dbid
group by 1, 2
order by 3 desc;
```

下面是一个输出示例:

```
db_name              |            the_query             |      runs_per_second 
----------------------------------+------------------------------------------------------------
           db_1      | select ... from ... where ...    |               10
           db_1      | insert into ... values (...)     |               2
```

这个结果集包括一个查询列表和相应的频率:每个查询每秒运行多少次。

# 对 SQL 查询#5 有什么建议？

检查 SQL 查询和相应的应用程序逻辑。尝试改进应用程序架构以使用缓存机制。这将有助于防止在 PostgreSQL 数据库服务器上过于频繁地运行 SQL 查询。

# SQL 查询#6 —每个数据库和每个查询的 PostgreSQL 数据库 CPU 分布

该查询检查每个数据库中的每个查询使用了多少 CPU。它提供了一个结果集，该结果集按 CPU 占用率最高的查询降序排序。

对于 PostgreSQL 版本 12 和更低版本:

```
SELECT 
        pss.userid,
        pss.dbid,
        pd.datname as db_name,
        round(pss.total_time::numeric, 2) as total_time, 
        pss.calls, 
        round(pss.mean_time::numeric, 2) as mean, 
        round((100 * pss.total_time / sum(pss.total_time::numeric) OVER ())::numeric, 2) as cpu_portion_pctg,
        pss.query
FROM pg_stat_statements pss, pg_database pd
WHERE pd.oid=pss.dbid
ORDER BY pss.total_time 
DESC LIMIT 30;
```

对于 PostgreSQL 版本从 13:

```
SELECT 
        pss.userid,
        pss.dbid,
        pd.datname as db_name,
        round((pss.total_exec_time + pss.total_plan_time)::numeric, 2) as total_time, 
        pss.calls, 
        round((pss.mean_exec_time+pss.mean_plan_time)::numeric, 2) as mean, 
        round((100 * (pss.total_exec_time + pss.total_plan_time) / sum((pss.total_exec_time + pss.total_plan_time)::numeric) OVER ())::numeric, 2) as cpu_portion_pctg,
        pss.query
FROM pg_stat_statements pss, pg_database pd 
WHERE pd.oid=pss.dbid
ORDER BY (pss.total_exec_time + pss.total_plan_time)
DESC LIMIT 30;
```

如果输出文本太长，SQL 查询#6 可以修改如下:

对于 PostgreSQL 版本 12 和更低版本:

```
SELECT 
        pss.userid,
        pss.dbid,
        pd.datname as db_name,
        round(pss.total_time::numeric, 2) as total_time, 
        pss.calls, 
        round(pss.mean_time::numeric, 2) as mean, 
        round((100 * pss.total_time / sum(pss.total_time::numeric) OVER ())::numeric, 2) as cpu_portion_pctg,
        substr(pss.query, 1, 200) short_query
FROM pg_stat_statements pss, pg_database pd
WHERE pd.oid=pss.dbid
ORDER BY pss.total_time 
DESC LIMIT 30;
```

对于 PostgreSQL 版本从 13:

```
SELECT 
        pss.userid,
        pss.dbid,
        pd.datname as db_name,
        round((pss.total_exec_time + pss.total_plan_time)::numeric, 2) as total_time, 
        pss.calls, 
        round((pss.mean_exec_time+pss.mean_plan_time)::numeric, 2) as mean, 
        round((100 * (pss.total_exec_time + pss.total_plan_time) / sum((pss.total_exec_time + pss.total_plan_time)::numeric) OVER ())::numeric, 2) as cpu_portion_pctg,
        substr(pss.query, 1, 200) short_query
FROM pg_stat_statements pss, pg_database pd 
WHERE pd.oid=pss.dbid
ORDER BY (pss.total_exec_time + pss.total_plan_time)
DESC LIMIT 30;
```

以下是 SQL 输出的一个示例:

```
userid | dbid  | db_name | total_time  |  calls   |  mean   | cpu_portion_pctg |  short_query
 16409 | 16410 |  db_1   | 27349172.12 |  3905898 |    7.00 |           25.15  |  select ... from table_1
 16409 | 16410 |  db_1   |   391755.00 |      105 | 3731.00 |           16.76  |  select ... from table_2
 ...
```

从输出示例中，您可以看到:

*   第一个查询(即 select … from table_1)占用了 CPU 的大部分时间，因为调用次数很多。我建议查看应用程序运行连接到这个 PostgreSQL 数据库的查询的频率。
*   还要检查第二个查询(即 select … from table_2 ),因为执行该语句所花费的最高平均时间是 3731 ms，超过了三秒。

# SQL 查询#6 的建议是什么？

检查使用大量 CPU 或时间的 SQL 查询。此外，查找平均时间和/或调用次数较高的查询。

如果查询的输出指示“权限不足”，则应运行以下命令，如下例所示:

```
GRANT pg_read_all_stats TO <db_user>;
```

SQL 查询#5 和#6 基于 PostgreSQL 的 pg_stat_statements 扩展。pg_stat_statements 扩展允许跟踪在 PostgreSQL DB 实例上运行的 top/all SQL 语句的统计信息。在运行查询之前，应该为 PostgreSQL DB 实例启用 pg_stat_statements。

要启用 pg_stat_statements，请将 PostgreSQL 服务器配置参数 pg_stat_statements.track 设置为 TOP，并以管理员身份运行以下连接到 DB PostgreSQL 的命令:

```
create extension pg_stat_statements;
```

pg_stat_statements.track 配置参数控制模块统计哪些语句。指定 top 可跟踪顶级语句(由客户机直接发出的语句)，指定 all 还可跟踪嵌套语句(如函数内调用的语句)，指定 none 可禁用语句统计信息收集。默认值为 top。只有超级用户可以更改此设置。

要验证 pg_stat_statements 扩展是否已启用，以下命令应该返回一些正数的记录:

```
select count(1) from pg_stat_statements;
```

要重置 pg _ stat _ 语句收集的所有统计信息，请运行以下命令:

```
select pg_stat_statements_reset();
```

要将 PostgreSQL 查询结果保存到文件中，请使用以下方法:

```
postgres=> \o some_output_file.trc
postgres=> select * from some_table_1;
postgres=> select * from some_table_2;
postgres=> select * from some_table_3;
postgres=> \q
dmitryr@dmitryr-mac my_postgres % ls -rtogla some_output_file.trc
-rw-r--r--  1   59 Oct 24 23:32 some_output_file.trc
dmitryr@dmitryr-mac my_postgres %
```

# SQL 查询#7 —检查 PostgreSQL 数据库表统计信息

过时的 PostgreSQL 统计数据可能是高 CPU 利用率的另一个根本原因。当统计数据没有更新时，PostgreSQL 查询规划器可能会为查询生成低效的执行计划，这将导致整个 PostgreSQL DB 服务器的性能下降。

要检查特定数据库的 PostgreSQL 数据库服务器中每个表的统计信息的上次更新日期和时间，请连接到该数据库并运行以下查询:

```
select
  schemaname,
  relname,
  DATE_TRUNC('minute', last_analyze) last_analyze,
  DATE_TRUNC('minute', last_autoanalyze) last_autoanalyze
from
  pg_stat_all_tables
where
  schemaname = 'public'
order by
  last_analyze desc NULLS FIRST,
  last_autoanalyze desc NULLS FIRST;
```

以下是 SQL 输出的一个示例:

```
schemaname |                         relname                         |      last_analyze      |    last_autoanalyze    
------------+---------------------------------------------------------+------------------------+------------------------
 public     | my_table_1                                              | 2022-11-05 04:05:00+00 | 
 public     | my_table_3                                              | 2022-11-05 04:05:00+00 | 
 public     | my_table_2                                              | 2022-11-05 04:05:00+00 |
```

# 对 SQL 查询#7 有什么建议？

确保定期分析表格。

要手动收集特定表及其关联索引的统计信息，请运行命令:

```
ANALYZE <table_name>;
```

# SQL 查询#8 —检查 PostgreSQL 数据库膨胀

在密集数据更新的情况下，无论是频繁的更新还是插入/删除操作，PostgreSQL 表及其索引都会变得臃肿。膨胀是指由表或索引分配的磁盘空间，现在可供数据库重用，但尚未回收。由于这种膨胀，PostgreSQL 数据库服务器的性能下降，这可能导致高 CPU 利用率的情况。

在正常的 PostgreSQL 操作下，由于更新而被删除或失效的元组不会从表中物理删除，而是存储在那里，直到发出 VACUUM 命令。真空释放被“死”元组占据的空间。因此，有必要定期执行清理，尤其是对于经常变化的表。

要检查有关死元组的信息，以及针对特定数据库对 PostgreSQL 数据库服务器中的每个表运行 vacuum / autovacuum 的时间，请连接到该数据库并运行以下查询:

```
select 
  schemaname, 
  relname, 
  n_tup_ins, 
  n_tup_upd, 
  n_tup_del, 
  n_live_tup, 
  n_dead_tup, 
  DATE_TRUNC('minute', last_vacuum) last_vacuum, 
  DATE_TRUNC('minute', last_autovacuum) last_autovacuum
from 
  pg_stat_all_tables 
where 
  schemaname = 'public'
order by 
  n_dead_tup desc;
```

下面是一个输出示例:

```
schemaname | relname | n_tup_ins | n_tup_upd | n_tup_del | n_live_tup | n_dead_tup |      last_vacuum       |    last_autovacuum     

 public     | table_1 |  30237555 |  41784024 |     26858 |   30184398 |     142226 | 2022-11-05 04:00:00+00 | 
 public     | table_4 |  26204826 |         0 |  23898236 |    3628982 |      11688 | 2022-11-05 04:03:00+00 | 2022-11-04 16:13:00+00
 public     | table_2 |  25934622 |         0 |  23741303 |    3447190 |      11647 | 2022-11-05 04:03:00+00 | 2022-11-04 00:15:00+00
 public     | table_3 |    577825 |   4573009 |         0 |    6476040 |      11132 | 2022-11-05 04:04:00+00 |
```

# 对 SQL 查询#8 有什么建议？

确保桌子定期吸尘。

要对特定表及其所有关联索引运行 VACUUM(常规，非满),请运行命令:

```
VACUUM <table_name>;
```

可以通过自动真空处理和统计数据收集来运行真空。Autovacuum 是一个后台进程，它自动执行真空和分析命令。

# SQL 查询#9 —检查 PostgreSQL 数据库表统计和膨胀

您还可以将 SQL 查询#7 和#8 合并成一个 SQL 查询。

```
select 
  schemaname, 
  relname, 
  n_tup_ins, 
  n_tup_upd, 
  n_tup_del, 
  n_live_tup, 
  n_dead_tup, 
  last_vacuum, 
  last_autovacuum, 
  last_analyze, 
  last_autoanalyze 
from 
  pg_stat_all_tables 
where 
  schemaname = 'public'
order by 
  n_dead_tup desc;
```

# 对 SQL 查询#9 有什么建议？

获取从未分析或清空的表、很久以前分析过的表或自上次收集和清空 DB 统计数据以来发生了大量更改的表的列表。调整 autovacuum PostgreSQL 进程，以确保表或其索引的更改越频繁，执行 vacuum 和 analyze 的频率就越高。

要收集数据库统计信息并对 PostgreSQL 数据库实例的所有数据库的所有对象执行 vacuum(常规，非满)操作，请运行以下命令:

```
vacuumdb -h <db_host> -p <db_port> -U <db_user> -j 4 -z -v -a
```

要收集数据库统计信息并对 PostgreSQL 数据库实例的某个特定数据库的所有对象执行 vacuum(常规，非满)操作，请运行以下命令:

```
vacuumdb -h <db_host> -p <db_port> -U <db_user> -j 4 -z -v <db_name>
```

# 结论

在这篇博文中，我研究了针对 PostgreSQL 数据库中高 CPU 利用率问题的最全面、最有效的故障排除实践。我还提供了对不同类型的 SQL 查询的深入分类，这些查询可用于对高 CPU 利用率进行故障排除，以及如何使用这些查询的示例。这些知识无疑将成为任何生产任务关键型 PostgreSQL DB 环境的行动手册的一部分。