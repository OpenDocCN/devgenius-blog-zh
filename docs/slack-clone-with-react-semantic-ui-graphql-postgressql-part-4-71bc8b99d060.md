# 带有 React |语义 UI | GraphQL | PostgresSQL 的松弛克隆(第 4 部分)

> 原文：<https://blog.devgenius.io/slack-clone-with-react-semantic-ui-graphql-postgressql-part-4-71bc8b99d060?source=collection_archive---------11----------------------->

## 之前，我们启动了数据库。你可以在这里找到那篇[文章。](https://medium.com/dev-genius/slack-clone-with-react-semantic-ui-graphql-postgressql-part-3-f42515446c80)

![](img/20839c740b90ba944468979690f01820.png)

沃洛季米尔·赫里先科在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

今天，我们将简要讨论 Graphql 查询和变异。

# 简单介绍一下 GraphQL API

# 类型:查询、突变、自定义

类型定义了你的端点是什么，并描述了它们应该返回什么。

查询如下所示= >

```
type Query {
    getColors: [String]!
    getNames: [String]
    sayName: String!
  }
```

(例如:`getColors`需要返回一个字符串数组，这些字符串将是颜色名称)。感叹号表示项目不能为空。类型查询类别将是您的 GET 端点。

突变是这样的= >

```
type Mutation {
    sayHello(message: String!): String!
  }
```

同样的规则也适用于突变。唯一的区别是，类型突变类别将是您的 POST、PUT、DELETE 端点。

自定义类型如下所示= >

```
type User {
  name: String!
  age: Int!
  bio: String!
}
```

这是一个常规的自定义对象，有 3 个属性描述它(`name, age, bio`)，您可以像 so = >一样使用它

```
type Query{
/** returns array of users */
  getUsers: [User!]
  getUser: User!
}type Mutation {
/** creates a user, returns that user */
  createUser: (name: String!, age: Int!, bio:String!): User
}
```

# 解析器:查询和突变

*解析器返回您在类型中描述的实际数据。您的查询和突变名称需要与您在* `*type query*` *类别*中描述的名称相匹配

解析器中的查询如下所示= >

```
Query: {
  getColors: () => ["blue", "yellow", "green"],
  sayName: () => "Ajea!"
}
```

解析器中的突变看起来像这样= >

```
/**args is whatever data you passed in (as an object), when you call this type. There are more params by default but we don't need them, thats way we use ```_,```/ 
Mutation: {
   sayHello: (_, args) => {
      return `hello ${args.message}`
   },
   createUser: async (_, args) => {
      try{
        /** async code happens **/
       /** create user with args data into DB, and then return  user*/
      }catch(err){
       console.log(err)
      }
  }
}
```

如果所有这些仍然模糊不清，请不要担心，一旦我们在下一篇文章中创建了真正的查询和变化，就开始变得有意义了。在下一篇文章中，我们将创建它们，并在 Graphql 服务器中进行实际测试。我只是想回顾一下 GraphQL API 的概述。

希望有所帮助，如果不放心让我知道: )