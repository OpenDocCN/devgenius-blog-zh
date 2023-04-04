# MS SQL Server 数据库的 Salts 和 UUIDs

> 原文：<https://blog.devgenius.io/salts-and-uuids-for-your-ms-sql-server-database-5f8d34b85265?source=collection_archive---------4----------------------->

一篇简明的帖子，介绍了在使用 MS SQL Server 数据库时 UUIDs 和密码加盐散列的实际实现步骤。

![](img/32e544a05629341d788ade6ab374f7c5.png)

# 介绍

本文是在使用 MS SQL Server 数据库时 UUIDs 和 Salts 的实际实现案例。将讨论的主题是:

*   使用数据库并创建 demousers 表
*   MS SQL Server UUIDs 使用内置的 **uuid()** 函数作为列数据类型
*   使用**触发器**
*   使用哈希验证存储的**函数**

# 先决条件

假设你理解基本的数据库概念， [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) 和[密码加盐和散列](https://www.okta.com/uk/blog/2019/03/what-are-salted-passwords-and-password-hashing/)。我之前的这篇文章对这些主题有所了解。

[](https://medium.com/@zzpzaf.se/salts-and-uuids-for-your-databases-intro-41cc58fe100a) [## 所有数据库的 Salts 和 UUIDs

### 这是一篇关于 Salts 和 UUIDs 的介绍性文章，也是我的其他文章中关于如何在…

medium.com](https://medium.com/@zzpzaf.se/salts-and-uuids-for-your-databases-intro-41cc58fe100a) 

此外，为了遵循本文的示例，您必须能够访问正在运行的 MS SQL Server 实例和 [sqlcmd](https://learn.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-ver16) CLI 工具[ [在这里](/the-database-command-line-tools-you-can-add-to-your-dev-environment-without-database-installation-9091dd0c0277?sk=50026b887181c062144e6fbb9130663e)您可以找到如何将它作为独立工具安装在您的系统中]。如果你愿意的话，你可以看看我下面关于如何创建和开始使用一个 [MS SQL Server 容器](https://medium.com/@zzpzaf.se/ms-sql-server-in-docker-b0397a55859c)的帖子。

[](https://medium.com/@zzpzaf.se/ms-sql-server-in-docker-b0397a55859c) [## Docker 中的 MS SQL Server

### 4 分钟指南。轻松全面！

medium.com](https://medium.com/@zzpzaf.se/ms-sql-server-in-docker-b0397a55859c) 

# 开始使用 MS SQL Server，创建一个新用户和 demousers 表

使用正确的凭据(用户名和密码)，使用 **sqlcnd** CLI 实用程序连接到正在运行的 MS SQL Server 实例。

```
~ sqlcmd -S 192.168.0.84,1443 -U SA -P 'Rpassw!1'
```

访问正在运行的 MS SQL Server 实例后，可以使用 CLI 工作。我们开始吧。

您可以创建一个新的数据库，或者只连接到您喜欢的数据库。在这里，我们将创建并使用名为“票证管理”的数据库，例如:

```
**>** CREATE DATABASE **[**ticket-management**]**;
**>** GO
**>** USE **[**ticket-management**]
>** GO
```

之后，创建一个新用户“user1 ”,密码为“Upassw！1”，然后为他分配票证管理数据库的所有权限。首先，对于 MS SQL Server，我们必须创建一个“登录”对象。“登录”对象授予对服务器的访问权限，然后“用户”授予对数据库的登录访问权限。首先，创建一个登录名:

```
**>** CREATE LOGIN **[**user1**]** WITH PASSWORD 'Upassw!1'
**>** GO
```

然后你可以使用下面的脚本来检查登录对象是否已经被创建:

以下是输出:

现在，切换到票证管理数据库，并为该数据库创建 user1:

```
**>** USE **[**ticket-management**]
>** GO
**>** CREATE USER user1 **FOR** LOGIN user1
**>** GO
```

然后，您可以使用以下脚本来检查是否已经创建了 user1:

输出如下所示:

下一步，我们必须向票证管理数据库的 user1 所有者添加' **db_owner** '角色。请注意，具有 db_owner 角色的任何人都可以对数据库执行所有配置和维护活动。

```
**>** EXEC sp_addrolemember 'db_owner', 'user1';
**>** GO
```

之后，注销并重新登录，作为用户 1:

```
~ sqlcmd -S 192.168.0.17,1443 -U user1 -P 'Upassw!1'
```

登录后，切换到票证管理数据库，确保您以用户 1 的身份登录，并且正在使用票证管理数据库。这就是以下 SQL 脚本的作用:

```
**>** SELECT @@SPID AS 'ID', SYSTEM_USER AS 'Login Name', USER AS 'User Name', DB_name**()** AS 'database'
```

以下是输出:

之后，在票证管理数据库/模式中创建一个新表(名为 DEMOUSERS ):

您也可以检查它:

就是这样。现在是时候看看如何在 MS SQL Server 中使用 UUIDs 了。

# MS SQL Server UUIDs

MS SQL Server 为我们提供了一个**内置的** [**uuid()**](https://learn.microsoft.com/en-us/sql/t-sql/functions/newid-transact-sql?view=sql-server-ver16) 函数，它符合 RFC4122，默认情况下返回一个 UUIDv4 字符串。简单地说，您可以执行 Select 语句:

此外，我们可以非常方便地将它用作表的 id 列的默认值。[注意，这种可能性与 PostgreSQL 非常相似]。因此，不需要使用 MS SQL Server 实现触发器来处理列的新 UUID。我们将这种类型用于我们希望的列，例如 id 列。

**创建一个新表 demousers 表**

因此，使用以下 SQL 脚本来创建我们的 demousers 表:

您还可以验证其模式:

此外，我们可以添加几个约束，这些约束在以后可能会非常有用。实际上，我们将为 username 列创建一个主键、一个索引和一个唯一约束:

```
**>** ALTER TABLE demousers ADD CONSTRAINT **[**PK_demousers1**]** PRIMARY KEY CLUSTERED **(**id**)
>** GO
**>** CREATE NONCLUSTERED INDEX index1 ON demousers **(**id ASC**)
>** GO
**>** ALTER TABLE demousers ADD CONSTRAINT uniques1 UNIQUE NONCLUSTERED **(**username ASC**)
>** GO
```

然后，如果您愿意，您可以使用下面提供的任何 SQL 命令获得演示用户的相关信息:

```
**>** EXEC sp_columns demousers
**>** GO
**>** EXEC sp_help demousers
**>** GO
**>** SELECT ***** FROM information_schema.columns WHERE table_name = 'demousers'
**>** GO
```

太好了！到目前为止，一切顺利！现在，通过向 demousers 表添加一个新用户，可以看到默认的 newid()函数对 id 列的影响了:

```
**>** INSERT INTO demousers **(**username, password**)** VALUES **('**panos', 'panospassw1'**)
>** GO
```

输出如下所示:

就是这样！如您所见，自动创建的“9 baea 927–3234–421 e-B561–6e 948d 4 FBE 5 e”UUID 已作为值正确插入 ID 字段！

# MS SQL Server 哈希和加盐

现在是时候创建一个**触发器**来完成这项工作了。你可以在[官方文档](https://learn.microsoft.com/en-us/sql/t-sql/statements/create-trigger-transact-sql?view=sql-server-ver16)中读到关于 MS SQL Server 触发器的内容。

MS SQL Server 提供的用于散列和加盐的重要加密函数是，我们将在这里使用:

*   [**CRYPT _ GEN _ RANDOM()**](https://learn.microsoft.com/en-us/sql/t-sql/functions/crypt-gen-random-transact-sql?view=sql-server-ver16)—该函数基于加密 API (CAPI)，它返回一个随机生成的加密数。
*   [**【HASHBYTES()**](https://learn.microsoft.com/en-us/sql/t-sql/functions/hashbytes-transact-sql?view=sql-server-ver16)—这是主哈希值生成器。它根据所提供的输入返回一个摘要(散列)。可以使用任何可用的算法(MD2、MD4、MD5、SHA、SHA1 或 SHA2)作为参数。
*   我们将使用的其他函数有 [CONVERT()](https://learn.microsoft.com/en-us/sql/t-sql/functions/cast-and-convert-transact-sql?view=sql-server-ver16) 和 [SUBSTRING()](https://learn.microsoft.com/en-us/sql/t-sql/functions/substring-transact-sql?view=sql-server-ver16) 。

下面是创建触发器的 SQL 脚本:

逻辑类似于我们在其他数据库中使用的散列函数。首先我们随机生成一个盐。然后，我们添加纯文本密码，对于整个字符串，我们使用 SHA-256 算法计算哈希值。最后，我们在计算出的散列前面添加 salt，并将其存储为密码字段/列的值。

创建触发器后，可以通过查询 sys_triggers 表来检查它是否存在:

以下是输出:

```
**>** SELECT name, create_date FROM sys.triggers WHERE type = 'TR';
```

现在，我们可以通过向票证管理表中插入一个新用户来测试触发器，然后查询表中的行:

```
**>** INSERT INTO demousers **(**username, password**)** VALUES **('**panos2', 'panospassw2'**)
>** GO
**>** Select ***** FROM demousers;
**>** GO
```

这是输出:

酷！正如你在上面看到的，它像预期的那样工作。

现在，让我们继续创建**验证**函数。

# 使用存储功能进行密码验证

类似于其他数据库，这里我们也可以通过使用 [**存储函数**](https://mariadb.com/kb/en/stored-function-overview/) 来实现。(你可以在[官方文档](https://learn.microsoft.com/en-us/sql/t-sql/statements/create-function-transact-sql?view=sql-server-ver16)中读到更多关于 MS SQL Server 存储函数的内容)。下面是一个例子:

如果你愿意，你可以使用下面的查询来检查存储函数是否已经被创建:

```
**>** SELECT name, definition, type_desc FROM sys.sql_modules m INNER JOIN sys.objects o ON m.object_id=o.object_id WHERE type_desc like ‘%function%’;
**>** GO
```

以下是输出:

如果匹配，则该函数返回 TRUE (1)，如果计算出的哈希不匹配，则返回 False (0)。为了检查给定的密码，我们可以使用下面的简单查询:

**>** 选择 ispassword valid**(**' panos 2 '，**)**；
**>** 走

从下面的输出可以看出，它返回 1 = TRUE，所以我们有一个匹配！

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

以上就是演示如何在 MS SQL Server 中开始处理 UUID、salts 和 hashes 的全部内容！

继续阅读我的其他帖子，了解其他数据库的实现示例。

暂时就这样吧！我希望你喜欢它！感谢您的阅读👏敬请关注！