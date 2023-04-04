# 使用这些简单的 bash 脚本导出和导入您的 MongoDB 集合

> 原文：<https://blog.devgenius.io/export-and-import-your-mongodb-collections-using-these-simple-bash-scripts-23ccf658206b?source=collection_archive---------2----------------------->

![](img/8046c10960a80e2203fedbcc5f375356.png)

朱庇特艺术区，苏格兰爱丁堡

几天前，我不得不将一个远程 Mongo 数据库复制到我们的一台本地机器上。本质上，我们的最终目标是用一个大数据集测试我们的开发代码的性能。在本文中，我将解释一种借助 bash 脚本导出和导入多个数据库集合的方法。

以 MySQL 数据库为例，只需在 phpMyAdmin 中单击一个按钮，就可以完成这个导出/导入任务。然而对于 MongoDB，由于我们的团队只使用 Mongo Shell，我唯一的选择就是使用 CLI 来完成任务。

[MongoDB](https://docs.mongodb.com/) 的官方文档建议使用 [mongoexport](https://docs.mongodb.com/database-tools/mongoexport/) 工具来导出特定的集合，使用 [mongoimport](https://docs.mongodb.com/database-tools/mongoimport/) 工具来导入。

为了导出集合:“events”例如，从数据库将“reporting”导出到文件 events.json，可以简单地使用以下命令:

> mongoexport-collection = events-db = reporting-out = events . JSON

在我们的例子中，由于数据库需要认证，上面的命令必须用我们的数据库凭证来扩展。

```
mongoexport --db=the_db_name --collection=the_collection_name \
--out=the_collection_name.json --host 127.0.0.1:27017 \
-u the_username -p the_password --authenticationDatabase=admin 
```

然后， [mongoimport](https://docs.mongodb.com/database-tools/mongoimport/) 的用法也非常简单:

```
mongoimport --db=the_db_name --collection=the_collection_name \ 
--drop --file=the_collection_name.json --port=27017 \ 
-u the_username -p the_password --authenticationDatabase=admin
```

使用这些命令，我们只能从一个数据库向另一个数据库导出/导入单个集合。然后，为了复制 *n* 个集合，上述命令应重复复制和编辑 *n* 次。这是“好的”，但不是很好，也没有效率。因此，下一步是找到更好的自动化任务的方法。

我首先想到的是写一个 shell 脚本来完成所有多余的任务。例如，在导出过程中，脚本必须遍历可用的集合，并为每个集合调用 [mongoexport](https://docs.mongodb.com/database-tools/mongoexport/) 命令。另一方面，对于导入，脚本必须遍历包含导出数据的`.json`文件，然后将后者导入第二个数据库的新集合。

当我们在 UNIX 环境下工作时，BASH 脚本是我能想到的执行自动化的最明显的选择。

# 出口商

bash 脚本应该以`#!/bin/bash` 开始，其中`#!`是一个操作符，它将脚本指向解释器位置。在我们的例子中，它指向位于`/bin/bash`的 bourne-shell。

因此，我们代码的第一行应该是这样的:

```
#!/bin/bash
```

使用 mongo Shell，可以通过调用:`db.getCollectionNames()`获得所选数据库中的集合列表。为了使这个命令的结果成为 bash 数组格式，我们在`db.getCollectionNames()`后添加了`.join(' ')`。

要从 mongo Shell 外部执行这个命令，我们需要使用`--eval`,并以如下方式将其作为字符串传递:

```
mongo the_database_name --quiet --eval "db.getCollectionNames().join(' ')"
```

然后，有必要将集合名称保存到一个变量中，以便我们以后可以遍历它们。

```
#!/bin/bash
DB_COLLECTIONS=$(mongo the_database_name --quiet --eval "db.getCollectionNames().join(' ')")
```

一旦我们有了每个集合的名称，我们就可以执行前面的 [mongoexport](https://docs.mongodb.com/database-tools/mongoexport/) 命令将集合导出到不同的文件中。我们的`export.sh`的内容现在应该是这样的:

```
#!/bin/bashDB_COLLECTIONS=$(mongo the_database_name --quiet --eval "db.getCollectionNames().join(' ')")for collection in $DB_COLLECTIONS; do mongoexport --db=the_database_name --collection=$collection --out=$collection.jsondone
```

为了让脚本更具重用性，我们可以将`the_database_name`作为脚本的一个参数传递。`exporter.sh`的最终内容应该如下所示:

对于一个安全的数据库，我们应该在代码中添加凭证，以确保脚本的顺利执行。

要执行这个脚本，我们只需运行`./exporter.sh the_database_name`。

# 进口商. sh

为了将导出的集合导入到另一个数据库中，我们将`.json`文件(从前面的`exporter.sh`中获得)移动到一个单独的文件夹中。然后，我们还在同一个文件夹中创建了`importer.sh`文件，以使事情更简单。

importer 文件本质上包含一段代码，这段代码循环遍历`.json`文件，并在每次迭代中执行 mongoimport 命令。

```
#!/bin/bashfor FILE in *.json; do c= basename $FILE .json; mongoimport --db=$DB --collection=$c --drop --file=$FILE --host 127.0.0.1:27017 -u the_username -p the_password --authenticationDatabase=admindone
```

我们还可以将数据库名称作为参数传递，以提高脚本的可重用性。

为了使用这个脚本，我们只需运行`./importer.sh the_database_name`。

如果脚本的执行导致`Permission denied`错误，我们需要通过运行以下命令为脚本添加“执行”权限:

```
chmod +x importer.sh exporter.sh
```

最后，例如，为了将集合从`database_0`导出到`database_1`，我们可以运行以下命令:

```
./exporter.sh database_0 && ./importer.sh database_1
```

就是这样！现在我们可以坐下来看脚本为我们执行导出-导入。

包含这些代码的一个小仓库可以在:[https://github.com/HoracioSoldman/export-import-mongodb](https://github.com/HoracioSoldman/export-import-mongodb)找到