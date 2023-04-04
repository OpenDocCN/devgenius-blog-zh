# 带有 React |语义 UI | GraphQL | PostgresSQL 的 Slack 克隆(第 1 部分)

> 原文：<https://blog.devgenius.io/slack-clone-with-react-semantic-ui-graphql-postgressql-part-1-cd40b5d3460?source=collection_archive---------5----------------------->

![](img/b089ec0138a431114b105aafd8611271.png)

沃洛季米尔·赫里先科在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

嘿大家，这个项目将是一个系列。我不知道这个系列会有多长，因为在我写这些文章的时候，我还在做这个项目。我想开发一个聊天应用已经有一段时间了。我偶然发现了一个更老的教程(3 年前)，是关于**本阿瓦德**(牛逼的 YouTuber)做一个 slack 克隆的，对我来说很完美，所以我跟随他的方法，把我的做了一个更新版本(3 年来很多都变了)。

我想练习构建更复杂的项目。到目前为止，我学到了很多东西，比如使用 PostgresSQL 数据库，为 ORM 使用 Sequelize，并将其与 Graphql 连接。所以我希望你们也能学到一些东西:)但是介绍到此为止，让我们开始第一部分吧。

## 数据库安装

在我们得到好东西之前，我们需要为这个项目安装我们需要的东西。我将在整个系列中使用 Mac。

1.  当然是:)(如果你还没有=>[https://nodejs.org/en/download/](https://nodejs.org/en/download/))
2.  **PostgreSQL**(针对 Windows 和 Mac[https://www.postgresql.org/download/](https://www.postgresql.org/download/))
    安装视频
    Mac 视频:[https://www.youtube.com/watch?v=EZAa0LSxPPU](https://www.youtube.com/watch?v=EZAa0LSxPPU)
    Windows 视频:[https://www.youtube.com/watch?v=RAFZleZYxsc](https://www.youtube.com/watch?v=RAFZleZYxsc)
3.  这是你的数据库的图形用户界面。(适用于 mac)

这就是使用 Postgres 设置数据库部分所需的全部内容(没有那么多)。在下一个，我们将工作在文件夹设置和安装我们后端需要的软件包。在那之前，伙计们:)