# Oracle 数据库的 Salts 和 UUIDs

> 原文：<https://blog.devgenius.io/salts-and-uuids-for-your-oracle-database-990af1e361a1?source=collection_archive---------2----------------------->

一篇简明的帖子，介绍了在使用 Oracle 数据库时 UUIDs 和密码加盐散列的实际实现步骤。

![](img/8a66c097e8aa7c6d9dcea3d2885cd658.png)

# 介绍

本文是在使用 Oracle 数据库时 UUIDs 和 Salts 的实际实现案例。将讨论的主题是:

*   使用数据库并创建 demousers 表
*   Oracle UUIDs —使用内置的 [**SYS_GUID()**](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/SYS_GUID.html#GUID-761E36B4-32DA-497D-8829-3D4653381F9B) **函数**
*   使用**触发器**
*   使用哈希验证存储的**函数**

# 先决条件

假设你理解基本的数据库概念， [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) 和[密码加盐和散列](https://www.okta.com/uk/blog/2019/03/what-are-salted-passwords-and-password-hashing/)。我之前的帖子对这些主题有所了解:

[](https://medium.com/@zzpzaf.se/salts-and-uuids-for-your-databases-intro-41cc58fe100a) [## 所有数据库的 Salts 和 UUIDs

### 这是一篇关于 Salts 和 UUIDs 的介绍性文章，也是我的其他文章中关于如何在…

medium.com](https://medium.com/@zzpzaf.se/salts-and-uuids-for-your-databases-intro-41cc58fe100a) 

此外，为了理解本文中的示例，您必须能够访问正在运行的 Oracle 数据库实例和 [SQLcl](https://docs.oracle.com/en/database/oracle/sql-developer-command-line/20.3/sqcug/working-sqlcl.html) CLI 工具[ [在这里](https://www.devxperiences.com/pzwp1/2022/10/30/the-database-command-line-tools-you-can-add-to-your-dev-environment-without-database-installation/#sqlcl)您可以找到如何将它作为独立工具安装在您的系统中]。如果你愿意的话，你可以看看我下面关于如何创建和开始使用 OracleDB 容器的帖子。

[](/oracle-database-in-docker-65da9c96ed56) [## Docker 中的 Oracle 数据库

### 5 分钟指南。轻松全面！

blog.devgenius.io](/oracle-database-in-docker-65da9c96ed56) 

# 开始使用 Oracle 数据库，创建新的模式/用户和 demousers 表

通过[**SQL**](https://docs.oracle.com/en/database/oracle/sql-developer-command-line/20.3/sqcug/working-sqlcl.html)实用程序连接到您的 Oracle 运行实例。下面我们使用一个命令作为 sysdba 直接连接到一个名为‘dockor 19 cpdb’的 [PDB(可插拔数据库)](https://docs.oracle.com/en/database/oracle/oracle-database/21/riwin/about-pluggable-databases-in-oracle-rac.html):

```
~ sqlcl sys/myPassw@192.168.0.17:1621/dockor19cPDB as sysdba
```

> [👉注意:上面的 **sql** 可执行文件已经从 sql 重命名为 **sqlcl**

访问 Oracle 运行实例后，您可以使用 CLI 来处理。我们开始吧。

您可以创建一个新的数据库，或者只连接到您喜欢的数据库。在这里，我们将创建并使用名为“票证管理”的数据库，例如:

**创建新用户(用户模式)**

您必须知道，在 Oracle 中，当您创建一个新用户时，会自动创建一个同名的模式。Oracle 模式可被视为类似于其他数据库中的“数据库”概念(例如，MariaDB/MySQL、PostgreSQL 或 MS SQL Server)。也就是说，让我们创建一个名为“ticket_management”、密码为“ticketPassw1”的新用户。

创建用户/模式后，更改它并授予必要的权限。然后，您可以查看用户，查询 dba_users。请参见下面的输出:

```
SQL> CREATE USER ticket_management IDENTIFIED BY "ticketPassw1";
User TICKET_MANAGEMENT created. 
SQL>SQL> GRANT connect, resource, create any context TO TICKET_MANAGEMENT;
Grant succeeded. 
SQL> ALTER USER TICKET_MANAGEMENT quota unlimited on users; 
User altered.
SQL>
```

> [👉注意: [Oracle 不建议对数据库对象名使用带引号的标识符。SQL*Plus 接受这些带引号的标识符，但在使用管理数据库对象的其他工具时，它们可能无效]。](https://docs.oracle.com/cd/E11882_01/server.112/e41084/sql_elements008.htm#SQLRF51129)

请参见下面的命令输出，并检查用户对象是否已创建:

之后，注销并作为**票证管理**用户重新登录，并确保/检查您已正确登录:

```
~ sqlcl TICKET_MANAGEMENT/ticketPassw1@192.168.0.17:1621/DOCKOR19CPDB
```

之后，在票证管理数据库/模式中创建一个新表(名为 DEMOUSERS ):

现在我们准备看看如何使 ID 列成为 UUID 列。

# Oracle 数据库 UUIDs

**Oracle**[**SYS _ GUID()**](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/SYS_GUID.html#GUID-761E36B4-32DA-497D-8829-3D4653381F9B)**函数**

Oracle 提供了内置的 [**SYS_GUID()**](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/SYS_GUID.html#GUID-761E36B4-32DA-497D-8829-3D4653381F9B) 函数，可用于生成全局唯一标识符。它实际上返回一个随机的 16 字节原始值(32 个十六进制字符)，如下所示:

请注意，生成的值不遵循任何 UUID 标准。然而，我们可以对结果稍加阐述，使它看起来像一个 UUID 标准。例如，我们可以使用 Oracle [RegEx](https://docs.oracle.com/en/database/oracle/oracle-database/21/adfns/regexp.html) 将其格式化，看起来像 UUID:

然而，有一个需要注意的地方是，它实际上会产生一个序列，使得它不是真正随机的。如果你多次重复调用它，你会注意到它:

![](img/f76e91062c70ebee679a1dd493d68abb.png)

因此，更好的解决方案可能是使用 Oracle 的功能来创建加密安全的伪随机字节序列，该序列也可以由自定义存储函数精心设计以兼容 UUID。

**Oracle**[**DBMS _ CRYPTO**](https://docs.oracle.com/en/database/oracle/oracle-database/21/arpls/DBMS_CRYPTO.html#GUID-1C98C203-29EF-488D-A5FA-42AD4BD7718D)**包**

为此，我们必须使用 [DBMS_CRYPTO](https://docs.oracle.com/en/database/oracle/oracle-database/21/arpls/DBMS_CRYPTO.html#GUID-1C98C203-29EF-488D-A5FA-42AD4BD7718D) 包，它提供了一个我们可以使用的[加密伪随机数生成器](https://docs.oracle.com/en/database/oracle/oracle-database/21/arpls/DBMS_CRYPTO.html#GUID-FAC58F76-A0EA-4FB4-B31F-745B3F7BF75F)。

请注意，DBMS_CRYPTO 是一个包，默认情况下由 SYS 模式拥有。因此，在使用它之前，您必须将 execute 特权授予您要使用它的用户/模式:

```
GRANT EXECUTE ON dbms_crypto TO TICKET-MANAGEMENT;
```

请注意，DBMS_CRYPTO 包也是我们稍后将看到的 salt 散列所需要的。

现在我们的模式中有了 DBMS_CRYPTO，我们可以继续了。

首先，我们将定义一个自定义存储的 [**函数**](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/Functions.html) 来创建一个随机生成的 UUID，之后，我们将创建一个触发器来使用它。

下面给出了两个示例函数。它们都使用 sys.dbms_crypto.randombytes()函数创建一个由 16 个十六进制字符(8 个字节)组成的随机字符串。然而，第二个函数使用一些正则表达式“炼金术”来使返回结果 UUIDv4 兼容。存储函数的名称分别为“RUUID”和“RUUIDv4”。

示例 1

示例 2

正如我们所说，这是一个 UUID 版本 4 兼容的功能。我们只需在前面的函数中添加一行代码就可以做到这一点，该函数实际上用版本号指示符 4 替换了随机生成的 32 字节十六进制字符串的第 13 个十六进制字符。

现在您可以检查这些函数以及它们返回的内容:

```
SQL> SELECT OBJECT_NAME FROM user_objects WHERE object_type IN('FUNCTION', 'PROCEDURE');
SQL> SELECT RUUID() FROM DUAL; 
SQL> SELECT "RUUIDv4"() FROM DUAL;
```

到目前为止一切顺利。现在，是进行下一个必要步骤的时候了。它只是创建一个新的 [**触发器**](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/CREATE-TRIGGER.html) ，它将在每次插入新记录时被触发。没什么特别的。它只是使用先前创建的函数:

就是这样！现在，让我们通过向表中添加一个新用户来测试它:

```
SQL> INSERT INTO DEMOUSERS (username, password) VALUES ('panos', 'panospassw1');
```

酷！如您所见，自动创建的“8 B1 d3c 05–8 FAA-463 a-D4CC-105252 caae 13”UUID 已作为值正确插入 ID 字段！

# Oracle 数据库散列和加盐

下面是一个简单的 SQL 脚本，它创建了自动存储加盐哈希密码的触发器:

如果您愿意，可以使用下面的 sql 命令列出到目前为止您的所有触发器:

```
SQL> SELECT OBJECT_NAME FROM user_objects WHERE object_type = ‘TRIGGER’;
```

创建“HASH_PASSWORD_INSERT”触发器后，我们可以通过插入一个新用户，然后从 DEMOUSERS 表中选择所有行来测试它:

```
SQL> INSERT INTO DEMOUSERS (username, password) VALUES ('panos2', 'panospassw2'); 
SQL> SELECT * FROM DEMOUSERS;
```

输出如下所示:

正如你在上面看到的，触发器工作正常。但是，我们还需要一个存储函数来检查给定的密码是否对应于 DEMOUSERS 表的 password 列中存储的散列密码。

以下是此类存储函数的一个示例:

该函数接受两个 VARCHAR2 参数作为用户名和密码。首先，它使用提供的用户名获取现有的用户密码。然后，提取 salt，它实际上是密码的前 16 个字符的字符串。接下来，使用相同的算法重新计算新的哈希值。最后将新计算的散列值与现有散列值进行比较，现有散列值是现有密码串的最后一部分(从第 17 个字符直到末尾)。如果匹配，则返回 TRUE (1)，否则，返回 FALSE (0)。

最后，让我们检查一下:

```
SQL> SELECT ISUSERPASSWORDVALID('panos2', 'panospassw2') FROM DUAL;
```

输出如下所示:

正如你所看到的，它返回 1 =真，所以我们有一个匹配！注意，在您的后端/中间件中，您可以创建相同的查询字符串来调用存储的函数！酷！

好了，这就是我们演示如何使用 Oracle 数据库开始处理 UUID、盐和散列的全部内容！

继续阅读我的其他帖子，了解其他数据库的实现示例。

暂时就这样吧！我希望你喜欢它！
感谢阅读👏敬请关注！