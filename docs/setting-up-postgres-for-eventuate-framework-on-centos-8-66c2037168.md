# 在 CentOS 8 上为最终框架设置 Postgres

> 原文：<https://blog.devgenius.io/setting-up-postgres-for-eventuate-framework-on-centos-8-66c2037168?source=collection_archive---------9----------------------->

![](img/003a5a22993fb225671145dc32001f3e.png)

简·安东宁·科拉尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

本文描述了在 CentOS 8 上逐步安装 Postgres 来支持 eventuate 框架。 [Eventuate Tram](https://github.com/eventuate-tram/eventuate-tram-core) 是一个开源的事务消息传递库，它使用 [MySQL bin 日志协议](https://dev.mysql.com/doc/internals/en/binary-log-overview.html#)或 [Postgres Wal](https://www.postgresql.org/docs/9.0/wal-intro.html) 来读取对发件箱表所做的更改，并将这些事务作为消息发布给 Kafka 或 RabbitMQ 等消息代理。这篇文章的读者应该知道这个框架，因为这是一篇关于如何在 CentOS 8 上为 eventuate 框架设置 Postgres 的实用文章。

建议在虚拟机中建立数据库，而不是在生产环境中使用 docker 容器。最终框架的[示例基于这个](https://eventuate.io/exampleapps.html) [postgres docker 映像](https://hub.docker.com/r/eventuateio/eventuate-postgres)的 docker 映像。这个 docker 映像对于本地开发来说很好，但是当涉及到在生产中部署时，最好遵循最佳实践在虚拟机中安装数据库，这也是我撰写本文的动机。

**在 CentOS 8 上安装 Postgres 10**

在 CentOs 8 上以 root 用户身份执行以下命令。

1.  dnf 模块列表 postgresql
2.  dnf 安装 postgresql 服务器
3.  dnf 安装 postgresql10-devel(这是安装 wal2json 模块所必需的，该模块是 eventuate 框架所必需的)。
4.  导出路径="/usr/pgsql-10/bin:$PATH "。(将安装路径添加到路径中)

一旦你完成了以上四个步骤，Postgres 将被成功安装。下一步是安装最终框架所需的 [wal2json](https://github.com/eulerto/wal2json) 模块。

**安装 wal2json 模块**

执行以下命令。

1.  git 克隆[https://github.com/eulerto/wal2json](https://github.com/eulerto/wal2json)-b 主——单枝
2.  cd wal2json
3.  git check out d4c0e 814696695 bbf 853 c 48 b 38 b 7479 E0 f 83 f 6 c 7
4.  制作并制作安装
5.  激光唱片..
6.  rm -rf wal2json

一旦这些步骤成功完成，wal2json 模块就会被添加到 Postgres 库中。

**初始化最终框架的 Postgres】**

执行以下步骤

1.  通过执行以下命令，以 Postgres 用户身份登录。

> 苏后记

2.通过执行以下命令初始化 Postgres 数据库。

> pg _ CTL-D/var/lib/pg SQL/10/data/initdb

3.对/var/lib/pgsql/10/data/PostgreSQL . conf 进行以下更改。

> # MODULES
> shared _ preload _ libraries = ' wal 2 JSON '
> 
> # REPLICATION
> wal _ level = logical # minimal、archive、hot_standby 或 logical(更改需要重启)
> max _ wal _ senders = 16 # wal sender 进程的最大数量(更改需要重启)
> max_replication_slots = 10 #复制插槽的最大数量(更改需要重启)

4.将/var/lib/pgsql/10/data/PostgreSQL . conf 中的行 listen_addresses 编辑为 *listen_addresses = '*'*

5.对/var/lib/pgsql/10/data/pg _ HBA . conf 进行以下更改。

> 主人所有所有所有的信任

6.通过执行以下命令启动 Postgres 数据库。

> pg _ CTL start-D/var/lib/pg SQL/10/data/

**更改默认 Postgres 密码**

这是一个可选步骤，但建议这样做。执行以下命令来更改 Postgres 密码。

1.  psql
2.  \密码
3.  输入两次密码。

**初始化未来用户和数据库**

本节描述如何设置最终用户和最终数据库。根据需要替换$USERNAME，$PASSWORD，$DBNAME。

1.  导出 PGPASSWORD=
2.  psql -U postgres -c "使用密码' $PASSWORD '创建用户$USERNAME "
3.  create db-U postgres-O $ USERNAME $ DBNAME
4.  psql -U postgres -d $DBNAME -c "将数据库$DBNAME 的所有权限授予$ USERNAME"
5.  psql -d $DBNAME -U $USERNAME -c "创建模式$ DBNAME；"

**为最终方案创建表格**

本节描述如何为最终结果创建表。执行以下 SQL 语句来创建事件表。该查询假设 eventuate 是架构的名称。如果不是，用实际出现的模式替换出现的模式。

> 如果存在，则删除表。
> 
> 如果存在，则删除表。
> 
> 如果存在，则删除表。
> 
> DROP TABLE IF existing event uate . CDC _ monitoring CASCADE；
> 
> 创建表 eventuate.events(
> 
> event_id VARCHAR(1000)主键，
> 
> event_type VARCHAR(1000)，
> 
> event_data VARCHAR(1000)不为空，
> 
> entity_type VARCHAR(1000)不为空，
> 
> entity_id VARCHAR(1000)不为空，
> 
> 触发 _ 事件 VARCHAR(1000)，
> 
> 元数据 VARCHAR(1000)，
> 
> 已发布的 SMALLINT 默认值为 0
> 
> );
> 
> 在 eventuate.events(entity_type，entity_id，event_id)上创建索引 events _ idx
> 
> 在 eventuate.events 上创建索引 events _ published _ idx(published，event _ id)；
> 
> 创建表 eventuate.entities(
> 
> entity_type VARCHAR(1000)，
> 
> entity_id VARCHAR(1000)，
> 
> entity_version VARCHAR(1000)不为空，
> 
> 主键(实体类型，实体标识)
> 
> );
> 
> 在 eventuate.events(entity_type，entity_id)上创建索引 entities _ idx
> 
> 创建表最终快照(
> 
> entity_type VARCHAR(1000)，
> 
> entity_id VARCHAR(1000)，
> 
> entity_version VARCHAR(1000)，
> 
> snapshot_type VARCHAR(1000)不为空，
> 
> snapshot_json VARCHAR(1000)不为空，
> 
> 触发事件 VARCHAR(1000)，
> 
> 主键(实体类型、实体标识、实体版本)
> 
> );
> 
> 创建表 eventuate.cdc_monitoring(
> 
> reader_id VARCHAR(1000)主键，
> 
> 最后一次 BIGINT
> 
> );
> 
> 如果存在，则删除表。
> 
> 如果存在，则删除表 event uate . received _ messages CASCADE；
> 
> 创建表 eventuate.message(
> 
> id VARCHAR(1000)主键，
> 
> 目标 VARCHAR(1000)不为空，
> 
> 头 VARCHAR(1000)不为空，
> 
> 有效负载文本不为空，
> 
> 已发布的 SMALLINT 默认为 0，
> 
> 创建时间 BIGINT
> 
> );
> 
> 创建表 eventuate.received_messages(
> 
> consumer_id VARCHAR(1000)，
> 
> message_id VARCHAR(1000)，
> 
> creation_time BIGINT，
> 
> 主键(消费者标识，消息标识)
> 
> );
> 
> 创建表 eventuate.offset_store(
> 
> client_name VARCHAR(255)非空主键，
> 
> serialized_offset VARCHAR(255)
> 
> );
> 
> SELECT * FROM pg _ create _ logical _ replication _ slot(' event uate _ slot '，' wal 2 JSON ')；
> 
> SELECT * FROM pg _ create _ logical _ replication _ slot(' event uate _ slot 2 '，' wal 2 JSON ')；

确保以上所有查询都执行无误。

一旦所有这些步骤都完成了，我们就可以调出最终的服务，这些服务可以连接到我们建立的数据库。

如果这对你有用，请告诉我。