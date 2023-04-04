# 简单快速 API

> 原文：<https://blog.devgenius.io/simple-express-api-2aeec5b6855c?source=collection_archive---------0----------------------->

## 创建一个简单的快速 API

![](img/47e117e1977ea30ecb2b1310da38538d.png)

照片由[米切尔罗](https://unsplash.com/@mitchel3uo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/api-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在不深入了解发生了什么的情况下，我只想在这篇文章的开头说:我希望你们都保持安全，保持活跃，并在疫情期间努力成为更好的自己。

#安全停留

随着时间在室内飞逝，我试图扩展我的技能。今天，我看了一下 [ExpressJS](https://expressjs.com/) ，这是一个用于 [NodeJS](https://nodejs.org/en/) 的快速、非个性化、极简的 web 框架。

> 在写这篇文章的时候，ExpressJS 4.x 是最新的版本，alpha 中有 5.x。

有些人可能知道，我完成了我的学位，完成了一个用打字稿写的项目([见这里的项目](https://github.com/srepollock/divine-engine))。这让我开始涉足 JavaScript 社区。仍然有一些我不知道的东西，但是我希望扩展我的 web 技能，包括前端和后端，不仅给出一些 JavaScript 相关内容的实例，而且给出其他项目和工作的实例。

## 快速表达

Express 是一个框架，用于创建 web 应用程序、API 等，重点关注性能。

ExpressJS 不使用纯粹的 NodeJS——在 NodeJS 中，您必须编写自己的有效负载解析、cookies、存储会话、选择正确的路由模式——而是用更少的代码为您处理所有这些。

Express 主要用于处理请求和传递数据。它是 ***而不是*** 一个 MVC——你必须自己带来——但是可以为终端用户提供预先生成的静态文件。

在这个例子中，我们将关注 Express 的 API 端点。

> Express 也可以提供静态文件，并且有一个叫做 [express-generator](https://github.com/expressjs/generator) 的奇特工具，可以帮助搭建一个带有预定义静态站点引擎的简单站点。

# 这个例子

首先，需要一些先决条件:

## 安装节点

就在这里-> [NodeJS](https://nodejs.org/en/download/)

使用以下命令检查安装的版本(NodeJS/NPM ):

`$ node -v; npm -v`

## 创建新目录

`$ mkdir myapp; cd myapp`

-**M**a**k**e**方向**目录
- **C** 改变 **d** 目录

## 在本地创建 npm 项目和 git repo

`$ npm init — yes; git init — quiet`

-使用默认值初始化 npm 项目

-初始化 git repo 并抑制输出

## 安装 ExpressJS

`$ npm install — save express`

-将 express 模块作为项目依赖项安装

## 创建` index.js '

`$ touch index.js`

然后可以用几行代码设置 Express:

```
*// index.js* var express = require(‘express’);
var app = express()
app.get(‘/’, (request, response) => { response.send(“Hello world!”); })
app.listen(3000);
```

解释:

express 变量被设置为模块“express”。该应用程序被设置为随叫随到的 express 实例。然后，应用程序可以在“/”端点(在基本 URL 之后)响应 get 请求，并发送“Hello world！”的请求。最后，应用程序监听端口 3000。

要运行，只需在命令行中输入 *`$node index.js`* 。

您也可以在 *`package.json`* 中设置“脚本”来运行
在 *`script: {}`* 括号:
`"start”: “node index.js”,`中添加该行，然后调用`$ npm start`来代替。

运行时，API 响应将位于:`http://localhost:3000`

现在您已经有了一个样例 Express API！

我开始用 JavaScript/TypeScript 比以前多写几个应用；我希望还有其他超越知识的应用程序。这是练习、工作和旅程日志。我期待着使更多的项目和职位。随着隔离期间时间的飞逝，我将在我的 GitHub 上发布更多的工作，所以如果你也想了解那里的最新情况，请给我一个关注。

感谢你花时间阅读这篇文章，让我对自己有了更多的了解。期待前方的路。

祝你一切顺利——斯潘塞