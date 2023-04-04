# 如何在 NodeJS 中处理多种环境

> 原文：<https://blog.devgenius.io/how-to-handle-multiple-environments-in-nodejs-7391d2db2abe?source=collection_archive---------0----------------------->

## 如何建立一个专业的节点工程

![](img/dc55d69842f08d118c70a801f41dec40.png)

由[法尔扎德·纳兹菲](https://unsplash.com/@euwars?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

对于生产级应用程序，我们需要支持多种环境。例如，您需要为不同的环境使用不同的数据库。用途很多，这只是其中之一。

今天，我们将了解如何在 NodeJS 应用程序中管理多个环境。

## 在我们开始之前…

我用 ExpressJS 和 Typescript 创建了一个[专业样板。本文是这个系列的一部分。你可以在下面找到所有的文章。](https://github.com/Mohammad-Faisal/professional-express-sequelize-docker-boilerplate)

```
***** [**Creating a ExpressJS + Typescript Boilerplate**](https://javascript.plainenglish.io/create-an-express-boilerplate-with-typescript-810eb6c29196) ***** [**How to setup Linter and Formatter for NodeJS**](https://javascript.plainenglish.io/how-to-set-up-linter-formatter-for-node-js-d6b34c0c8be5) ***** [**How to handle multiple environments in NodeJS**](/how-to-handle-multiple-environments-in-nodejs-7391d2db2abe) ***** [**Error Handling in NodeJS**](https://javascript.plainenglish.io/error-handling-in-node-js-like-a-pro-ed210baa0600) ***** [**Request validation in NodeJS**](https://medium.com/p/c69f2494cf18) ***** [**Using Docker Professionally with NodeJS**](https://levelup.gitconnected.com/use-docker-with-nodejs-projects-like-a-pro-a9e7504e1308) ***** [**Using Docker for Local Development in NodeJS**](https://javascript.plainenglish.io/node-js-database-with-docker-for-local-development-285212c5162f) ***** [**Logging in NodeJS**](https://javascript.plainenglish.io/node-js-logging-for-professionals-6be07c003e7f) ***** [**Kubernetes with NodeJS**](https://levelup.gitconnected.com/kubernetes-deployment-with-nodejs-made-easy-eaeec32b62e3)
```

我们开始吧！

# 传统方式

使用环境变量最直接的方法是使用我们的 package.json 文件。让我们创建两个命令来演示两种环境。

```
"scripts": {
    "start-dev": "PORT=3000 ts-node-dev --respawn src/index.ts",
    "start-prod": "PORT=4000  ts-node-dev --respawn src/index.ts"
  }
```

我们定义了两个命令。对于开发，我们已经设置了`start-dev`，其中端口变量的值应该是 3000。而对于`prod`将是 4000。

如果我们运行应用程序

```
yarn start-dev
```

您将收到以下消息。

```
Connected successfully on port 3000
```

运行 start-prod 将为您提供

```
Connected successfully on port 4000
```

因此，我们已经成功地使我们的应用程序可配置。

# 让我们更进一步。

如果我们需要另一个在不同环境下会变化的变量呢？我们可以在字符串中添加另一个变量。

package.json

通过`process.env.SOMETHING_ELSE`可以到达这里的`SOMETHING_ELSE`

但是在现实生活中，我们有很多环境需要改变。我们如何做到这一点？

# 让我们创建一个环境文件。

为了管理许多环境变量，我们可以使用环境文件。转到您的项目，创建一个. env 文件，并粘贴以下内容。

```
PORT=3000
SOMETHING_ELSE=dev
```

并从 package.json 命令中删除环境。

```
"scripts": {
    "start-dev": "ts-node-dev --respawn src/index.ts",
    "start-prod": "ts-node-dev --respawn src/index.ts",
 },
```

我们跑吧！

```
yarn start-dev
```

这将给出以下输出。

```
Connected successfully on port 5000
```

这太奇怪了。因为我们应该将 3000 视为端口，但它给了我们 5000。

原因是尽管我们添加了环境变量，NodeJS 不会自动读取它们。

# 让我们读取我们的环境变量。

我们有一个非常受欢迎的软件包来为我们做这件事。名字是 [dotenv](https://www.npmjs.com/package/dotenv)

让我们先安装它。

```
yarn add dotenv
```

并将其导入我们的应用程序，如下所示。

```
import "dotenv/config";
```

或者像这样

```
import dotenv from "dotenv";
const configuration: any = dotenv.config().parsed;
```

这就够了。您不需要做任何其他事情来读取环境文件。这个包将通过 process.env 为我们的应用程序提供变量

为了验证这一点，您可以运行。

```
yarn start-dev
```

您将再次看到应用程序在端口 3000 上运行。

# 多重环境

现在让我们为应用程序生产做好准备。让我们为不同的环境创建两个不同的文件。

根据环境创建文件，如`.env.development`和`.env.production`

现在我们需要在启动过程中加载文件。Windows 环境有时会面临加载环境的问题。为了解决这个问题，让我们安装一个名为`[**cross-env**](https://www.npmjs.com/package/cross-env)`的包

```
yarn add -D cross-env
```

然后为 package.json 中的那些创建单独的脚本

```
"dev": "cross-env NODE_ENV=development ts-node-dev --respawn src/index.ts",
"prod": "cross-env NODE_ENV=production ts-node-dev --respawn src/index.ts",
```

然后修改配置文件，如下所示:

配置文件

现在，我们可以在应用程序的任何地方使用该配置。

```
import Config from "./utils/Configuration";console.log(Config.port);
```

就是这样！现在，您可以拥有任意多的环境，并轻松处理它们。

# 最后的话

不要忘记添加。env 文件到您的。gitignore 文件。否则，你所有的秘密都会暴露。

打开您的`.gitignore`文件并添加以下内容

```
.env.development
.env.production
```

就是这样。祝你今天过得愉快，:D

```
**Want to Connect?**You can reach out to me via [**LinkedIN**](https://www.linkedin.com/in/56faisal/) or my [**Personal Website**](https://www.mohammadfaisal.dev/blog)
```

# GitHub 回购

[](https://github.com/Mohammad-Faisal/nodejs-environment-handling) [## GitHub-Mohammad-fais al/nodejs-environment-handling:在 NodeJS 中处理多个环境

### 对于生产级应用程序，我们需要支持多种环境。例如，您将需要使用不同的…

github.com](https://github.com/Mohammad-Faisal/nodejs-environment-handling) [](https://javascript.plainenglish.io/error-handling-in-node-js-like-a-pro-ed210baa0600) [## 像专家一样处理 Node.js 中的错误

### 你需要知道的一切。

javascript.plainenglish.io](https://javascript.plainenglish.io/error-handling-in-node-js-like-a-pro-ed210baa0600) [](https://levelup.gitconnected.com/use-docker-with-nodejs-projects-like-a-pro-a9e7504e1308) [## 像专业人士一样使用 Docker 和 NodeJS 项目！

### 在开发和生产中利用 Docker 的力量

levelup.gitconnected.com](https://levelup.gitconnected.com/use-docker-with-nodejs-projects-like-a-pro-a9e7504e1308) [](https://javascript.plainenglish.io/node-js-logging-for-professionals-6be07c003e7f) [## 专业人员的 Node.js 日志记录

### 发挥温斯顿和摩根的全部潜力

javascript.plainenglish.io](https://javascript.plainenglish.io/node-js-logging-for-professionals-6be07c003e7f) [](https://javascript.plainenglish.io/node-js-database-with-docker-for-local-development-285212c5162f) [## Node.js +带有 Docker 的数据库，用于本地开发

### 各种数据库的一站式解决方案。

javascript.plainenglish.io](https://javascript.plainenglish.io/node-js-database-with-docker-for-local-development-285212c5162f)