# 用神奇宝贝数据集在 Jupyter 笔记本上练习 SQL

> 原文：<https://blog.devgenius.io/practising-sql-on-jupyter-notebook-with-pok%C3%A9mon-dataset-e2d90971684e?source=collection_archive---------10----------------------->

## 使用连接器/Python 连接到 MySQL

我最近一直在复习我的 SQL 技能。厌倦了 Sakila 示例数据库，我认为使用我能联系起来的数据可能会令人耳目一新。虽然 Spotify 数据集是一个受欢迎的选择，但由于编码问题中断了 csv 文件的导入，我最终选择了 [*神奇宝贝数据集*](https://www.kaggle.com/rounakbanik/pokemon) 。

![](img/248a50591ea9ab55ec5f4e29de51e82e.png)

抱歉皮卡丘。卡比兽是最棒的，不是你。

在这篇文章中，我将分享:

1.  我如何预处理数据集；
2.  如何将数据导入 MySQL 数据库；
3.  如何使用 MySQL 连接器/Python；和
4.  探索性数据分析(EDA)对来自数据库的数据进行 ETL。

如果您只对某些部分感兴趣，请随意跳过。

## 数据预处理

数据集来自一个 801*41 单元格的大 csv 文件。我做的第一件事是将它分解成更小的表(以便我们可以在 SQL 中执行 join 和其他操作)。列分组如下:

1.  基本信息:神奇宝贝编号、名称、身高、体重和分类。
2.  统计:神奇宝贝编号，HP，攻击，防御，SP 攻击，SP 防御，神奇宝贝速度。
3.  类型:各个神奇宝贝的神奇宝贝编号和类型。
4.  不同移动类型的系数:神奇宝贝编号和每个神奇宝贝所有 19 种移动类型的系数。
5.  能力:神奇宝贝的编号和能力。
6.  其他信息:神奇宝贝编号，完成成长所需的经验，捕获率，男性百分比，基础蛋步数，基础快乐，神奇宝贝的世代以及神奇宝贝是否是传奇。
7.  日文名称:神奇宝贝编号和神奇宝贝的日文名称。

导入库，修复错别字，替换单词和创建不需要处理的表格

“Pokémon”一词已从分类栏的所有单元格中完全删除，以避免在数据导入过程中因非英语字符导致的编码问题。ID 列被添加到每个表中，因为在 MySQL 中创建的表需要主键。

类型和能力被挑选出来，因为一个神奇宝贝可以有多种类型和能力。在像 MySQL 这样的关系数据库中，一个单元格应该只有一个值(除非你想要更艰难的生活)。在原始数据集中，有“类型 1”和“类型 2”列。它们被重新排列成一列。“能力”列中的字符串首先被删除了不必要的字符，然后被拆分和扩展成多个列。最后，它们被展平成一列。

展平类型和能力列并导出为 csv

在此阶段，csv 文件已准备好导入。

另一方面，我试图将原始数据集作为一个整体导入，但失败了，尽管该文件是以 UTF-8 编码的，并且在尝试导入时设置正确。我怀疑这是因为出现了非英语字符，如é和日语字符。谷歌搜索显示，许多人在 MySQL 上遇到了类似的问题，而且没有单一有效的解决办法。有些人能解决它，但不是我。因此，我采取了预防措施，没有导入日本姓名列。后来我试着单独进口，效果不错。那么，为什么不对日本姓名列进行预处理呢？

将日文名称拆分为片假名和 rō maji

创建的数据帧应该如下所示:

![](img/db1893fb779c9cb7860a366687f85f1e.png)![](img/3b0b4fb3d304f30e985b856fb6e41de9.png)![](img/709b9dc1c03f70d390f5c201fcda21ca.png)

## 将数据导入 MySQL 服务器

有几种方法可以做到这一点。基本上，他们是

1.  使用 SQL 命令，即 [*加载数据语句*](https://dev.mysql.com/doc/refman/8.0/en/load-data.html)；和
2.  使用 MySQL Workbench 中的 [*表数据导入向导*](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-table.html) 。

我两个都试过，导入向导显然更容易使用。您不需要学习任何命令。您不需要考虑如何处理空值。数据导入只需点击几下鼠标。根据我的经验，只要在 Excel 中打开 csv，在空白处填入“null”，导入向导就会正确地将其作为 NULL 值导入。

综上所述，LOAD DATA 语句并不太难使用。您只需要在导入之前创建所有需要的表。列顺序和数据类型必须与要导入的 csv 文件相匹配。如果有空值，您必须知道它们在哪个列中，并指定如何在语句中处理它们。如果您是高级用户，您可能会发现这种方法在某些情况下更好，因为它允许您自定义流程的行为。下面是创建表和导入数据的 SQL 代码。

创建数据库、表格和导入数据

在上面的代码块中，我包括了 LOAD DATA 语句的基本结构、每行的注释以及一个真实的例子。

为表创建了 id，因为 Pokédex 号不能用作索引。理论上，有些神奇宝贝有两种或两种以上的形态。如果包括其他形式的数据，一些神奇宝贝将会有多个记录。其中的索引号是重复的(不是唯一的)，因此不能作为主键。但是，它可以是非唯一索引，即 MUL 键。

根据您的服务器设置，您可能会也可能不会从您喜欢的任何文件目标导入数据。这是 MySQL 的一个安全特性，与系统变量“ [secure_file_priv](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_secure_file_priv) ”有关。如果您不希望禁用它，可以使用下面这行代码检查允许数据导入的路径:

```
SHOW VARIABLES LIKE “secure_file_priv”;
```

如果您尝试从另一个目标导入，它将返回以下错误消息:

```
Error Code: 1290\. The MySQL server is running with the — secure-file-priv option so it cannot execute this statement
```

您可以使用 MySQL 连接器/Python、shell 甚至命令行客户端随意创建表和加载数据。然而，出于几个原因，我更喜欢使用 MySQL workbench 来完成这个过程。

1.  有两个选项可用:SQL 命令或表数据导入向导。
2.  不知何故，通过 MySQL Connector/Python 导入并不总是返回错误消息，即使过程出错也是如此(也许这只是我的问题)。同时，workbench 或 shell 肯定会返回这样的消息:

![](img/98ab4b38cd40e91f5a74b8ffb0113a6c.png)

MySQL 工作台中的错误消息

从这些信息中，你可以知道到底哪里出了问题。您知道记录是被删除还是被跳过。表数据导入向导还可以显示用于诊断的导入日志。

## 连接到 MySQL 服务器

在进行第一次查询之前，您必须安装 MySQL Connector/Python。因为我用的是阿纳康达的 Jupyter 实验室，所以我刚刚查了 anaconda.org 的包裹。

```
conda install -c conda-forge mysql-connector-python
```

只需使用上面的命令将软件包安装到您的环境中。

连接器非常容易使用。要建立或关闭连接，您只需要下面几行代码。

建立连接并创建光标

建立连接后，有趣的部分来了:选择你喜欢的任何东西！为此，您需要创建一个游标。

## 查询表

游标允许您执行 SQL 语句、获取返回的行以及做许多其他事情。最基本的方法是:

1.  cursor.execute("您的 SQL 语句")
2.  cursor.fetchall()
3.  游标.列名

下面是一些演示如何使用它们的示例代码。

查询该表，获取结果并创建一个表来显示结果

输出应该如下所示:

![](img/5ea4ac73ce1ecf974cdec5be0eb01a40.png)

3 种最常见和 3 种最不常见的神奇宝贝

fetchall()方法返回的原始输出是一个元组列表。如果返回的结果集像这样小，直接读取就可以了。否则，随着结果集变大，它会变得更加不可读。熊猫数据框也更容易操作。

请注意，您可以选择使用 fetch one()和 fetchmany()方法一次获取一行或任意数量的行。但是您必须在执行新语句之前获取所有行。

## 探索性数据分析

MySQL 和 Python 的集成意味着你可以用你最喜欢的 Python 包方便地可视化和操作数据。我认为这比使用 MySQL workbench 有很大的优势(至少对于练习编码来说)。下面是一个从数据库进行数据 ETL 的 EDA 示例。

首先，通过下面的 SQL 语句，我创建了一个临时表，总结了神奇宝贝类型、属性、能力的信息，它们在不同类型的移动中表现如何，以及它们是否是传奇。

为我需要的信息创建临时表

然后，我从这个总结中选择了所有的，并从中制作了一个数据框架。

输出应该如下所示:

![](img/ff96e11b24f12b9922ea8852314c9298.png)

神奇宝贝的统计数据，类型，能力等的摘要

您可能会注意到，我使用了不同的方法来检索列名。描述方法基本上与 SHOW COLUMNS 语句相同，它提供关于数据类型、是否允许 null 等信息。

关于返回数据的数据类型的说明:

1.  从查询中检索的字符串数据有时需要清理，尤其是那些受 GROUP_CONCAT()函数和 GROUP BY 语句影响的数据。经常可以找到回车符“\r”。
2.  SUM()函数产生的整数值以某种方式被转换为 decimal 类型，这通常不是问题。奇怪的是，当我试图用熊猫来绘制时。DataFrame.plot 方法，它返回“ **TypeError** :没有要绘制的数值数据”。将数据转换为 int64 解决了这个问题。
3.  MySQL 中没有布尔数据类型。而是使用 TINYINT(通常为 0 和 1)。然后，可以使用 pandas 将 0 和 1 分别转换为 false 和 true。DataFrame.astype(bool)。

下一步是绘制数据。我通过在 3D 散点图上绘制 5 维数据做了一点实验，用颜色和标记形状作为第 4 维和第 5 维。x、y 和 z 轴代表状态的总和、不同移动的系数总和以及能力数量。对于类型编号和神奇宝贝是否是传奇，它们只有两个可能的值(即布尔)。因此，它们可以用两种不同的标记和颜色来表示。为此，我需要将数据分成 4 组:

1.  不传奇/类型单一；
2.  不传奇/双型；
3.  是传奇/单一类型；和
4.  是传奇/双型。

将数据分成 4 组

最后一步是通过遍历我刚刚创建的数据字典来创建绘图。

通过循环刚刚分割的数据进行绘图

3D 散点图如下所示:

![](img/e2a6a11a9fe1530549567821e1f370ba.png)

这不是一个很好的可视化，但它清楚地显示了与普通神奇宝贝相比，传奇神奇宝贝(我的兴趣)是如何分布的。

1.  传说中的神奇宝贝只有 1 到 3 种能力。他们拥有的能力越多，就越稀有。另一方面，大多数普通的神奇宝贝有 3 种能力，最多可以有 6 种能力。
2.  大多数传说中的神奇宝贝都是双类型的。
3.  拥有 3 个以上能力的神奇宝贝大多是双型。
4.  与普通的神奇宝贝相比，大多数传奇的神奇宝贝都有更高的统计数据。总和 500 似乎是分水岭。小小的普通神奇宝贝越过了那条线。
5.  根据不同移动的系数总和(axis sum_ag)来判断，传说中的神奇宝贝不一定在不同类型的移动中表现得更好或更差。

从以上几点可以看出传说中的神奇宝贝更强(当然！)但不一定在其他各方面都比普通的神奇宝贝好。比如传说中的神奇宝贝，能力就比较少。他们可能被设计来维持游戏平衡。

## 收场白

我非常喜欢使用这个数据集。我觉得我也可以用一些机器学习算法来研究这个数据集。我希望你喜欢阅读这篇文章，就像我喜欢练习和分享它一样。感谢您的宝贵时间！

*本文使用的 Jupyter 笔记本文件:*

1.  [https://github . com/tsang-George/notebooks/blob/2b 37 Bab 600199 FD 48112993748 BDD 08 c 3586 cc7a/pokemon/pokemon _ cleaning . ipynb](https://github.com/tsang-george/notebooks/blob/2b37bab600199fd48112993748bdd08c3586cc7a/pokemon/pokemon_cleaning.ipynb)
2.  [*https://github . com/tsang-George/notebooks/blob/2b 37 Bab 600199 FD 48112993748 BDD 08 c 3586 cc7a/pokemon/pokemon _ JPN . ipynb*](https://github.com/tsang-george/notebooks/blob/2b37bab600199fd48112993748bdd08c3586cc7a/pokemon/pokemon_jpn.ipynb)
3.  [*https://github . com/tsang-George/notebooks/blob/2b 37 Bab 600199 FD 48112993748 BDD 08 c 3586 cc7a/pokemon/pokemon _ MySQL . ipynb*](https://github.com/tsang-george/notebooks/blob/2b37bab600199fd48112993748bdd08c3586cc7a/pokemon/pokemon_mysql.ipynb)

*分割数据集:*

[*https://github . com/tsang-George/notebooks/tree/main/pokemon/dataset*](https://github.com/tsang-george/notebooks/tree/main/pokemon/dataset)

## 参考

[](https://www.kaggle.com/rounakbanik/pokemon) [## 完整的口袋妖怪数据集

### 所有 7 代 800 多只口袋妖怪的数据。

www.kaggle.com](https://www.kaggle.com/rounakbanik/pokemon)  [## MySQL :: MySQL 8.0 参考手册::13.2.7 加载数据语句

### 将文件'文件名'[替换|忽略]中的数据[低优先级|并发][本地]加载到 table TBL _ name[分区…

dev.mysql.com](https://dev.mysql.com/doc/refman/8.0/en/load-data.html) [](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-table.html) [## MySQL :: MySQL 工作台手册::6.5.1 表数据导出和导入向导

### 该向导支持使用 CSV 和 JSON 文件的导入和导出操作，并包括几个配置选项…

dev.mysql.com](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-table.html)  [## MySQL :: MySQL 5.7 参考手册::5.1.7 服务器系统变量

### 一些系统变量控制缓冲区或缓存的大小。对于给定的缓冲区，服务器可能需要分配…

dev.mysql.com](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_secure_file_priv)  [## MySQL :: MySQL 连接器/Python 开发者指南::10.5 游标。MySQLCursor 类

### MySQLCursor 类实例化可以执行 SQL 语句等操作的对象。光标对象交互…

dev.mysql.com](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor.html)  [## Mysql 连接器 Python::Anaconda.org

### 用于与 MySQL 服务器通信的 Python 驱动程序信息:这个包包含非标准标签的文件。康达…

anaconda.org](https://anaconda.org/conda-forge/mysql-connector-python)