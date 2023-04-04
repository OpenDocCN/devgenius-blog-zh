# 无痛且强大——Nx/Next/Keystone/graph QL/prisma 2/Postgres

> 原文：<https://blog.devgenius.io/painless-powerful-nx-nest-keystone-graphql-prisma2-postgres-139805eee2b?source=collection_archive---------4----------------------->

![](img/b1e66d29b69fda401fd6562cdeac4b27.png)

阿瑟尼·托古列夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

好吧，对于一个博客标题来说，这是很多技术词汇——但是，嘿，它把你带到了这里，对吗？因此，让我告诉你如何让一个强大的单一定义模型系统与这个技术堆栈一起运行。

早些时候我在这里写了关于使用 Nx/Prisma & GraphQL [的文章](https://javascript.plainenglish.io/nx-nest-prisma-graphql-single-data-model-definition-e601eaa372c6) : [让我们创建下一个，Nx，GraphQL，Prisma 单一数据模型定义](https://javascript.plainenglish.io/nx-nest-prisma-graphql-single-data-model-definition-e601eaa372c6)——这篇文章是对那篇文章的更新/替换。

在这篇文章中，我将分享我如何通过将一个新的技术堆栈元素( [KeystoneJS v6](https://keystonejs.com/) )分层来改进我早期的方法，该元素处理大量繁重的工作，并为数据管理提供了一个很好的起点。我倾向于编写大量的公司内部应用程序来管理信息和知识，下面概述的方法使我能够更灵活、更快速地进行操作和开发。

# **第 1 部分:设置**

本节将涵盖:

*   生成 Nx 工作空间
*   在 docker 中调出 postgres DB 实例
*   在您的 Nx 工作空间中生成一个 NextJS 应用程序

第 2 部分介绍了 KeystoneJS 中的分层。

**Nx 工作空间**

生成一个 Nx 工作空间(或者，如果您已经有了一个工作空间，您可以跳过下一步，但我建议您使用或升级到 v13+)。有关设置 Nx mono repo 工作区的更详细信息，请参见位于 [Nx.dev](https://nx.dev) 的文档。

```
> **npx create-nx-workspace@latest**
Need to install the following packages:
  create-nx-workspace@latest
Ok to proceed? (y) y
✔ Workspace name (e.g., org name)     · **krispy.example**
✔ What to create in the new workspace · **next**
✔ Application name                    · **ksexample**
✔ Default stylesheet format           · **css**
✔ Use Nx Cloud? (It's free and doesn't require registration.) · **No**✔ Installing dependencies with npm
✔ Nx has successfully created the workspace.

 >  NX   Nx is creating your v13.5.1 workspace.To make sure the command works reliably in all environments, and that the preset is applied correctly,
   Nx will run "npm install" several times. Please wait.✔ Installing dependencies with npm
✔ Nx has successfully created the workspace.
```

如果您已经有了一个正在使用的 Nx 工作空间，那么您可以使用以下代码生成一个 Next.js 应用程序:

```
npx nx g @nrwl/next:app ksexample
```

如果你以前没有在你的工作空间中生成过 nextjs 应用程序(可能你的工作空间是为 pure node 或 express 等设置的。)那么您可能需要在您的工作区中 npm install @nrwl/next。

**在 Docker 中运行 Postgres 实例**

为了有效地使用 keystone，postgres db 应该在某个地方运行。在本地，docker 实例是最快的方法。稍后，在生产环境中，您可能会在另一个云提供商上使用 GCP 云 SQL (postgres)或同等工具，但这超出了本文的讨论范围。

使用卷装载运行 postgres 的本地 docker 实例，以在实例重新启动之间保存数据。

postgres 的连接字符串(示例)—您将在第 2 部分中使用它—显然您应该使用不同的用户名/密码。

```
postgres://app_admin:sup3rs3cr3t@localhost:11032/app_db
```

此时，您应该有一个 docker 实例正在运行，并且有一个生成了 Next.js 应用程序的 Nx 工作空间可供使用。这两件事之间还没有联系(这将在第 2 部分中介绍)。您可以通过执行以下命令来验证您的 next.js 应用程序是否正确生成:

```
npx nx serve ksexample
```

执行此操作应该会在您的终端中显示以下内容:

```
> **nx run ksexample:serve**We detected TypeScript in your project and reconfigured your tsconfig.json file for you. Strict-mode is set to false by default.The following suggested values were added to your tsconfig.json. These values can be changed to fit your project's needs:- incremental was set to trueinfo  - automatically enabled Fast Refresh for 1 custom loader
event - compiled client and server successfully in 2.3s (173 modules)
[ ready ] on [http://localhost:4200](http://localhost:4200)
```

在浏览器中访问 [http://localhost:4200](http://localhost:4200) 应该会显示默认生成的下一个应用。如果这对您有用，那么是时候继续第 2 部分了，我们将在第 2 部分中加入 KeystoneJS headless CMS。

# 第 2 部分:密钥(Keystone 应用生成和配置)

为什么选择 KeystoneJS，它到底能为我们做什么？

[KeystoneJS](https://keystonejs.com/) 是(在撰写本文时，v6 刚刚发布)一款成熟的无头 CMS，它结合了 [Prisma](https://www.prisma.io/) 、GraphQL 和 [Next](https://nextjs.org/) 来提供一个强大的系统，您可以在其上快速构建数据/信息应用程序。它通过为开发人员提供在单一位置以编程方式指定模型定义的能力来实现这一点，它生成 prisma 模式，管理数据库迁移(是的，如果您已经有了数据库模式，它还具有自省功能)，生成具有 ACL 支持的数据管理，并在开箱即用的 graphql 模式中生成数据模型的 ORM 排列。

在文章的这一部分，我们将在之前生成的 NextJs 应用程序中生成一个 Keystone 层。

**注意** : *KeystoneJS 应用程序可以独立于现有的 NextJS 应用程序生成——您可以使用 KeystoneJS 构建一个完整的应用程序，而无需 Nx 或本文中提到的其他东西*。

步骤 1:在终端 cd 中，从 Nx 工作区的根目录进入 apps/ksexample 目录。

```
cd apps/ksexample
```

第 2 步:在 apps/ksexample 目录下创建一个 package.json，内容如下

第三步:在`apps/ksexample`目录下创建一个`keystone.ts`文件，内容如下。注意:这是我们使用前面的 postgres 连接字符串的地方:`postgres://app_admin:sup3rs3cr3t@localhost:11032/app_db`参见 config 对象中的 url 属性。

一旦创建了这两个文件(`package.json`和`keystone.ts`)并且运行了 postgres docker 容器。在`apps/ksexample`目录中，在您的终端中执行一个`npm i`。您的终端输出应该如下所示:

```
npm i> ksexmple@1.0.0 postinstall
> keystone postinstallYour Prisma and GraphQL schemas are not up to date
✔ Would you like to update your Prisma and GraphQL schemas? … yes
✨ GraphQL and Prisma schemas are up to dateup to date, audited 708 packages in 13s
```

步骤 4:将 keystone 添加到 next.js 配置中

编辑`apps/ksexample` 目录中的`next.config.js`文件，如下所示:

创建一个`<workspace>/apps/ksexample/.gitignore`文件并添加:

```
.keystone
node_modules
```

最后，我们需要修改`apps/ksexample directory`中的`project.json`文件。请注意添加的两个目标“`keystone`”和“`devserve`”:

严格地说，你不需要这两个目标——“梯形”目标就足够了。我将“devserve”目标作为替代方法的一个例子。

你猜怎么着，你现在可以运行应用程序了，keystone + ksexample 将会启动。从工作区根目录在终端中执行以下操作。

```
npx nx keystone ksexample
```

您应该会看到类似于以下内容的输出:

```
npx nx keystone ksexample> nx run ksexample:keystone> ksexmple@1.0.0 postinstall
> keystone postinstall✨ GraphQL and Prisma schemas are up to date> ksexmple@1.0.0 dev
> npx next devready - started server on 0.0.0.0:3000, url: [http://localhost:3000](http://localhost:3000)
✨ Generated GraphQL and Prisma schemas
event - compiled client and server successfully in 520 ms (127 modules)
✨ Starting Keystone
⭐️ Dev Server Starting on [http://localhost:8000](http://localhost:8000)
⭐️ GraphQL API Starting on [http://localhost:8000/api/graphql](http://localhost:8000/api/graphql)
✨ Generating GraphQL and Prisma schemas
✨ The database is already in sync with the Prisma schema.
✨ Connecting to the database
✨ Creating server
✅ GraphQL API ready
✨ Generating Admin UI code
✨ Preparing Admin UI app
event - compiled client and server successfully in 3.7s (960 modules)
✅ Admin UI ready
wait  - compiling /api/__keystone_api_build...
event - compiled client and server successfully in 244 ms (973 modules)
```

此时，您可以访问 [http://localhost:3000](http://localhost:3000) 并查看下一个正在运行的应用程序，您也可以访问 [http://localhost:8000](http://localhost:8000) 正在运行。这些还没有完全与数据联系起来，但我们将从 Keystonejs.com 提供的例子中借用(此处[为](https://keystonejs.com/docs/walkthroughs/embedded-mode-with-sqlite-nextjs))。在本文中，我们不会深入探讨 keystone 的建模方面，但是这个例子应该清楚地表明它有多么强大，因为您可以使用在 nextjs 应用程序代码中定义的 keystone 模型中生成的所有模型。

关闭正在运行的实例(终端中的 ctl+c)。

将`apps/ksexample/pages/index.tsx`编辑成:
注意:下面的代码是直接从 Keystone 教程[这里](https://keystonejs.com/docs/walkthroughs/embedded-mode-with-sqlite-nextjs)复制过来的。

创建新文件:`apps/ksexample/pages/post/[slug].tsx`

重新启动开发服务器:

```
npx nx keystone ksexample
```

现在转到 [http://localhost:8000](http://localhost:8000) 创建一个帖子记录(当然也可以创建多个帖子)。

![](img/a01553846c87b0c4cf6474289742e01c.png)

一旦您创建了至少一个，请访问 [http://localhost:3000。](http://localhost:3000.)

![](img/436cf0c46d2bea3a230a851814c836cb.png)

点击你的文章标题会显示内容:

![](img/5c6f41c6d4abad12bfe1b19bd6919509.png)

# 第 3 部分:为什么这很有用

在这个示例应用的`keystone.ts`中，我们定义了一个简单的模型。这篇文章已经很长了，所以下次我会更深入地解释可以实现的更复杂的建模。现在，让我们看看 Keystone 和 GraphQL 的开箱即用——我们在 Nx/Next 应用程序中分层的东西。

如果你的应用服务器还在运行，尝试访问[http://localhost:8000/API/graph QL](http://localhost:8000/api/graphql)

您应该会看到一个 GraphQL 沙箱，您可以用它做一些显而易见的事情:

![](img/ca77584780aca9e43fcf6dfa205e6907.png)

好吧，很标准的东西。但是请注意，我们不需要显式设置 GraphQL 绑定。还要注意，从开发人员设置的角度来看，这要简单得多。我去年写的方法需要更多复杂的配置。

尝试运行这种查询:

```
{
  posts(where:{title:{startsWith:"Testing"}}) {
    id
    title
    slug
    content
  }
}
```

是的，我们得到了 where 子句！还有更多。想点成绩？

```
{
  posts(orderBy:{id:desc}) {
    id
    title
    slug
    content
  }
}
```

也许你只想要一个结果？

```
{
  posts(take:1) {
    id
    title
    slug
    content
  }
}
```

或者跳过一些结果？

```
{
  posts(skip:1) {
    id
    title
    slug
    content
  }
}
```

当然，您也可以将这些结合起来:

```
{
  posts(where:{title:{startsWith:"Testing"}}, orderBy:{id:desc} take,: 1, skip:1) {
    id
    title
    slug
    content
  }
}
```

这里的要点是，通过使用 Keystone 及其所有特性，您可以获得巨大的收益。使用 keystone DSL 进行数据建模会生成一个 prisma ORM 层，您不需要接触它。GraphQL 模式生成得到了扩展，可以用所有标准类型的过滤和查询/变异支持来映射您的模型——您不再需要手动检查这些样板文件，也不需要担心如何生成它。

将 Keystone 填充到 NextJS 应用程序中，可以在您的“后端”和前端之间利用相同的模型定义和数据访问，而无需进行端口映射等。现在，如果您的数据模型发生变化，您将知道它是否破坏了您的前端。

# 包扎

在本文中，我们生成了一个 Nx 工作区，生成了一个 NextJS 应用程序，在 docker 中引导了一个 Postgres 12 db，在 Keystone 中填充了一个数据建模和管理应用程序层，在后端和前端之间共享了一个模型，在 Nx 构建和服务管道中进行了连接。关于 Keystone 强大的建模功能，还有更多的内容需要介绍。我希望现在对你来说很明显——如果这在某种程度上对你有所帮助，我总是感谢一两次鼓掌。

当我有更多的时间时，我计划写一些更复杂的建模，这些建模已经在使用这种技术堆栈构建的几个应用程序中运行。让我在评论中知道什么样的模型会有帮助，我会尝试去涵盖这些。