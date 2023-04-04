# 你从未听说过的 MySQL 备份工具

> 原文：<https://blog.devgenius.io/mysql-backup-tool-you-have-never-heard-about-3df35f7f0b6d?source=collection_archive---------11----------------------->

## 使用这个工具可以高效地备份您的 MySQL 数据库，而不会出现任何停机

![](img/50e1bea2d9f70b9c6e3010fa57750a2b.png)

由[罗伯特·阿纳奇](https://unsplash.com/@diesektion)在 Unsplash 上拍摄。编辑于 [Pixlr](https://pixlr.com)

您是否曾经不得不备份您的 MySQL 服务器数据？如果是这样，您可能已经使用了流行的 mysqldump 实用程序。

今天，我们将借助一款鲜为人知且完全免费的软件——**Percona xtra backup**；来自著名的 MariaDB 数据库的制作者。

mysqldump 是一个简单的数据到文本的转储工具。除了以文本形式保存的表中的数据，该文件还将包含 DDL 操作(即“创建表”)和 DML 操作(“插入”)形式的 SQL 查询。

mysqldump 可能适合小项目。**然而，在较大的应用程序中，事务处理的问题越来越多**。

想象一个 SaaS 应用程序出售对它的访问权。有一个用于用户帐户的“用户”表，由会计域使用，还有一个用于支付域的“发票”表。当一个新客户注册时，一个账户被创建，并且在短暂的片刻之后一张发票被发出。

现在我们运行 mysqldump，是在半夜凌晨 3 点，这个时候应用的使用率最低。不幸的是(或者不是)，就在这个时候，一个新的客户在这个过程中注册了！

可能会发生他的用户账号**不在【用户】表**中，但是在【发票】表**中会有发票**。

“user”表中的数据已经被转储，但是注册发生在导出“invoice”表之前。

这种事件的可能性随着时间的推移而增加。并且越频繁的数据写操作越有可能。试图从这样的转储中恢复数据库会导致错误。更不用说数据丢失了。

为了避免事务错误，在用`mysqldump`导出时，我们应该使用`--single-transaction`标志(对于 InnoDB)。但是，使用此标志会在导出过程中阻止写操作。

由于商业决策的原因，它的使用在某些时候可能是不可能的。毕竟，我们希望新客户随时注册。

# 制作更好的备份

此时 [Percona XtraBackup](https://docs.percona.com/percona-xtrabackup/2.4/backup_scenarios/full_backup.html#preparing-a-backup) 前来救援。它是一个允许您生成所谓的“热数据备份”的工具，而无需停止数据库或阻止表写入。

> 警告:目前 [XtraBackup 由于 InnoDB 引擎的变化而无法与 MySQL 版本 8.0.20](https://docs.percona.com/percona-xtrabackup/8.0/) 兼容

它甚至可以动态生成增量备份、压缩和加密备份；它有很多其他方便的特性，我不打算在这里介绍。

自 2012 年以来，我一直使用这个工具来备份一个大型 MySQL 服务器，其中包含我所在公司的几十个大型客户数据库。它仍然工作，每天在 CRON 中运行。

它可以在 Linux 下运行，但是也可以在其他平台上运行，比如 Docker / Windows WSL 等等。我不打算介绍该工具的所有特性，因为这需要专门的出版物。

# 演示应用程序

我已经创建了一个演示操场知识库，并在 GitHub 上开源:[https://github.com/dotcom-poland/xtrabackup-demo](https://github.com/dotcom-poland/xtrabackup-demo)

> 演示假设您已经安装了 *Docker* 。不使用 *Docker* 的话就要开始了。相信我，这个决定**会改变你的人生**。

演示很简单。只有几个命令:

1.  `make start`创建项目
2.  `make list_customers`将客户转储到控制台
3.  `make backup`创建备份
4.  `make corrupt`损坏数据库！
5.  `make list_customers`试图再次甩掉顾客
6.  `make restore`从备份中恢复数据库
7.  `make list_customers`验证客户是否已恢复
8.  停下来打扫一切

你会尝试一下吗？这是值得的。该文件可在[Percona.com](https://docs.percona.com/percona-xtrabackup/2.4/)获得。如果你有任何问题，请给我留言。下次见。