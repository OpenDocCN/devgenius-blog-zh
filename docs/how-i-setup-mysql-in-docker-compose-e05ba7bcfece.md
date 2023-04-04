# 如何在 Docker Compose 中设置 MySQL？

> 原文：<https://blog.devgenius.io/how-i-setup-mysql-in-docker-compose-e05ba7bcfece?source=collection_archive---------0----------------------->

## 软件工程之旅

## 软件工程师数据库系列最佳实践

![](img/e1793ca4cbb5a4c52cf07fb4f26e963d.png)

照片由 Unsplash 上的 Sunder Muthukumaran 拍摄

# 概观

如今，数据库是应用程序不可缺少的最重要的东西之一。在这一系列的文章中，我将向你介绍如何建立一个 RDBMS(关系数据库管理系统)，关于 SQL(结构化查询语言)的知识和一些实用的例子来帮助你弄清楚并能把这些知识应用到你的实际项目中。

在这第一篇文章中，我将向您介绍用 Docker 轻松设置 MySQL(一种 RDBMS)的方法。一旦你完成了这篇文章中的设置，你就可以在任何时候用你的机器与你的服务器进行交互并练习 SQL。

使用 Docker Compose，我们可以很容易地创建一个 MySQL 实例，配置端口，您的“root”帐户密码，也许您可以在您的机器上创建多个 MySQL 实例。

# 步骤 1:设置 Docker

Docker 是一个开发、发布和运行应用程序的开放平台。Docker 使您能够将应用程序从基础设施中分离出来，这样您就可以快速交付软件。使用 Docker，您可以像管理应用程序一样管理基础设施。

Compose 是一个定义和运行多容器 Docker 应用程序的工具。使用 Compose，您可以使用 YAML 文件来配置应用程序的服务。然后，只需一个命令，您就可以从您的配置中创建并启动所有服务。

正如标题中主要提到的，为了启动我们的 SQL server，您应该首先确保您的机器已经安装了 Docker。您可以通过键入以下命令来检查您的计算机上是否有 Docker:

```
**docker version**
```

如果您的机器上没有安装 Docker，您可以通过链接安装一个[安装 Docker](https://docs.docker.com/get-docker/) 。请使用上面的命令进行验证，以确保 Docker 存在于您的机器上。

# 第二步:启动 MySQL

MySQL 是 Oracle 开发的基于结构化查询语言(SQL)的关系数据库管理系统(RDBMS)。

在这一步中，我将为您提供一个 Docker compose 文件，其中解释了构建 SQL server 的属性。

**Docker 撰写文件**

使用上面的 docker-compose.yml，您可以运行下面的命令，然后您将成功地在您的机器上创建一个 MYSQL:

```
**docker compose up build**
```

**解释 Docker 撰写属性**

- **图像**:该图像是您的 My SQL 版本的图像，在我的示例中，我使用 MySQL 8.0 版本，因此我的图像名称为 mysql:8.0

- **container_name** :这是由 Docker 管理的容器的名称

- **端口**:端口信息是你的 MySQL 将要连接的端口，左边的数字是你机器中的端口，右边的数字是你容器中的端口。

- **MYSQL_ROOT_PASSWORD** :这是您在 SQL server 上拥有完全权限的 ROOT 帐户的密码

- **MYSQL_USER** :这是一个额外的用户，如果你想在你的服务器上有更多的用户，而不仅仅是根用户

- **MYSQL_PASSWORD** :这是你的用户密码

- **MYSQL_ROOT_HOST** :这个选项让我们可以用 ROOT 帐号连接数据库

# 步骤 3:测试 MySQL 和基本 DDL

**连接数据库**

完成 MYSQL 设置后，您可以使用一些 MySQL 应用程序连接到您的服务器。对我来说，我使用 MYSQL Workbench 连接，如下图所示:

![](img/e9ed1c51e455b408788eef63cfcd1579.png)

*   **连接名称:N** 您的连接名称，您可以随意命名。
*   **主机名:**我们可以使用本地主机或 127.0.0.1
*   这是我们在 Docker Compose 文件上配置的端口。您可以在 Docker Compose 中更改端口，也可以在这里进行修改。
*   **用户名:T** 他连接 MYSQL 的用户名，我们应该你“根”在这里
*   密码:T 我们在 Docker Compose 文件中使用属性 **MYSQL_ROOT_PASSWORD** 设置的密码

**用 DDL 脚本创建数据库和第一个表**

上面的所有设置都有助于我们创建一个 MYSQL，而服务器中没有任何已创建的内容。因此，在这一步，我将为您提供一些简单的数据库创建脚本和数据库中的第一个表。

# 结论

当你看完这篇文章的所有内容后，我想现在你已经有了一台可以在你的机器上使用 SQL 的服务器。让我们在我以后的文章中探索更多关于 SQL 和最佳实践的内容。希望你喜欢这篇文章。感谢您的阅读！

如果你喜欢这篇文章，你也可以喜欢这些文章:

*   如何在我的本地开发环境中用 Docker 设置 MongoDB 来加快开发速度？
*   如何在 Docker 中设置 Redis 来提高本地环境的开发效率？