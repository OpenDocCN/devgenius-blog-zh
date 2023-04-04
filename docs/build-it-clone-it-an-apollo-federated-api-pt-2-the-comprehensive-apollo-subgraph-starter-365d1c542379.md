# 建造它；克隆它；—一个阿波罗联合 API — Pt。2:全面的阿波罗子图启动器

> 原文：<https://blog.devgenius.io/build-it-clone-it-an-apollo-federated-api-pt-2-the-comprehensive-apollo-subgraph-starter-365d1c542379?source=collection_archive---------4----------------------->

大家好！感谢您的加入。

欢迎来建；克隆它；—和我一起构建你自己的子图启动库，或者跳到底部克隆完成的项目—选择权在你！

今天，我很乐意分享我的 starter repository，我用它在 Apollo 联合 API 中启动新的子图服务。它不仅仅是一个联邦 Apollo 服务器样板——它拥有真正的 API 所需要的特性！

所以如果你想和我一起 ***建造它*** 那么继续阅读。

如果丫只是需要 ***克隆它*** ，跳到最后！但也许可以直接阅读下面的功能。

![](img/d4e7d2b8b63f9ba23541def5b5f9d3c7.png)

# 启动器功能

如果它没有帮助我们编写特定于服务的代码的预构建功能，它就不是一个入门库，对吗？检查完成的存储库/我们的项目将具有的特性。

1.  Typescript —所有类型化和绝对导入
2.  联合子图/阿波罗服务器
3.  自动上下文生成、创建和定制
4.  CodeGen —生成类型和上下文
5.  mongose/Mongo DB 集成
6.  预构建的标准化分页、过滤和查找方法
7.  文档化开发、准备和生产设置(在 repo！)

# 项目设置

## 初始化

嘿，现在，让我们准备好。这里没什么特别的。创建一个项目目录，并用 NPM 初始化。

```
mkdir -p apollo-subgraph-starter/src
cd apollo-subgraph-starter
npm init -y
```

*俏皮的终端小把戏，用* `*mkdir -p*` *创建嵌套目录，即使根目录不存在。*

# 安装程序类型脚本

## 安装一些软件包

首先，安装 typescript 并创建一个`tsconfig.json`。

```
npm i typescript nodemon
touch tsconfig.json
mkdir types
```

## 配置 Typescript

除了一些非常标准的打字稿配置—

*   我们将包含`types`文件夹中的所有文件。
*   我们也在做一些工作，以实现绝对进口
*   我们定义输出目录，以便运行编译后的代码

将以下内容添加到`tsconfig.json`:

```
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es2018",
    "outDir": "./dist",
    "strict": true,
    "baseUrl": ".",
    "esModuleInterop": true,
    "skipLibCheck": true ,
    "forceConsistentCasingInFileNames": true,
    "paths": {
      "[@src](http://twitter.com/src)/*": ["src/*"]
    }
  },
  "include": [
    "types/**/*",
    "src"
  ],
  "exclude": ["node_modules"]
}
```

## 改变入口点

我更喜欢`server.js`而不是`index.js`——这似乎是一个更具描述性的名字。在`package.json`内改变入口点。

```
// package.json
{
  // ...other package.json properties
  "main": "dist/server.js",
}
```

## 修改启动脚本

为了启动服务器，我们需要将源代码从`src`编译到`dist`文件夹中。编辑`start`和`dev`和`scripts`来运行编译后的代码。

```
// package.json"scripts": {
    "start": "node app/dist/server.js",
    "dev": "tsc -w & nodemon dist/server.js",
    // For Docker
    // "dev": "tsc; tsc -w & nodemon dist/server.js",
    "build": "tsc",
    "generate": "graphql-codegen"
  },
```

docker 注意:如果您使用 Docker 和挂载卷进行开发，请尝试上面的 docker `dev`脚本。这将确保在启动容器中的服务器之前已经构建了 dist 文件。不过，更多的对话还是改天再说吧！

## 绝对进口

首先安装几个软件包:

```
npm i module-alias source-map-support
```

这两个包将有助于为任何模块创建自定义别名，并帮助编译后的代码找到正确的模块。

在`package.json`文件中，添加下面的模块别名，它将`@src/*`输入映射到编译后的`dist`文件。

```
"_moduleAliases": {
    "[@src](http://twitter.com/src)": "dist/"
  }
```

还有一个步骤，但是我们需要首先创建服务器文件。

```
touch src/server.ts
```

好的，在服务器文件中，我们需要完成绝对导入设置。确保这是前两行，并且永远保持前两行，至少根据文档是这样的。

```
// src/server.ts
import "module-alias/register";
import "source-map-support/register";
```

现在，我们有绝对进口！

```
// Example Only
import { whateverYouWant } from "@src/where-ever-you-want";
```

# 创建阿波罗服务器

## 设置服务器

接下来，我们将创建 Apollo 服务器。这超级简单，但我们需要几个包。

```
npm i apollo-server dotenv
```

导入`ApolloServer`，然后定义并调用服务器。

```
// src/server.ts
// ...imports
import { ApolloServer } from "apollo-server";
import { schema } from "@src/graphql";
import dotenv from "dotenv";
dotenv.config();const apolloServer = new ApolloServer({
  schema,
});const port = process.env.PORT || 5999;apolloServer
  .listen({ port })
  .then(({ url }) => console.log(`Service ready at ${url}`));
```

我们还导入并调用了`dotenv`来访问环境变量，这有助于保存在 Mongo URIs 和服务器端口等产品之间变化的变量。

## 该模式

我们还必须创建一个模式，因为`server.ts`应该会抛出一个 typescript 错误。

该模式将向服务器提供`typeDefs`和`resolvers`。为模式创建一个目录和文件。

```
mkdir src/graphql
touch src/graphql/schema.ts graphql/index.ts
```

从`index.ts`中导出模式，以便更清晰地导入。*从现在开始，我将在创建新目录时跳过这一步。我只是喜欢从索引文件中导出。*

```
// src/graphql/index.ts
export * from './schema';
```

并创建样板模式。我们很快会回到这个问题，但它至少会消除`server.ts`文件中`schema`的`no exported member`错误。

```
// src/graphql/schema.ts
import { buildSubgraphSchema } from "[@apollo/federation](http://twitter.com/apollo/federation)";export const schema = buildSubgraphSchema([]);
```

## 启用自动上下文生成

**什么是语境生成？**

上下文是在本地服务内部甚至在网络服务之间共享的数据。上下文可以来自服务本身的定制数据，或者更常见的来自传入的**请求头。**

下面是极其一般的低沉:

1.  客户端向微服务请求*某样东西*。
2.  为了访问微服务，他们必须向网关发送请求。
3.  网关完成它的工作并创建将在整个 API 中使用的`Context`。如果您需要了解这方面的更多信息，请查看我的 [Gateway 文章！](/build-or-clone-a-federated-api-pt-1-the-gateway-ad75dad21459)
4.  一旦在网关内创建了上下文，它就准备将请求发送到子图，以便解析来自客户端的请求。它将上下文作为头部附加到请求上，请求被发送到子图。
5.  子图接收带有标头的请求，可以从标头中提取信息，并将其作为上下文添加到服务中，然后可以从解析器访问该服务。

这意味着，在我们的子图中，我们应该期望在每个请求中出现某种类型的头。该标题将为我们提供在整个服务中用作上下文的信息。

为了让这个超级简单，我们将使用`@the-devoyage/micro-auth-helpers`包！

**启用生成**

安装并定义包的范围指向 GitHub 注册表并安装`@the-devoyage/micro-auth-helpers`。

```
echo [@the](http://twitter.com/the)-devoyage:registry=[https://npm.pkg.github.com](https://npm.pkg.github.com) >> .npmrc
npm i @the-devoyage/micro-auth-helpers
```

下面这条线会让我们的生活变得更容易。将下面的`context`属性添加到 Apollo 服务器的实例中。

```
// src/server.ts
const apolloServer = new ApolloServer({
  schema,
  context: ({ req }) => Helpers.Subgraph.GenerateContext({ req }),
});
```

此助手检查每个传入请求中可用作上下文的标头；默认情况下，它期望接收`req.headers.context`。

我们可以通过在网关服务中使用相同的包来确保网关发送正确的`context`头，因为它还提供了一个助手来生成适当的头。

此外，如果您的网关也在使用`@the-devoyage/micro-auth-helpers`包，那么您可以利用内置的`auth`上下文，该上下文会对每个请求进行解析。这是整个上下文对象的样子！它在我们的解析器中应该非常有用。

```
export interface Context extends Record<string, any> {
  auth: AuthContext;
  // ...other context sent from gateway
}export interface AuthContext {
  payload: Payload;
  isAuth: boolean;
  error?: string;
}
```

查看这里的包装！

[](https://github.com/The-Devoyage/micro-auth-helpers) [## GitHub-The-devo yage/micro-auth-helpers:帮助检查身份验证状态的函数集合…

### 一个功能集合，使处理认证、授权、上下文路由、文件…

github.com](https://github.com/The-Devoyage/micro-auth-helpers) 

如果您感兴趣的话，这里还有一篇关于请求如何通过联邦 API 的文章！

[](/apollo-federation-how-do-request-travel-through-a-federated-architecture-e4a4da54f46d) [## Apollo 联邦:请求如何通过联邦架构？

### 通过联合 Apollo 架构的网络请求之旅非常有趣——了解流程，同时…

blog.devgenius.io](/apollo-federation-how-do-request-travel-through-a-federated-architecture-e4a4da54f46d) 

让我们回到正轨。

# Mongo +分页+高级客户端过滤

在联邦 API 中处理分页、过滤和查找可能会很棘手，因为您希望标准化整个微服务中的请求方式。这可能意味着您可能正在管理重复的代码…这可不好玩。我们将通过几个快速步骤来避免这种情况。

## 连接 Mongo 和 Mongoose

你知道的，这是标准的猫鼬样本。

```
npm i mongoose
```

并建立联系…

```
// src/server.ts
import mongoose from "mongoose";
// ...importslet DB = process.env.MONGO_URI;mongoose
  .connect(DB, {
    useUnifiedTopology: true,
    useNewUrlParser: true,
    useCreateIndex: true,
    useFindAndModify: false,
  })
  .then(() => console.log("Mongo DB Connected..."))
  .catch((err) => console.log(err));// Start The Apollo Server Below
```

## 创建一个模型+实现查找过滤器和分页方法

只是为了在将来更快地设置，让我们创建一个模型来演示标准化的查找、过滤和分页。我们将需要另一个包来帮助这一点。

```
npm i @the-devoyage/mongo-filter-generator
mkdir src/models
touch src/models/model-example.ts
```

`mongo-filter-generator`提供了一个 mongoose 插件，使得查找、过滤和分页在整个 API 中变得简单和标准化。在`server.ts`文件中安装插件。务必在导入模式之前调用函数 ***！*** *和其他任何猫鼬插件一样。*

```
// src/server.ts
import { findAndPaginatePlugin } from "[@the](http://twitter.com/the)-devoyage/mongo-filter-generator";
import mongoose from "mongoose";
mongoose.plugin(findAndPaginatePlugin);// ...imports
```

关于这个包的一些额外阅读:

[](/easy-filter-mongo-ose-documents-b1d1232eba31) [## 轻松过滤蒙古文(ose)文档

### MFG 包为您的 mongo 启用的 API 添加了即时过滤功能和最少的代码

blog.devgenius.io](/easy-filter-mongo-ose-documents-b1d1232eba31) [](https://thedevoyage.medium.com/instant-api-pagination-and-filtering-with-mongo-filter-generator-228a57182116) [## 使用 Mongo-Filter-Generator 进行即时 API 分页和过滤

### 使用 mongo-filter-generator 包立即为您的 api 添加分页和过滤功能！

thedevoyage.medium.com](https://thedevoyage.medium.com/instant-api-pagination-and-filtering-with-mongo-filter-generator-228a57182116) [](https://github.com/The-Devoyage/mongo-filter-generator/packages/1228449) [## 软件包 mongo-filter-generator

### 用几行代码查找、过滤、分页 Mongo 过滤器生成器包允许客户端请求…

github.com](https://github.com/The-Devoyage/mongo-filter-generator/packages/1228449) 

让我们创建一个示例模型。

```
// src/models/example-model.ts
import { FindAndPaginateModel } from "[@the](http://twitter.com/the)-devoyage/mongo-filter-generator";
import mongoose from "mongoose";const Schema = mongoose.Schema;const ModelSchema = new Schema<any, FindAndPaginateModel>({
  name: { type: String, required: true },
});const Model = mongoose.model<any, FindAndPaginateModel>("Model", ModelSchema);export { Model };
```

由`@the-devoyage/mongo-filter-generator`提供的`FindAndPaginateModel`类型将为我们用上面的 mongoose 插件安装的新方法提供类型。这样，在使用这些新方法时，我们可以让 typescript 满意。

最后一步！我们需要为模式提供一些类型和解析器，以便在 graphQL 中使用`mongo-filter-generator`。

```
// src/graphql/schema.tsimport { buildSubgraphSchema } from "[@apollo/federation](http://twitter.com/apollo/federation)";
import { GraphQL } from "[@the](http://twitter.com/the)-devoyage/mongo-filter-generator";export const schema = buildSubgraphSchema([
  { typeDefs: GraphQL.typeDefs, resolvers: GraphQL.resolvers },
]);
```

***最终结果！***

现在，我们可以在解析器中使用自定义方法进行查找和分页，非常简单:

```
const { filters, options } = GenerateMongo(request)
const data = await model.findAndPaginate(filters, options);
```

我会给你看更多。

# 创建类型定义

组织类型定义和解析器绝对是你要考虑的事情。我试图保持每个子图看起来有组织和干净。这样，当在开发过程中切换子图时，我可以找到我需要的文件。

下面是我如何设置我的类型定义和子图文件结构。继续在您的项目中创建这些文件。

```
src/graphql
├── index.ts
├── resolvers
│   ├── index.ts
│   ├── mutation
│   │   ├── index.ts
│   │   └── mutation.ts
│   ├── query
│   │   ├── index.ts
│   │   └── query.ts
│   └── resolvers.ts
├── schema.ts
└── typeDefs
    ├── index.ts
    ├── example-model
    │   ├── index.ts
    │   └── example-model.ts
    ├── mutation
    │   ├── index.ts
    │   └── mutation.ts
    ├── query
    │   ├── index.ts
    │   └── query.ts
    └── typeDefs.ts
```

让我们从文件结构的深层开始，逐步解决这个问题。

## 类型定义

**型号定义**

按类型排序似乎是我最喜欢的组织类型定义的方式。首先，定义我们创建的示例模型。

```
// src/graphql/typeDefs/example-model/example-model.ts
import { gql } from "apollo-server";export const ExampleModel = gql`
  type ExampleModel {
    _id: ObjectID!
    createdAt: DateTime!
    updatedAt: DateTime!
    name: String!
  }
`;
```

我通常为每个为数据库创建的模型创建一个新的 defs 文件夹和文件。

**查询定义**

检查查询类型 Defs！*注意这里，这是轻松分页和过滤魔法的开始。*

```
// src/typeDefs/query/query.ts
import { gql } from "apollo-server-express";export const Query = gql`
  type GetModelsResponse {
    data: [ExampleModel]
    stats: Stats
  }input GetModelsInput {
    _id: StringFieldFilter
    createdAt: StringFieldFilter
    updatedAt: StringFieldFilter
    name: StringFieldFilter
  }extend type Query {
    getModels(getModelsInput: GetModelsInput!): GetModelsResponse!
  }
`;
```

`@the-devoyage/mongo-filter-generator`包提供了多种 graphql 类型来帮助过滤和分页。上面，我们定义了一个与`ExampleModel`的形状相匹配的输入，只有一点不同，我们指定了过滤器类型来定义如何查询每个属性。

`filter`类型由`mongo-filter-package`提供。它们允许客户端通过各种方法过滤所有文档。有许多滤波器选项可以满足大多数需求。[查看文档](https://github.com/The-Devoyage/mongo-filter-generator/packages/1228449)。

这个包还提供了一个`Stats`对象，可以向客户端共享分页信息…还剩多少文档，文档的页数…

**突变定义**

```
// src/graphql/mutation/mutation.tsimport { gql } from "apollo-server-express";export const Mutation = gql`
  input CreateModelInput {
    name: String!
  }input UpdateModelInput {
    _id: ObjectID!
    name: String
  }input DeleteModelInput {
    _id: ObjectID!
  }extend type Mutation {
    createModel(createModelInput: CreateModelInput!): ExampleModel!
    updateModel(updateModelInput: UpdateModelInput!): ExampleModel!
    deleteModel(deleteModelInput: DeleteModelInput!): ExampleModel!
  }
`;
```

将所有这些放在 typeDefs 文件本身中。

```
// src/graphql/typeDefs/typeDefs.tsimport { ExampleModel } from "./example-model";
import { Query } from "./query";
import { Mutation } from "./mutation";export const typeDefs = {
  ExampleModel,
  Mutation,
  Query,
};
```

最后，将`typeDefs`导入`schema`中。

```
// src/graphql/schema.ts
import { buildSubgraphSchema } from "[@apollo/federation](http://twitter.com/apollo/federation)";
import { typeDefs } from "[@src/graphql](http://twitter.com/src/graphql)/typeDefs";
import { GraphQL } from "[@the](http://twitter.com/the)-devoyage/mongo-filter-generator";export const schema = buildSubgraphSchema([
  { typeDefs: typeDefs.ExampleModel },
  { typeDefs: typeDefs.Query },
  { typeDefs: typeDefs.Mutation },
  { typeDefs: GraphQL.typeDefs, resolvers: GraphQL.resolvers },
]);
```

# 生成类型

我知道这可能看起来有点混乱…但是我们现在可以启动服务器并生成类型了。这样，我们可以在进行过程中键入解析器。

## 启动服务器

服务器需要运行才能生成类型。现在还不要担心 Mongo 连接，因为缺少与数据库的连接不会导致服务器崩溃。

```
npm run dev
```

## 生成类型

为了生成类型，我们将使用`graphql-codegen`包。

```
npm i -D [@graphql](http://twitter.com/graphql)-codegen/cli [@graphql](http://twitter.com/graphql)-codegen/typescript [@graphql](http://twitter.com/graphql)-codegen/typescript-resolverstouch codegen.yaml
```

创建一个文件来键入此服务的上下文。

```
mkdir types/context
touch types/context/index.d.ts
```

因为我们使用`@the-devoyage/micro-auth-helpers`包来生成上下文，所以我们可以扩展它提供的上下文类型，以包含我们自己的定制值(如果适用的话)。如果没有，只需创建一个名为`Context`的接口来定义您的自定义上下文。

```
// types/context.index.d.ts
import { Context as IContext } from "[@the](http://twitter.com/the)-devoyage/micro-auth-helpers";export interface Context extends IContext {
  //Add custom context types
}
```

设置 codegen 配置。

```
## codegen.yaml
schema: [http://localhost:5999/graphql](http://localhost:5002/graphql)
generates:
  ./types/generated/index.d.ts:
    config:
      useIndexSignature: true
      federation: true
      contextType: types/context#Context
    plugins:
      - typescript
      - typescript-resolvers
```

向 package.json 添加一个快速的`generate`脚本。

```
"generate": "graphql-codegen"
```

运行脚本！

```
npm run generate
```

现在您应该在`types/generated`中有了一个新文件，它包含了从 graphql 子图中生成的所有类型脚本类型。

这将使我们能够在整个服务中使用新生成的类型。

# **创建解析器**

是时候创建解析器并实现查找、过滤和分页功能了。

## 查询解析器

查询解析器将解析需要信息的请求，比如 restful API 中的`GET`请求。

```
// src/graphql/resolvers/query/query.ts
import { GenerateMongo } from "[@the](http://twitter.com/the)-devoyage/mongo-filter-generator";
import { QueryResolvers, Model as IModel } from "types/generated";
import { Model } from "[@src/models](http://twitter.com/src/models)";export const Query: QueryResolvers = {
  getModels: async (_model, args, _context) => {
    try {
      const { filters, options } = GenerateMongo({
        fieldFilters: args.getModelsInput,
        config: args.getModelsInput.config,
      }); const models = await Model.findAndPaginate<IModel>(filters, options); return models;
    } catch (error) {
      console.log(error);
      throw error;
    }
  },
};
```

就这样——我们实现了查找、过滤和分页！让我解释一下…

*   请求到达解析器。
*   `GenerateMongo`函数将 args(我们上面应用的过滤器)转换为输入类型 defs！)转化为`filters`，可应用于`model.find()`或`model.findAndPaingate()`方法。
*   可以简单地将`filters`和`options`传递给 mongoose，用`findAndPaginate()`方法查找过滤器和分页。

通过几行代码，我们现在已经在`query`解析器中实现了`finding`、`filtering`和`paginating`。

## 变异分解器

对于解析器，我们将设置一些标准解析器，这些解析器实现了我们已经安装的`@the-devoyage/micro-auth-helpers`包中的一些`auth`特性。

```
// src/graphql/resolvers/mutation/mutation.tsimport { Helpers } from "[@the](http://twitter.com/the)-devoyage/micro-auth-helpers";
import { MutationResolvers } from "types/generated";
import { Model } from "[@src/models](http://twitter.com/src/models)";export const Mutation: MutationResolvers = {
  createModel: async (_, args, context) => {
    try {
      Helpers.Resolver.CheckAuth({ context, requireUser: true }); const model = new Model({
        ...args.createModelInput,
        created_by: context.auth.payload.user._id,
      }); await model.save(); return model;
    } catch (error) {
      console.log(error);
      throw error;
    }
  },updateModel: async (_, args, context) => {
    try {
      Helpers.Resolver.CheckAuth({ context, requireUser: true }); const model = await Model.findOne(
        { _id: args.updateModelInput?._id },
        args.updateModelInput
      ); if (!model) {
        return Error("Does not exist.");
      } if (context.auth.payload.user._id !== model.created_by) {
        Helpers.Resolver.LimitRole({
          userRole: context.auth.payload.user.role,
          roleLimit: 1,
          errorMessage:
            "Only admin may edit models that are created by other users.",
        });
      } const updated = await Model.findOneAndUpdate(
        { _id: model._id },
        { ...args.updateModelInput },
        { new: true }
      ); return updated;
    } catch (error) {
      console.log(error);
      throw error;
    }
  },deleteModel: async (_, args, context) => {
    try {
      Helpers.Resolver.CheckAuth({ context, requireUser: true }); const model = await Model.findOne(
        { _id: args.deleteModelInput?._id },
        args.deleteModelInput
      ); if (!model) {
        return Error("Does not exist.");
      } if (context.auth.payload.user._id !== model.created_by) {
        Helpers.Resolver.LimitRole({
          userRole: context.auth.payload.user.role,
          roleLimit: 1,
          errorMessage:
            "Only admin may delete models that are created by other users.",
        });
      } await Model.deleteOne({ _id: model._id }); return model;
    } catch (error) {
      console.log(error);
      throw error;
    }
  },
};
```

`micro-auth-helpers`中的`CheckAuth`函数将使用网关提供的上下文来检查网关是否验证了用户是授权的，从而允许代码继续执行。[记住:我希望使用的网关也是使用`@the-devoyage/micro-auth-helpers`包构建的，所以它会向我们发送`auth`上下文！]

此外,`LimitRole`特性非常自描述，也有助于使用上下文来限制每个请求中的角色。

现在，把它们放在一起。

```
// src/graphql/resolvers/resolvers.ts
import { Resolvers } from "types/generated/index";
import { Query } from "./query";
import { Mutation } from "./mutation";export const resolvers: Resolvers = {
  Query: {
    Query,
  },
  Mutation: { Mutation },
};
```

就是这样！我们完了！我们坚持到了最后！现在是时候祈祷我们实现的逻辑能够正常工作了！

```
npm run dev
```

# 仓库

哇！我们坚持到了最后！

我们刚刚构建了一个子图启动器，它为 API 做好了 80%的准备——允许您更快地对实际项目进行编码。

## 克隆它

要克隆，非常简单——我已经在存储库前面建立了一个简单的付费墙。您的购买授权您访问协作和克隆它永远与所有的更新，铃声和哨声来了！在此获取访问权限=> [CloneTheRepo()](https://basetools.io/checkout/cgfbWjXq)

感谢您的支持！

嘿你们！

上周五手术进行得很顺利——如果你是新手，我必须修复我的前交叉韧带和半月板。好了，差不多 2 周了，我又能走路了，比计划的早了 4.5 周——当然！

虽然我只有两个星期的时间，但我错过了一些非常重要的事情。你有没有意识到散步对于解决关键问题有多重要？我想我可能最怀念这个了。一次愉快的散步能如此之快地激发你用代码解决一个问题，这太疯狂了。

我有四个新的 starter 服务教程准备用于这个 API——全部构建在您刚刚构建或从本文克隆的子图 starter 之上！接下来将是帐户服务与认证和更多！请继续关注，以便我们可以构建或克隆它。

感谢您查看这篇文章，并伸出援手——这意义重大！

尼克·M