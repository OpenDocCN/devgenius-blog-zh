# Magento 2 中的声明式模式概述—第 2 部分

> 原文：<https://blog.devgenius.io/how-to-use-declarative-schemas-in-magento-2-part-2-e4c09f5b6b6?source=collection_archive---------10----------------------->

![](img/51f78be737e930cfa63ee66962e4b881.png)

保罗·弗伦泽尔在 [Unsplash](https://unsplash.com/) 上的照片

在第一部分中，我们讨论了什么是声明式模式，为什么应该使用声明式模式而不是安装/升级脚本，如何创建、编辑并最终删除声明式模式。如果你还没有读过第一部分，我建议你花一点时间[来读这第一个](https://medium.com/@rick.daalhuizen90/why-use-a-declarative-schema-over-install-scripts-in-magento-2-85092f9f85b9)。

作为一个例子，我创建了一个简单的模块，包含一个看起来像这样的[InstallSchema.php](https://www.mageplaza.com/magento-2-module-development/magento-2-how-to-create-sql-setup-script.html)。

**将安装/升级脚本迁移到声明性方案**

现在也可以使用“模式监听器工具”将现有的安装/升级脚本转换为声明性方案。了解只能在开发人员模式下使用模式监听器工具是很有用的，因为它会对代码进行更改。此外，它没有考虑原始 SQL 数据，以及在*\ Magento \ Framework \ DB \ Adapter \ Pdo \ Mysql*之外的自定义 DDL 操作。

要执行迁移，请运行以下命令:

```
bin/magento s:up --convert-old-scripts=1
```

> 对于低于 2.4 的 Magento 版本，请使用— convert_old_scripts 标志

请注意，这仅在模块安装或升级期间有效。如果在生成 db_schema.xml 时遇到问题，请尝试删除 setup_module 表中的模块条目，然后再次运行 setup:upgrade。

这将在您的模块 etc 目录中生成一个 db_schema.xml。

然后我们添加一些数据

```
INSERT INTO `magento`.`quote_item_file` (`entity_id`, `filename`, `location`, `quote_item_item_id`) VALUES ('1', 'examole.png', 'media/foo/bar/example.png', '1');
```

**什么是模式白名单，如何创建它们**

方案白名单是为了向后兼容。这跟踪对声明性方案进行了哪些调整以防止数据丢失，而不知道这是何时发生的。当使用安装/升级脚本时，这通常可以在代码中找到。

白名单在<module_vendor>/<module_name>/etc/db _ schema _ whitelist . JSON 中生成，可以按如下方式创建:</module_name></module_vendor>

```
bin/magento setup:db-declaration:generate-whitelist --module-name=<Module_Name>
```

看起来像这样

```
{
   "quote_item_file": {
       "column": {
           "entity_id": true,
           "filename": true,
           "location": true,
           "quote_item_item_id": true
       },
       "constraint": {
           "PRIMARY": true,
           "QUOTE_ITEM_FILE_QUOTE_ITEM_ITEM_ID_QUOTE_ITEM_ITEM_ID": true
       }
   }
}
```

> 作为最佳实践，您应该为每个版本生成一个新的白名单文件。您必须为 db_schema.xml 文件中包含变更的任何版本生成白名单。

**利用试运行**

现在我们已经创建了 db_schema.xml，如果我们想以任何方式修改表，比如添加一列，我们可以使用预演。预演确保您不必担心脚本会破坏某些东西。在模拟运行期间，不会对数据库进行任何更改。

让我们删除其中一列

运行以下命令以使用模拟运行:

```
bin/magento s:up --dry-run=1
```

在您执行该操作后，将在*<Magento _ Root>/var/log/dry-run-installation . log*中生成一个日志文件。该文件包含原始 SQL 语句，可用于在必要时优化或修改您的脚本。

```
www-data@example-php-fpm:06:57 PM:/var/www/html$ cat var/log/dry-run-installation.logALTER TABLE `quote_item_file` DROP COLUMN `location`
```

**如何执行回滚。**

如果在迁移过程中由于某种原因出现了问题，可以使用回滚。对于那些还不熟悉回滚的人来说，它确保了最近所做的更改可以被撤销。

每次对表或列进行破坏性操作时，使用安全模式选项都会创建一个 CSV 文件。

```
bin/magento s:up --safe-mode=1
```

这将在以下位置之一创建一个 csv 文件:

*   <magento_root>/var/declarative _ dumps _ CSV/{ column _ name _ column _ type _ other _ dimensions }。战斗支援车</magento_root>
*   <magento_root>/var/declarative _ dumps _ CSV/{ table _ name }。战斗支援车</magento_root>

csv 文件包含来自数据库表、行、列和值的信息，这些信息已针对安装进行了修改。

在我们的例子中，它看起来像这样

```
www-data@example-php-fpm:07:40 PM:/var/www/html$ cat var/declarative_dumps_csv/quote_item_file_column_location.csv
entity_id,location
1,media/foo/bar/example.png
```

验证一切正常后，我们可以从表中删除该列并运行:

```
bin/magento s:up
```

要将一切恢复原样，我们只需将删除的列放回 db_schema.xml 中。

最后，我们运行以下命令来执行回滚。

```
bin/magento s:up --data-restore=1
```

> — data-restore=1 仅适用于 setup: upgrade 命令

**结论**

我希望我已经向您介绍了 Magento 中的声明性模式以及如何使用它们。如果您想了解更多，我还建议您阅读 Magento 文档中关于“[将安装/升级脚本迁移到声明性模式](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/migration-commands.html)”的内容。如需反馈或意见，请在下方留言。

# 觉得这个帖子有用吗？请点击👏下面的按钮！:)