# 我如何使用 JavaScript 构建支付网关。

> 原文：<https://blog.devgenius.io/how-i-build-a-payment-gateway-using-javascript-2e4f3e52b7cd?source=collection_archive---------0----------------------->

![](img/56f6fbf562f5299685483ffe87d0fcbf.png)

潘卡杰·帕特尔在 [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在我的大学学习后(大约 5 年前)，我有一个愿望，那就是建立一个项目，不仅仅是为了创收，而是为了让其他人和团队也能创造和管理他们的收入。

尽管我学的是信息技术，但我对 JAVA 和 C/C++语言知之甚少，几乎不能用它们创建任何应用程序，但它们是令人惊叹的开发语言。我大胆尝试了简单易学的语言，如 JavaScript、Python 和 PHP(是的，我知道你在想什么，但 PHP 对我来说似乎很简单)。我能够在不同的工作环境中使用它们各自的框架，如 NodeJS 和 VueJS(用于 JavaScript)、Django(用于 Python)和 CodeIgniter、Symfony、Laravel(用于 PHP)。

# **项目**

最后，我回到创建自己的项目的时候到了。我进行思考，研究我能解决的问题。我决定首先研究我所在的国家和地区乌干达(东非)在创收和管理收入方面所面临的挑战，因此产生了开发一个简单而强大的支付平台的想法，以服务于个人、公司/组织和其他开发商，将可用的支付方式(移动货币和卡支付)集成到他们的日常交易活动中。

该平台将拥有与移动货币网络等第三方服务提供商直接通信的网关，服务于前端并帮助开发者将平台功能集成到自己的应用程序中的 api(后端),以及供用户管理交易(钱包)和执行其他支付(如充值通话时间、数据包等)的用户界面。以及后来的一个补充 web 应用的移动应用。

# **结构**

当谈到选择使用的语言和/或框架时，我决定选择 JavaScript。这是因为在所有语言中，我想使用一种我有更好经验的语言，这种语言可以为我提供用一种语言创建前端和后端的能力，因为一开始这是一个人的团队。

**网关**

该网关使用 NodeJS，凭借其单线程模型和良好的性能，我相信这将提供一个良好的开端，因为正在进行监控，以查看我们是否需要随着用户和请求的增加而进行更改。选择的数据库是 MongoDB，它提供了快速读写能力来记录事务状态，这将用于更新项目的 api 部分。这将使用 MongoDB 的 [TTL](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-TTL) 收集特性短时间存储数据，大部分或所有数据将由 API 存储。

**API**

该 API 还使用 NodeJS，选择的数据库是 Postgres over MySQL，因为它提供了各种数据类型，我将使用基本字符和数字数据类型中的独特数据类型，并且它能够轻松扩展到企业级复杂查询和频繁写操作。该 API 将能够服务于仪表板(WebApp/frontend)、开发者 API 和移动应用。

有许多 NodeJS api 样板项目，您可以利用它们来快速开始您的开发，下面是我从中获得灵感的一些项目:

[](https://github.com/hagopj13/node-express-boilerplate) [## GitHub-hago pj 13/node-express-boilerplate:用于构建生产就绪的 RESTful 的样板文件…

### 一个样板/入门项目，用于使用 Node.js、Express 和 Mongoose 快速构建 RESTful APIs。通过运行一个…

github.com](https://github.com/hagopj13/node-express-boilerplate) [](https://github.com/danielfsousa/express-rest-boilerplate) [## github-danielfsousa/express-rest-boilerplate:用于构建 restful apis 的⌛️快速入门

### 构建 restful apis 的⌛️快速入门。通过以下方式为 Daniel Sousa/express-rest-boilerplate 开发做出贡献…

github.com](https://github.com/danielfsousa/express-rest-boilerplate) [](https://github.com/madhums/node-express-mongoose) [## GitHub-madhums/node-express-mongose:一个样板应用程序，用于使用 node…

### 一个样板应用程序，用于使用 express、mongoose 和 passport 构建 web 应用程序。阅读维基以了解如何…

github.com](https://github.com/madhums/node-express-mongoose) 

**前端**(仪表盘)

该仪表板将使用户能够收集，自助存款和取款，检查他们的余额，并执行其他交易，如充值数据包。这将使用 VueJS 框架和 NuxtJS 作为用户界面，并为用户完成的不同活动处理通知和警报。

我将在下一篇文章中分享更多这个项目的上述 3 个组成部分的分类，例如认证、授权、请求、响应和错误处理等等。

谢谢你的阅读。

更多节点 api 锅炉板项目:

[](https://github.com/kunalkapadia/express-mongoose-es6-rest-api) [## GitHub-kunalkapadia/express-mongose-es6-rest-API:用于构建 RESTful 的样板应用程序…

### collision:在 Node.js 中使用 express 和 mongoose 构建 RESTful APIs 微服务的样板应用程序…

github.com](https://github.com/kunalkapadia/express-mongoose-es6-rest-api)