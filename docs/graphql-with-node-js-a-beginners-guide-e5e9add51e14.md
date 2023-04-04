# 带有 Node.js 的 GraphQL:初学者指南

> 原文：<https://blog.devgenius.io/graphql-with-node-js-a-beginners-guide-e5e9add51e14?source=collection_archive---------7----------------------->

![](img/34aae13d1a4a6190396a49c4f6f33c62.png)

带有 Node.js 的 GraphQL

*很长一段时间，我在 GraphQL 上工作，但是是在 DotNet 里。我打算使用 Node.js 与 GraphQL 一起工作，所以我做了一些研究，读了一些文章，把它们放在一起，它工作了。因此，我决定为像我一样想用 node.js 学习 GraphQL 的人做一个循序渐进的指南*

# 要求你所需要的，得到它！

向您的 API 发送一个 GraphQL 查询，得到您所需要的，不多也不少。GraphQL 查询总是返回可预测的结果。使用 GraphQL 的应用程序快速而稳定，因为它们控制着自己获得的数据，而不是服务器。

# 入门指南

如果你还没有，[下载并安装 Node.js](https://nodejs.org/en/download/) 。安装后，让我们从目录结构开始。

该项目分为两个目录，一个用于客户端，一个用于服务器。我选择将这两个项目都放在项目根目录中，但是如果您愿意，也可以将它分成两个不同的项目。

📁项目
├──📁客户
└──📁计算机网络服务器

我们现在将在服务器目录中启动项目。在您的终端中，导航到服务器文件夹并执行 npm init 来填充项目信息并生成 package.json 文件。

我们的服务器将配置有 [GraphQL.js](https://github.com/graphql/graphql-js) 和 [Apollo 服务器](https://github.com/apollographql/apollo-server)。GraphQL.js 将提供两个关键特性:

*1。创建一个类型模式，我们将在下面的步骤中完成。
2。使用这种模式来处理查询。*

# ***如何定义模式:***

任何 GraphQL 服务器实现都依赖于 GraphQL 模式。它使用由数据源生成的类型和字段的层次结构来定义数据的形式。它还指定了哪些查询和变化是可用的，允许客户机知道可以请求或提供什么数据。

例如，如果我们想制作一个草图应用程序，我们最基本的模式(通常在 schema.graphql 文件中定义)将包括两种对象类型:草图和艺术家，如下所示:

```
type Sketch
{ 
title: String
artist: [Artist]
}type Artist
{
name: String
sketch: [Sketch]
}
```

然后，可用的查询将由查询类型定义:getSketches 和 getArtists，每个查询返回匹配类型的列表。

```
getSketches: [Sketch] type Querytype Query 
{
getSketches: [Sketch]
getArtists: [Artist]
}
```

我们的模式将只有一种查询类型，它将返回一个字符串，以使事情尽可能简单。

状态:字符串:类型查询

为了设计一个 GraphQL 模式并围绕它构建一个接口，我们可以使用任何编程语言，但是正如我前面提到的，我们将利用 Apollo 服务器来执行 GraphQL 查询。

因此，在服务器目录下，我们创建一个新的 server.js 文件来描述模式。

📁项目
├──📁客户
└──📁服务器
└──📄server.js

现在运行 npm install apollo-server 来安装 apollo-server。

为了解析模式，我们需要从 apollo-server 导入标记函数 gql:然后创建一个 typeDefs 常量，这是 Graphql 代码的抽象语法树。var gql = require(' Apollo-server ')；

字符串是 GraphQL 服务器接收查询进行处理时最常见的格式。这个字符串必须被标记化并处理成机器可读的表示形式。抽象语法树是这种结构的名称。

AST Explorer 是一个在线工具，如果您想了解更多关于抽象语法树的知识，它可以让您作为解析器检查由所选语言形成的语法树。

server.js 文件如下所示:

```
const { gql } = require(‘apollo-server’);
const typeDefs = gql
type Query {
status: String
};
```

# **如何在你的代码中包含解析函数**

既然我们已经定义了模式，我们将需要解析器来响应客户端对该数据的请求。

解析器是管理每个模式字段的数据的功能。例如，您可以通过从后端数据库或第三方 API 检索数据来将数据交付给客户端。

它们必须与模式的类型定义相匹配。在我们的示例中，我们只有一个类型定义 Query，它返回一个字符串状态，因此我们将为 status 字段构造一个解析器，如下所示:

```
const resolvers = {
Query: {
status: () => ‘Hello GQL Server!👋’,
},
};
```

# **如何安装和配置服务器:**

我们在同一个 server.js 文件中定义并构建一个新的 ApolloServer 对象，将模式(typeDefs)和解析器作为参数传递。

```
const { ApolloServer, gql } = require(‘apollo-server’);
const server = new ApolloServer({ typeDefs, resolvers });
```

然后，使用 listen 方法在 params 中指定的端口上启动服务器。

```
server.listen({ port: 3000 }).then(serverInfo => 
console.log(`Server running at ${serverInfo.url}`));
```

我们还可以在登录时**析构**server info URL。

```
server.listen({ port: 3000 }).then(({ url }) => 
console.log(`Server running at ${url}`));
```

server.js 文件现在应该是这样的:

```
const { ApolloServer, gql } = require(‘apollo-server’);
const typeDefs = gql.type Query 
{
status: String
};
const resolvers = {
Query: {
status: () => ‘GQL Server!👋’,
},
};
const server = new ApolloServer({ typeDefs, resolvers });
server.listen({ port: 3000 }).then(({ url }) => 
console.log(`Server running at ${url}`));
```

现在，如果我们运行 node server/server.js，我们将最终启动并运行我们的 GraphQL 服务器！🎉

```
You can go and check it out on [http://localhost:3000/](http://localhost:9000/)
~/graphql-server
> node server/server.js
Server running at [http://localhost:3000/](http://localhost:9000/)
```

如果您曾经处理过 RESTful 请求，您会发现这是一个类似邮递员的接口。只是你不需要下载或配置任何东西，因为它已经包含在 Apollo 中了！

# 如何设置客户端？

既然我们的服务器已经启动并运行，让我们把注意力集中在客户机上。首先，我们将在客户端子目录中创建一个 client.html 文件。

index.html 文件将有标准的 HTML 元素以及一个加载头(h1 >)。

```
h1>Loading…/h1> to display something to the user while we request data from the server<!DOCTYPE html>
<html lang=”en”>
<head><meta charset=”UTF-8" />
<meta name=”viewport” content=”width=device-width, initial-scale=1.0" />
<title>GQL Server</title>
</head>
<body>
<h1>Loading…</h1>
<script src=”app.js”></script>
</body>
</html>
```

**如何从服务器访问数据？**

首先，我们在同一个客户机文件夹中创建一个 app.js 文件，我们将在这个文件夹中编写从服务器获取数据的客户机逻辑。

📁项目
├──📁客户
| └──📄client.html
|└──📄└──📁服务器
└──📄server.js

我们将服务器 URL 设置为发出请求的 URL。

```
const GRAPHQL_URL = ‘http://localhost:3000/';
```

然后，为了从服务器获取状态，我们定义了异步函数 fetchStatus()。为了发出 HTTP 请求，我们将利用 fetch API，默认情况下，它会生成一个承诺，我们可以异步订阅和检索响应。

```
async function fetchStatus() 
{
const response = await fetch(GRAPHQL_URL, {
method: ‘POST’,
headers: 
{
‘content-type’: ‘application/json’,
},
body: JSON.stringify({
query: `query 
{
status
}
`,
}),
});
const responseBody = await response.json();
console.log(responseBody);
}
```

请求的方法是 POST，这一点需要记住。如果我们习惯使用 RESTful，这可能会令人困惑，因为 RESTful 的一个类似请求(我们只想从服务器获取信息)通常是用 get 方法发出的。

问题是在使用 GraphQL 时，我们总是以查询作为有效负载(主体)来发送 POST 请求。

最后，我们调用 fetchStatus()方法。

```
const GRAPHQL_URL = ‘http://localhost:3000/';
async function fetchStatus() 
{
const response = await fetch(GRAPHQL_URL, {
method: ‘POST’,
headers: 
{
‘content-type’: ‘application/json’,
},
body: JSON.stringify({
query: `query 
{
status
}
`,
}),
});
const responseBody = await response.json();
console.log(responseBody);
}
fetchStatus();
```

如果您在浏览器中打开该文件并查看开发人员工具中的控制台，您可以看到我们从查询中获得了状态数据。希望有帮助。

**参考资料:**
https://graphql.org/
https://reindex.io/

此外，我很想知道你对我能做些什么在这方面产生更好的影响的想法？