# 利用配置 XML 和变量范围

> 原文：<https://blog.devgenius.io/utilize-configuration-xml-and-variables-scope-e74011371c0d?source=collection_archive---------2----------------------->

![](img/d15ed734db9d1c87e78cf7508f9efc46.png)

# 介绍

Magento 2 中的大部分配置是由 xml 文件完成的，并放在 *[Module_Root]/etc/* 目录中。根据您配置的**作用域**，将它们放在 *etc/adminhtml* 、 *etc/frontend* 或者 *etc/* 下的**全局作用域**下。当您将它们放入 adminhtml 或 frontend 时，这会覆盖全局范围。

# XML 配置文件列表。

*   ***acl.xml*** —资源标题，排序
*   ***adminhtml/rules/payment _ { country }。xml*** — paypal
*   ***address _ formats . XML***
*   ***address _ types . XML***—仅格式编码和标题
*   ***cache.xml*** —名称、实例—例如 full _ page =页面缓存
*   ***catalog _ attributes . XML***—catalog _ category，catalog_product，不可分配，used_in_autogeneration，quote_item *
*   **communication . XML**
*   ***config.xml*** —默认值
*   ***crontab . XML***—组[]，作业实例，方法，调度*
*   ***cron _ groups . XML****
*   ***di.xml*** —首选项、插件、虚拟类型*
*   ***eav _ attributes . XML***—锁定的实体属性(全局、唯一等。)
*   ***email _ templates . XML***—id 标签文件类型模块— view/frontend/email/name.html
*   ***events.xml*** —观察者，共享，禁用*
*   ***export.xml***
*   ***extension _ attributes . XML***—for，属性编码，属性类型
*   ***fieldset.xml***
*   ***import.xml***
*   ***indexer . XML***—类，视图 id，标题，描述
*   ***integration . XML***
*   ***集成/api.xml***
*   **integration/config . XML**
*   ***menu . XML***—管理菜单
*   ***module . XML***—版本，顺序
*   ***mview . XML***—计划更新、订阅表更改、索引器模型
*   **page _ types . XML**
*   ***payment . XML***—组，方法允许 _ 多个 _ 地址
*   ***pdf.xml*** —按类型(发票、发货、信用备忘录)和产品类型呈现
*   ***product _ types . XML***—标签、模型实例、索引优先级、(？)自定义属性，(！)可组合类型
*   ***product _ options . XML***
*   ***resources . XML***
*   ***routes.xml***
*   ***sales.xml*** —收款人(报价、订单、发票、贷项通知单)
*   ***search _ engine . XML***
*   ***search _ request . XML***—索引、维度、查询、过滤器、聚合、存储桶
*   ***sections . XML***—行动路线占位符- >使客户部分无效
*   ***system . XML***—adminhtml config
*   ***validation . XML***—实体、规则、约束—>类
*   ***view.xml*** —按模块变量
*   ***webapi.xml*** —路由、方法、服务类和方法、资源
*   ***widget.xml*** —类，邮件兼容，图片，ttl(？)、标签、描述、参数
*   ***ZIP _ codes . XML***—[为邮政编码添加自定义输入掩码](https://devdocs.magento.com/guides/v2.3/howdoi/checkout/checkout_zip.html)

# 配置加载顺序

配置文件的加载顺序从 *app/etc/di.xml* 开始。首先收集 *[Module_Root]etc/*下的配置文件。xml* ，然后在 *[Module_Root]/etc/[Area]/*下。xml* ，最后将它们合并在一起。

在合并配置文件时，检查 xml 节点的 id 属性是否唯一，否则它将被所有底层内容(子节点)覆盖。

当同一个 xml 文档(配置文件)包含多个具有相同 id 的节点时，它会给出一个错误。— [Magento DevDocs —模块配置文件](https://devdocs.magento.com/guides/v2.2/config-guide/config/config-files.html)

**配置合并类**

*   \Magento\Framework\Config\Dom

**合并每个文件时**

*   创建配置合并|合并
*   \ Magento \ Framework \ Config \ Dom::_ initDom(Dom，perFileSchema)
*   \ Magento \ Framework \ Config \ Dom::validateDomDocument
*   $dom->schemaValidate

**一切合并后**

*   \ Magento \ Framework \ Config \ Dom::validate(merged schema)
*   \ Magento \ Framework \ Config \ Dom::validateDomDocument

# 环境设置

Magento 2 附带了一个配置文件 *app/etc/env.php* ，您可以在其中存储特定于系统的敏感文件，这些文件可以是 API 密钥、密码和个人信息，例如电子邮件或电话号码。导出过程中不包括敏感数据。

该文件还包含您的数据库数据、用于保护您的密码和其他敏感信息的 crypt 密钥(在 Magento 安装期间创建)、用于管理 url 和缓存类型的后端 frontName。

要创建一个配置，你可以用 *bin/magento* 做如下操作。

> *要查看更多选项运行* bin/magento config *，选项对于* ***Magento 2.2.4 或更高版本*** *可能会有所不同。*

设置非敏感配置

```
bin/magento config:set [--scope] [--scope-code] path value
```

设置敏感配置(写入 app/etc/env.php)

```
bin/magento config:sensitive:set [--scope] [--scope-code] path value
```

显示保存的配置

```
bin/magento config:show
```

导出配置，注意:不包括敏感数据！

```
bin/magento app:config:dump
```

示例:

```
bin/magento config:set --scope=websites --scope-code=base web/unsecure/base_url http://example.com/
```

要找到正确的范围，您可以在数据库中运行以下 sql 查询:

```
SELECT * FROM store;
SELECT * FROM store_website;
```

使用 magerun:

```
n98-magerun.phar db:query "SELECT * FROM store;"
n98-magerun.phar db:query "SELECT * FROM store_website;"
```

有关更多信息，您可以参考以下 Magento 文档

*   [设置配置值](https://devdocs.magento.com/guides/v2.2/config-guide/cli/config-cli-subcommands-config-mgmt-set.html)
*   [敏感且特定于系统](https://devdocs.magento.com/guides/v2.2/config-guide/prod/config-reference-sens.html)
*   [其他配置路径参考](https://devdocs.magento.com/guides/v2.2/config-guide/prod/config-reference-most.html)

# 觉得这个帖子有用吗？请点击👏下面的按钮！:)