# prisma 简介

> 原文：<https://blog.devgenius.io/introduction-to-prisma-4d05989f8130?source=collection_archive---------2----------------------->

![](img/77ed2e65fc849ccf2f034caca9289dbc.png)

编写 SQL 查询是一件麻烦的事情。变得更难维护，也容易搞砸。ORMs 被引入来解决这个问题。

# 什么是 ORM？

ORM 代表*对象关系映射器*。对象关系映射器是一种技术，允许您使用面向对象的范例查询和操作数据库中的数据。[姜戈](https://www.djangoproject.com/)是最受欢迎的 ORM 之一。不管使用什么数据库，操作数据库的代码都是一样的。这使得在需要时切换到不同的数据库变得更加容易。

# Prisma 是什么？

Prisma 是 NodeJS 的 ORM。Prisma 支持 MySQL、SQLite、PostgreSQL、SQL Server 和 MongoDB。但是对 MongoDB 的支持还不稳定。Prisma 有 3 种工具:

[**Prisma Client JS**](https://github.com/prisma/prisma-client-js) 是一个数据库客户端，它允许我们通过普通的 JavaScript 方法和对象访问数据库，而不必用数据库语言本身编写查询。

[**Prisma Migrate**](https://github.com/prisma/migrate)是一款数据库模式迁移工具。它帮助我们跟踪迁移历史，并让我们轻松地恢复和重放迁移。

[**Prisma Studio**](https://github.com/prisma/studio) 是一个管理界面，允许我们查看、添加和更改数据库中的数据。

## 先决条件

*   在本教程中，我们将在 NodeJS 应用程序中使用 Prisma，因此建议您对 NodeJS 和 npm 有一些基本的了解。
*   我们将在本教程中使用 [PostgreSQL](https://www.postgresql.org/) ，所以了解一点 PostgreSQL 知识会有所帮助。

# 设置 Prisma:

让我们首先设置一个空的 nodejs 应用程序。在项目目录中打开终端并运行以下命令。

```
npm init -y
npm install prisma --save-dev
```

上述命令将创建一个空节点 js 项目，并将 prisma 作为 dev 依赖项添加到项目中。

下一步是通过使用以下命令创建一个[模式](https://www.prisma.io/docs/concepts/components/prisma-schema)文件来设置 Prisma

```
npx prisma init
```

您应该会看到如下所示的响应

```
✔ Your Prisma schema was created at prisma/schema.prisma
You can now open it in your favorite editor.

Next steps:
1\. Set the DATABASE_URL in the .env file to point to your existing database. If your database has no tables yet, read https://pris.ly/d/getting-started
2\. Set the provider of the datasource block in schema.prisma to match your database: postgresql, mysql, sqlite, sqlserver, mongodb or cockroachdb (Preview).
3\. Run prisma db pull to turn your database schema into a Prisma schema.
4\. Run prisma generate to generate the Prisma Client. You can then start querying your database.

More information in our documentation:
https://pris.ly/d/getting-started
```

这会在 **prisma** 目录下创建一个名为 **schema.prisma** 的模式文件，并为您的环境变量创建一个. env 文件。

# Prisma 架构文件:

让我们检查模式文件中的内容，模式文件应该有如下代码

```
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

`generator`部分是我们指定想要生成 Prisma 的客户端的地方。客户端通过运行 **prisma generate** 命令生成。每当对数据库模式进行更改时，我们都需要运行这个命令。

`datasource`部分是我们提供将要使用的数据库提供者的地方。默认情况下已经选择了 **postgresql** 。我们可以使用环境变量 **DATABASE_URL** 给出数据库连接 URL。现在在 **url** 下添加一个新行，并添加。

```
shadowDatabaseUrl = env(“SHADOW_DATABASE_URL”)
```

每次运行开发命令时，都会自动创建和删除[影子数据库](https://www.prisma.io/docs/concepts/components/prisma-migrate/shadow-database)，主要用于检测模式漂移等问题。如果您提供给 prisma 的数据库用户拥有创建新数据库的权限，则不需要这样做。当设置 prisma 时，您不需要它，但是当您开始对数据库进行更改时，您将需要它

打开`.env`文件，并更新连接 URL 以指向您自己的数据库。

连接 URL 的格式取决于您使用的数据库。的。env 文件应该有 2 个变量，带有更新的连接 URL

```
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE?schema=SCHEMA"
SHADOW_DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE?schema=SCHEMA"
```

接下来打开 **schema.prisma** 文件，让我们在**模型**块中添加`User`和`Post`模型。在所有的改变之后，schema.prisma 文件应该如下所示。

```
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
  shadowDatabaseUrl = env("SHADOW_DATABASE_URL")
}

model Post {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  title     String   @db.VarChar(255)
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}

model User {
  id      Int      @id @default(autoincrement())
  email   String   @unique
  name    String?
  posts   Post[]
}
```

模态是您在应用程序中使用的实体。**用户**模态具有以下属性

`id`是 **Int** 数据类型的主键，自动递增。我们可以使用 **@id** 属性使一个字段成为主键

`email`和`name`属于**型串**

`posts`属于类型 **Post** ，带有类型修饰符 **[]** ，这意味着我们可以在一个用户下保存一个帖子列表

类似地，我们的 Post 模式具有以下属性

`id`是 **Int** 类型的主键。

`createdAt`的类型为**日期时间**。

`title`和`content`属于**字符串**类型

`published`属于类型**布尔**

`author`属于类型**用户**。这是必需的，因为我们需要告诉 Prisma that 模态与用户模态有一个外键(在这种情况下是多对多关系)。我们可以指定文章模式中的哪个字段可以用来存储文章的用户 id，在本教程中我们已经提到了使用**@ relation(fields:[author id]**

`authorId`为 **Int** 类型，用于与用户模型建立多对多关系。

# 使用 Prisma 迁移

运行以下命令在数据库中生成表。

```
npx prisma migrate dev — name init
```

该命令还会在`prisma/migrations` 目录下生成一个迁移文件。这是一个简单的 SQL 文件，让用户可以方便地看到 Prisma 在幕后做了什么。

# 安装 Prisma 客户端

使用以下命令安装 Prisma 数据库客户端

```
npm install @prisma/client
```

您可以使用以下命令来生成 Prisma 客户端

```
npx prisma generate
```

将来无论何时对 prisma 模式进行更改，您都需要手动调用 **prisma generate** ，以适应 Prisma 客户端 API 中的更改。

# 查询数据库

创建一个名为 **index.js** 的新文件，并在其中添加以下代码

```
const { PrismaClient } = require('@prisma/client')

const prisma = new PrismaClient()

async function main() {
    *// insert a record into user table and post table*
    await prisma.user.create({
        data: {
            name: 'John',
            email: 'john@example.com',
            posts: {
                create: { title: 'Hello World' },
            }
        },
    })
    *// fetch all the records*
    const allUsers = await prisma.user.findMany({
        include: {
            posts: true,
        },
    })
    console.dir(allUsers, { depth: null })
}

main()
    .catch((e) => {
        throw e
    })
    .finally(async () => {
        await prisma.$disconnect()
    })
```

在上面的代码中，我们首先导入了 prisma 客户端，并使用`const prisma = new PrismaClient()`创建了一个新实例。

然后，我们声明了`**main()**`函数，并在用户模式中插入了一条新记录，我们还可以在同一个命令中插入与用户相关的帖子。Prisma 将在用户表和 post 表中添加一条记录。

然后，我们获取了用户表中的所有记录以及与用户相关的帖子

我们已经调用了 main 函数来运行查询。现在使用以下命令运行 index.js 文件

```
node ./index.js
```

用户记录连同帖子将在终端中打印出来

```
[
    {
        id: 1,
        email: 'john@example.com',
        name: 'John',
        posts: [
            {
                id: 1,
                createdAt: 2022-05-26T00:53:45.909Z,
                title: 'Hello World',
                content: null,
                published: false,
                authorId: 1
            }
        ]
    }
]
```

类似地，如果您想要获取所有的帖子并获取与它们相关联的用户。您可以使用下面的代码

```
const allPosts = await prisma.post.findMany({
    include: {
        author: true,
    },
})
```

这将打印以下内容

```
[
    {
        id: 1,
        createdAt: 2022-05-26T00:53:45.909Z,
        title: 'Hello World',
        content: null,
        published: false,
        authorId: 1,
        author: { id: 1, email: 'john@example.com', name: 'John' }
    }
]
```

恭喜🎉您已经成功安装了 Prisma。

# 探索 Prisma Studio 管理

Prisma 附带了一个管理 UI，用于对我们的数据执行 CRUD 操作。运行以下命令打开 Prisma studio。

```
npx prisma studio
```

如果您在浏览器中打开 [http://localhost:5555/](http://localhost:5555/) ，您可以访问管理界面。

![](img/7794b27a1b53fe8017429d440afe7802.png)

您可以单击并打开表格

你可以在这个 [github repo](https://github.com/dinesh24murali/prisma_example/tree/javascript-setup) 中参考上面的全部源代码。

如果你想了解什么是 Next.js 以及如何使用它，请查看我们的[博客](https://codeknight.in/react/next_js_crash_course)。