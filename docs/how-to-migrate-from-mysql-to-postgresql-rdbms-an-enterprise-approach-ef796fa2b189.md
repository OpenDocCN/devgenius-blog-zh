# 如何从 MySQL 迁移到 PostgreSQL RDBMS:企业方法

> 原文：<https://blog.devgenius.io/how-to-migrate-from-mysql-to-postgresql-rdbms-an-enterprise-approach-ef796fa2b189?source=collection_archive---------14----------------------->

这篇文章最初发表在 JFrog 的博客上。

![](img/7b3b50dbc9b77725a75f8a7ac1594d3c.png)

# 1.将 MySQL 迁移到 PostgreSQL 的介绍。

作为 JFrog 仅支持 PostgreSQL 数据库(DB)的 SaaS 战略的一部分，JFrog 需要将 SaaS 现有客户的数据库从 MySQL 迁移到 PostgreSQL。

必须开发一个完全自动化、可靠且稳定的迁移流程，实现零数据丢失和最短的停机时间，能够迁移数十/数百 GB 的数据库，并保证现有应用程序能够透明地处理迁移的数据库。

我们开发了一个基于 pgloader 开源实用程序[【https://github.com/dimitri/pgloader】T2]的 MySQL2PG 进程来满足这种需求。

# 2.将数据库从 MySQL 迁移到 PostgreSQL 的组件/拓扑是什么？

![](img/f869f3e4dcf64044655ad0121434d439.png)

DB 迁移过程的基本拓扑包括源数据库(MySQL)、目标数据库(PostgreSQL)和迁移机器。pgloader 从迁移机器上运行，从源数据库(MySQL)读取数据，并将其写入目标数据库(PostgreSQL)。

迁移机器应该具有到源和目标数据库的直接、可靠和高吞吐量的连接。迁移机器的典型配置包括 16 个 CPU 和 64 GB RAM、至少 10 Gbps 的网络带宽和至少 500GB 的存储空间。

为了保证数据库对象的兼容性、它们的结构以及与特定数据类型的一致性，数据库迁移过程首先运行适合特定类型/版本产品的 DDL 脚本。然后，它将数据复制到预先准备好的目标数据库模式中。

pgloader 没有将数据导出到转储中，没有将转储放在迁移机器上，也没有让 DBs 数据占用存储空间。它在迁移机器 RAM 中创建了一种管道进程，并从源数据库中选择数据并将其复制到目标数据库中。

# 3.迁移机器的典型操作系统是什么？

操作系统:Ubuntu 20.04

lsb_release -a
没有 lsb 模块可用。
发行商 ID: Ubuntu
描述:Ubuntu 20.04.2 LTS
发布时间:20.04
代号:focal

# 4.建议在迁移机器上安装什么？

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql-13
sudo apt-get install -y pgloader
sudo apt-get install mysql-client
```

# 5.如何运行 pgloader 将一个 DB 从 MySQL 迁移到 PostgreSQL？

创建 pgloader 配置文件:

```
cat pgloader.confLOAD DATABASE
FROM mysql://src_db_user:src_db_pwd@src_db_host/src_db_name?sslmode=<...>
INTO postgresql://dest_db_user:dest_db_pwd@dest_db_host/dest_db_name;
```

运行 pgloader:

```
pgloader pgloader.conf
```

# 6.如何运行 MySQL2PG pgloader 作为后台进程？

为了避免 MySQL2PG 迁移过程的中断，建议将 pgloader 作为后台进程运行。该过程包括以下步骤。

创建 pgloader 配置文件:

```
cat pgloader.confLOAD DATABASE
FROM mysql://src_db_user:src_db_pwd@src_db_host/src_db_name?sslmode=<...>
INTO postgresql://dest_db_user:dest_db_pwd@dest_db_host/dest_db_name;
```

创建一个 shell 脚本，将 pgloader 作为后台进程运行:

```
cat run_pgloader_background.shnohup pgloader pgloader.conf 2>&1 &
```

运行脚本:

```
run_pgloader_background.sh
```

运行跟踪将保存在文件中:

```
nohup.out
```

注意事项:

通常

```
src_db_user = dest_db_user,
src_db_pwd = dest_db_pwd,
src_db_name = dest_db_name
```

# 7.pgloader 的典型输出跟踪是什么样的？

![](img/0642fa7a06136f6632e1c1f03de4d9c0.png)

# 8.如何确保迁移成功完成？我如何确保零数据丢失？

验证 pgloader 的跟踪:它应该没有错误，并且读取和导入的值应该匹配。

# 9.如何从 pgloader 进程中排除表？

如果源数据库包含可以从迁移过程中安全排除的表，这可以通过 pgloader 的“排除表名匹配”配置来完成。

示例:

```
LOAD DATABASE
FROM mysql://src_db_user:src_db_pwd@src_db_host/src_db_name?sslmode=<...>
INTO postgresql://dest_db_user:dest_db_pwd@dest_db_host/dest_db_nameEXCLUDING TABLE NAMES MATCHING 'table_to_exclude_one','table_to_exclude_two','table_to_exclude_three';
```

# 10.“堆筋疲力尽，游戏结束”的问题。

来自 pgloader 的“Heap exhausted，game over”消息可能表明 pgloader 默认的专用 4GB RAM 对于给定的 DB 迁移运行来说是不够的。

这种情况下的解决方案是使用用更广泛的 DYNSIZE 参数编译的 pgloader。DYNSIZE 参数允许修改 pgloader 映像在运行数据时允许自己使用的默认内存量。

# 11.如何编译 pgloader 使用更多 RAM？

pgloader 实用程序没有使用主机上所有可用的 RAM。pgloader 可以使用的 RAM 数量在编译阶段定义。要使用更多的 RAM，应该编译定制版本的 pgloader。以下分步指南说明了如何使用自定义参数 DYNSIZE 编译 pgloader。DYNSIZE 参数允许在运行数据时修改 pgloader 映像。

```
pgloader compilation
--------------------Build pgloader from sources(1) Check OS:dima@dima-VirtualBox:~$ lsb_release  -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.3 LTS
Release:        20.04
Codename:       focal
dima@dima-VirtualBox:~$(2) Install git:sudo apt updatesudo apt install git(3) Clone pgloader:mkdir my_pgloader
cd my_pgloader/
git clone https://github.com/dimitri/pgloader.git(4) install packages necessary for build:sudo apt-get install sbcl unzip libsqlite3-dev make curl gawk freetds-dev libzip-dev(5) make build: make DYNSIZE=10240 pgloader(6) make outputs a ./build/bin/pgloader file for us to use.dima@dima-VirtualBox:~/my_pgloader/pgloader/build/bin$ pwd
/home/dima/my_pgloader/pgloader/build/bin
dima@dima-VirtualBox:~/my_pgloader/pgloader/build/bin$ ls -rtogla
total 69160
-rw-rw-r-- 1       70 Jan  7 17:17 .gitignore
drwxrwxr-x 5     4096 Jan  7 17:29 ..
-rwxr-xr-x 1 41918800 Jan  7 17:29 buildapp.sbcl
-rwxr-xr-x 1 29036008 Jan  7 17:29 pgloader
drwxrwxr-x 2     4096 Jan  7 17:29 .
dima@dima-VirtualBox:~/my_pgloader/pgloader/build/bin$cp pgloader pgloader_dima_dynsize_10240dima@dima-VirtualBox:~/my_pgloader/pgloader/build/bin$ ls -rtogla
total 97396
-rw-rw-r-- 1       70 Jan  7 17:17 .gitignore
drwxrwxr-x 5     4096 Jan  7 17:29 ..
-rwxr-xr-x 1 41918800 Jan  7 17:29 buildapp.sbcl
-rwxr-xr-x 1 29036008 Jan  7 17:29 pgloader
-rwxr-xr-x 1 29036008 Jan  7 17:33 pgloader_dima_dynsize_10240
drwxrwxr-x 2     4096 Jan  7 17:33 .
dima@dima-VirtualBox:~/my_pgloader/pgloader/build/bin$dima@dima-VirtualBox:~/my_pgloader/pgloader/build/bin$ ./pgloader_dima_dynsize_10240 --version
pgloader version "3.6.a94a0a3"
compiled with SBCL 2.0.1.debian
dima@dima-VirtualBox:~/my_pgloader/pgloader/build/bin$
```

# 12.如何管理“对等端重置连接”pgloader 问题？

pgloader 运行期间的“Connection reset by peer”错误消息通常表示 pgloader 连接 MySQL [源数据库]超时。

要解决此问题，我们建议将以下参数添加到 pgloader 配置脚本中:

```
SET MySQL PARAMETERS
net_read_timeout = '5000',
net_write_timeout = '5000'
```

# 13.如何改善 pgloader 迁移过程的持续时间？

有几种方法可以缩短 pgloader 迁移过程的持续时间:

–纵向扩展源和目标数据库
–确保源和目标数据库上无负载/大量活动/足够的存储、RAM、连接和资源
–使用使用定制参数 DYNSIZE
编译的 pgloader–纵向扩展迁移机器以允许更多 RAM
–使用以下参数运行 pg loader:

```
SET PostgreSQL PARAMETERS
maintenance_work_mem to '512MB',
work_mem to '48MB'
```

# 14.如何配置 pgloader 在大数据库/大数据量上稳定可靠运行？

–使用用定制的 DYNSIZE 参数编译的 pgloader。它将允许更多的 RAM 专用于 pgloader 进程
–使用以下 pgloader 配置:

```
cat pgloader.confFROM LOAD DATABASE
FROM mysql://db_user:db_pwd@src_db_host/db_name?sslmode=<...>
INTO postgresql://db_user:db_pwd@dest_db_host/db_nameWITHdata only, create no indexes,workers = 8, concurrency = 1,
multiple readers per thread, rows per range = 10000,
batch rows = 10000SET PostgreSQL PARAMETERS
maintenance_work_mem to '512MB',
work_mem to '48MB'SET MySQL PARAMETERS
net_read_timeout = '5000',
net_write_timeout = '5000'EXCLUDING TABLE NAMES MATCHING 'table_to_exclude_one','table_to_exclude_two','table_to_exclude_three'ALTER SCHEMA 'db_schema_name' RENAME TO 'public';
```

# 15.最小的 pgloader 配置会是什么样子？

```
FROM LOAD DATABASE
FROM mysql://db_user:db_pwd@src_db_host/db_name?sslmode=<...>
INTO postgresql://db_user:db_pwd@dest_db_host/db_namewith
batch size = 2048 kB,
batch rows = 2000,
prefetch rows = 2000SET PostgreSQL PARAMETERS
maintenance_work_mem to '512MB',
work_mem to '48MB'SET MySQL PARAMETERS
net_read_timeout = '5000',
net_write_timeout = '5000'EXCLUDING TABLE NAMES MATCHING 'table_to_exclude_one','table_to_exclude_two','table_to_exclude_three'ALTER SCHEMA 'db_schema_name' RENAME TO 'public';
```

# 16.为什么在迁移过程之前、之中和之后监控源、目标数据库和迁移机器很重要？

为了保证 pgloader 的稳定、可靠和最佳工作，在迁移之前、期间和之后监控源和目标数据库是至关重要的。DB 的机器应该足够强大，并且它们需要足够的资源来执行 pgloader 密集型活动。源和目标 DBs 机器都不应该被繁重的活动或密集的 CPU 操作或高读或(和)写负载。

注意:源或目标数据库机器上的任何负载或繁重活动都会直接影响数据库迁移的性能和成功。

此外，在迁移过程中监控迁移机器的 CPU、IOPs、内存和网络也很重要。

# 17.分割对源数据库、目标数据库和迁移机器的重要性。

将源数据库实例、目标数据库实例和迁移机器分开很重要。组合这些组件将影响数据库迁移的性能和稳定性。

# 18.在迁移过程中，可以查询目标 PostgreSQL 数据库吗？通过“动态”查询复制的表，我们会看到数据库迁移的进度吗？

虽然我们可以在迁移过程中看到目标 PostgreSQL 数据库上的实时会话，但实际数据只是在迁移结束时才提交。

在 pgloader 运行过程中查询任何填充的表都是无用的:它们的数据被放在脏的、尚未提交的块上。只有当 pgloader 完成后，我们才能看到查询“select * from”的目标数据库上的实际记录