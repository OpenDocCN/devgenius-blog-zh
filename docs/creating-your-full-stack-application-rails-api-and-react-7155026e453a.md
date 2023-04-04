# 创建全栈应用程序:Rails API 和 React

> 原文：<https://blog.devgenius.io/creating-your-full-stack-application-rails-api-and-react-7155026e453a?source=collection_archive---------1----------------------->

## 想通过构建自己的应用程序来测试自己的技能吗？下面是如何创建和连接 GitHub 一个 Rails API 和 React app 的步骤！

![](img/df9b5325d8e3e2fcf2a3aed131ebde7b.png)

林赛·亨伍德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 目录

1.  [简介](#0b54)
2.  [概述](#950b)
3.  [前期步骤](#06e0)
4.  [创建 Rails API &连接到 GitHub](#2eab)
5.  [创建 React 前端&连接到 GitHub](#a149)
6.  [结论](#7029)
7.  [资源](#cb44)

# **简介**

在我最近的项目中，我用 Rails API 后端和 React/Redux 前端构建了一个全栈应用程序。这是我最新的项目，也是我在熨斗学校的最后一个项目，因为我已经毕业了！我真的很喜欢做一个全栈开发者，并计划开发更多的附带项目；毕业对我来说不是结束而是开始。

因为我计划前端使用 React，后端使用 Rails API，所以我拿出了我的笔记本来记录我正在做的事情。当我试图写下如何在本地创建后端和前端存储库*以及*并在 GitHub 上连接它们的步骤时，我开始混淆它们。所以为了跟踪这些步骤，也为了其他需要提醒的人，我把这个分步指南留在这里！

# 概观

下面的指南将在考虑部署的情况下编写。为了让应用程序更容易部署，最好让后端和前端位于 Github 上两个独立的存储库(repos)上。也就是说，尽管他们将远程地生活在两个独立的回购上，但在本地你可以让他们生活在同一个文件夹中。因此，当您工作时，保持两个代码编辑器和终端打开，一个用于前端，一个用于后端。这意味着你将把你的代码推到两个回购。

**提示**:由于您将打开两个终端，请记住您将提交哪个 GitHub repo 当我推送我的代码时，有时我会忘记我在哪个终端，所以我把提交消息写到我的后端，而把我的前端记在心里，反之亦然(哎呀！😅)

# 早期步骤

在我们创建前端和后端之前，让我们创建一个目录来本地保存它们。

1.  在您的本地计算机上创建一个新的项目目录(文件夹)。

```
mkdir <new-project-name>
```

2.进入目录

```
cd <new-project-name>
```

# **创建连接到 GitHub** 的 Rails API &

在我们创建 Rails API 之前，我建议用 Postgres 设置数据库，因为它比 SQLite 更容易部署。如果你没有安装 Postgres，你可以从[这里](https://postgresapp.com/)下载。但是如果你还不确定你想要什么，不要担心！以后你可以随时从 SQLite 切换到 Postgres 看看布兰登·桑顿的[文章](https://medium.com/@thorntonbrenden/ruby-on-rails-switch-from-sqlite3-to-postgres-590009645c25)来学习如何做。

1.  使用以下内容创建新的 Rails API:

```
rails new <app_name> --api --database=postgresql 
```

提示:我建议使用“backend”作为应用程序名称，因为它在项目目录中。

**注意**:数据库=postgresql 用 Postgres 创建数据库

2.切换到新创建的文件夹

```
cd <app_name>
```

3.在您的终端中运行:

```
rm -rf .git
git init
```

4.导航到[https://github.com/new](https://github.com/new)，在 GitHub 上创建一个新的存储库

5.在 GitHub 上命名并创建存储库

**提示**:我建议使用“app-name-backend”作为回购名称；您在这里给它起的名字不需要与它在本地的名字相同。

**注意**:不要勾选“用自述文件初始化”按钮。

6.回到您的终端，运行以下命令:

```
git add .
git commit -m "first commit"
git remote add origin … (your SSH)
git branch -M main
git push -u origin main
```

# **创建 React 前端&连接到 GitHub**

在我们做任何事情之前，我们首先需要用 npm 在你的计算机上安装 create-react-app。如果您已经安装了它，您可以跳过这一步，但是如果您不确定您仍然可以运行以下命令:

```
npm i -g create-react-app
```

有趣的事实:“I”标志表示安装，“-g”表示全球安装

**2021 年 3 月 22 日更新**:你不再需要全局安装 create-react-app。如果您以前在全局范围内安装了它，建议将其卸载，以确保 npx 始终使用最新版本:

```
npm uninstall -g create-react-app
```

1.  如果您没有改回您的项目目录:

```
cd ..
```

2.使用以下内容创建新的 React 应用程序:

```
npx create-react-app <app_name>
```

**提示**:我建议使用“前端”作为应用程序名称，因为它在项目目录中。

3.更改到新创建的文件夹:

```
cd <app_name>
```

4.在您的终端中运行:

```
rm -rf .git
git init
```

5.导航到[https://github.com/new](https://github.com/new)，在 GitHub 上创建一个新的存储库

6.在 GitHub 上命名并创建存储库

**提示**:我建议使用“app-name-backend”作为回购名称；您在这里给它起的名字不需要与它在本地的名字相同。

**注意**:不要勾选“用自述文件初始化”按钮。

7.回到您的终端，运行以下命令:

```
git add .
git commit -m "first commit"
git remote add origin … (your SSH)
git branch -M main
git push -u origin main
```

# 结论

耶，你已经创建了你的 Rails API 和 React 前端，并将它们连接到 GitHub！

这些可能是我们每天做的简单步骤，但有时你需要被提醒一些基本的东西。现在您已经创建了项目，如果您喜欢这篇文章，请查看我的相关文章"[设置您的全栈应用程序:Rails API 和 React/Redux](https://medium.com/dev-genius/setting-up-your-full-stack-application-rails-api-and-react-redux-428ff7a7a7c0) "！

非常感谢你的阅读，祝你的✨项目好运

# **资源**

📖【https://kbroman.org/github_tutorial/pages/init.html[📖](https://kbroman.org/github_tutorial/pages/init.html)[https://docs . github . com/en/free-pro-team @ latest/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line](https://docs.github.com/en/free-pro-team@latest/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)