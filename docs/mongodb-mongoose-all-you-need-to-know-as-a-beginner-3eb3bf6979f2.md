# MongoDB 和 mongose——作为初学者，您需要知道的一切

> 原文：<https://blog.devgenius.io/mongodb-mongoose-all-you-need-to-know-as-a-beginner-3eb3bf6979f2?source=collection_archive---------7----------------------->

## 在[我的技术文章](https://yumingchang1991.medium.com/technical-article-structure-on-medium-954850e1ef4d)中查看我所有的其他帖子

写这篇文章是为了帮助未来的我回忆起 MongoDB 的主要概念。内容可能不正确，所以如果你找到了，请在评论中告诉我，让我们一起成长。

## 主题包括

*   什么是 MongoDB
*   什么是 MongoDB Atlas
*   什么是猫鼬
*   在 Node.js 中使用 Mongoose 连接到 MongoDB Atlas
*   使用 Mongoose 创建模式
*   基于模式创建模型
*   MongoDB 中的 CRUD

## 排除的主题

*   如何设置 MongoDB Atlas

# 什么是 MongoDB

*   它是一个 noSQL 数据库，或非关系数据库管理系统
*   它的基本构建块是 JSON 文档，存储在一个集合中
*   MongoDB 中的集合就像关系数据库中的表一样
*   集合级没有模式，这使得 MongoDB 构建起来更加灵活和快速。在 Mongoose 的帮助下，它的模式被定义为一个文档数据结构。**注**:关系数据库在表定义中定义了模式，以保证数据的完整性

# 什么是 MongoDB Atlas

*   MongoDB Atlas 是 MongoDB 的云解决方案。它基本上是 MongoDB 的云版本，为我们处理部署工作

# 什么是猫鼬

*   Mongoose 是 MongoDB 和 Node.js 的对象数据建模(ODM)库
*   它提供了规范化数据的模式解决方案

# 使用 Mongoose 连接到 MongoDB

我们需要首先进口猫鼬在我们的。js 文件。有两种方法可以做到:

1.  使用`require('mongoose')`
2.  使用 ES6 语法`import mongoose from 'mongoose'`

我个人选择使用 require，这是专门为 NodeJS 设计的方法。在这篇文章的底部有讨论需求和导入的参考文章。你可能想去看看。

```
const mongoose = require('mongoose')
mongoose.connect(YOUR_MONGODB_URI, { useNewUrlParser: true })
```

MongoDB Atlas URI 看起来如下，但确保您直接从 MongoDB Atlas connection 获得，以防有变化:

mongodb+srv:// **<用户名>** : **<密码>** @ **<集群名>**. MongoDB . net/**<数据库名>**

# 使用 Mongoose 创建模式

首先，MongoDB 没有提供内置的模式来管理它可以接受的数据的“形状”。这就是 MongoDB 的灵活性所在。

然而，我们不希望我们的代码将一个我们不期望的文档保存回 MongoDB 集合。这就是模式发挥作用的地方。

为了声明模式，我们使用 mongoose 的 schema()方法，该方法接受一个描述我们期望的数据形状的对象。

```
const mongoose = require('mongoose')
const userSchema = **new** **mongoose.schema**({
  username: String,
  age: Number
})
```

从[官方文件](https://mongoosejs.com/docs/schematypes.html)中查看允许哪些图式。

# 基于模式创建模型

模式只是所需数据形状的蓝图，仅用于创建模型。

模型是我们用来创建文档实例的构造器。

请记住，所有 CRUD 操作都是在模型上执行的。

要在定义模式后声明模型，

```
**const User = mongoose.model('User', userSchema)**
// **'User'** is the name for the model
// **userSchema** is the schema we just defined in last section
```

# MongoDB 中的 CRUD

CRUD 操作相对简单，我发现官方文档是学习它的最佳资源。

让我感兴趣的是基本上所有 CRUD 操作中的回调函数。我正在向 freeCodeCamp 学习，他们使用的示例回调函数叫做 done。

我不是 100%理解为什么我们每次完成一些任务时都需要使用回调函数。然后我发现[这个响应](https://stackoverflow.com/questions/14220321/how-to-return-the-response-from-an-asynchronous-call/14220323#14220323)关于栈溢出。

这是一个简短的回答摘要，但是我鼓励你花 20 分钟来阅读它。

*   JavaScript 正在异步运行
*   回调函数只在每个函数结束时执行，以便让其他函数知道这个异步函数何时完成
*   当你熟悉了异步和回调之后，再深入这两个主题，因为 JS 并不是一直异步运行的，回调函数也不总是在函数结束时执行。对于初学者来说，这似乎是一个超前的话题，所以让我们暂时把它放在一边。

# 参考

*   **文章** : [*尼克·卡尼克为 MongoDB*](https://www.freecodecamp.org/news/introduction-to-mongoose-for-mongodb-d2a7aa593c57/) 介绍猫鼬
*   **文章**:[*IBM 的 MongoDB*](https://www.ibm.com/cloud/learn/mongodb) 是什么
*   **Article** : [*导入 v.s. Require in Node.js*](https://masteringjs.io/tutorials/node/import-vs-require) 通过掌握 js，2020
*   **文章** : [*导入、导出并要求在 JavaScript 中*](https://fjolt.com/article/javascript-export-import-node-js) 作者 Johnny Simpson，2022
*   **手册** : [*公文*](https://mongoosejs.com/docs/index.html) 由猫鼬
*   **手册** : [*公文*](https://www.mongodb.com/docs/guides/?_ga=2.187374316.519583772.1648536708-619028073.1648536708) 由 MongoDB