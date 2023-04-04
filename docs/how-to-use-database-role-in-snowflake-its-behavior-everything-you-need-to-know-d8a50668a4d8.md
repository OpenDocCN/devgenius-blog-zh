# 如何在雪花中使用数据库角色，它的行为，你需要知道的一切…！

> 原文：<https://blog.devgenius.io/how-to-use-database-role-in-snowflake-its-behavior-everything-you-need-to-know-d8a50668a4d8?source=collection_archive---------13----------------------->

![](img/47f0a47bb32241f8cd144d5c5b03a287.png)

Natalya Letunova 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

今天我们将讨论关于**数据库角色的用例演示。**雪花如何使用数据库角色？目前，我们在 Snowsight UI 中没有看到数据库角色的实现。预览功能目前仅在代码级别工作。

如果你还没有看过我最近的关于 [**数据库角色 V/S 账户角色雪花**](https://medium.com/@rajivgupta780184/database-role-v-s-account-role-in-snowflake-49e960b3924a?source=your_stories_page-------------------------------------) 的博客，那么我会推荐你 t 请看一下，以便更好地理解理论部分。

# 用例 1:数据库角色使用和帐户角色行为

# 用例 2:数据库克隆的数据库角色行为

# 用例 3:雪花共享数据库角色

# 要记住的事情:

1.  有关**帐户使用共享数据库角色**的更多详细信息，请参见此处的[](https://docs.snowflake.com/en/sql-reference/snowflake-db-roles.html#account-usage-schema)****。****
2.  **关于**读者账户使用共享数据库角色**的更多细节，请参见 [**此处**](https://docs.snowflake.com/en/sql-reference/snowflake-db-roles.html#reader-account-usage-schema) **。****
3.  **有关**组织使用共享数据库角色**的更多详细信息，请参见此处的[](https://docs.snowflake.com/en/sql-reference/snowflake-db-roles.html#organization-usage-schema)****。******
4.  ****关于**标签对象共享数据库角色**的更多细节参见 [**此处**](https://docs.snowflake.com/en/sql-reference/snowflake-db-roles.html#tag-objects) **。******
5.  ****有关**核心使用共享数据库角色**的更多详细信息，请参见此处的[](https://docs.snowflake.com/en/sql-reference/snowflake-db-roles.html#core-schema)****。无论如何都要将核心模式访问权限授予公共角色。********
6.  ****有关创建数据库角色的更多详细信息，请参见此处的[](https://docs.snowflake.com/en/sql-reference/sql/create-database-role.html#create-database-role)****。********
7.  ****有关授权数据库角色的更多详细信息，请参见此处的[](https://docs.snowflake.com/en/sql-reference/sql/grant-database-role.html#grant-database-role)****。********
8.  ****有关对象级授权到数据库级授权的更多详细信息，请参见此处的 [**。**](https://docs.snowflake.com/en/sql-reference/sql/grant-privilege.html#database-roles)****
9.  ****由于这是预览版，我们可以预计到正式发布时行为会有一些变化。****

****希望这个演示博客能帮助你深入了解**雪花数据库角色用例**。如果你对此有任何疑问，欢迎在评论区提问。如果你喜欢这个博客，请鼓掌。保持联系，看到更多这样的酷东西。谢谢你的支持。****

******你可以找我:******

******订阅我的 YouTube 频道:【https://www.youtube.com/c/RajivGuptaEverydayLearning】T22******

******跟我上媒:**https://rajivgupta780184.medium.com/****

******在推特上关注我:**[https://twitter.com/RAJIVGUPTA780](https://twitter.com/RAJIVGUPTA780)****

******在 LinkedIn 上联系我:**[https://www.linkedin.com/in/rajiv-gupta-618b0228/](https://www.linkedin.com/in/rajiv-gupta-618b0228/)****

****![](img/f193b0c42922eb4b58fa84cee8e00b7c.png)****

******#继续学习#继续分享# RajivGuptaEverydayLearning # SnowflakeDataSuperhero # RajivGupta******