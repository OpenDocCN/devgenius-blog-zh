# 如何保护您的 GraphQL API(第 1 部分)

> 原文：<https://blog.devgenius.io/how-to-secure-your-graphql-api-part-1-eb41f66ec07e?source=collection_archive---------6----------------------->

![](img/926329209bb3cbb470fa426a6de54fb9.png)

开发 GraphQL 后端的一个最关键的方面是创建一个**安全层**,无论图形导航从哪里开始，它都会管理数据访问。

GraphQL 促进了连接数据模型的设计，您可以用具有多个入口点的图的形式公开该模型。每个查询允许用户从这些访问之一开始请求图的一部分，通常没有深度限制。

试图在边界保护您的图，就像您在 REST 服务中可能做的那样，不是一个好策略。在每个查询和变异上添加一个安全层，您将结束在不同的解析器中复制相同的访问控制，可能会留下不安全的路径。这将是极其复杂、重复、维护不善和表现不佳的。

一个更好的方法是根据图表来设计 ACL 策略，而不是针对它所公开的每个操作。

# 保护数据，不仅仅是运营

在数据访问层中实现安全策略对于任何后端开发来说都是一个有用的机会，这对于那些希望实现 GraphQL 后端的人来说几乎是必不可少的。

这意味着您需要设计一个更复杂的数据访问层，它不仅关注从不同的数据源加载您需要的内容，还关注检查您的安全身份是否允许这样做。

如果连接实体及其关系的解析由一个库来处理，就像**现代 ORMs** 的情况一样(像 [Prisma](https://www.prisma.io/) 、 [TypeORM](https://typeorm.io/#/) 或 [Typetta](https://twinlogix.github.io/typetta/) ，那么在这个过程中必须定义和应用**安全策略**。

定义对数据的访问权限以及数据定义本身要容易得多，然后认为对数据的每次访问都是自动安全的。回到 GraphQL 上下文，您应该定义谁可以访问图的每个部分，而不管他沿着什么路径到达那个部分。

要实现这一结果，您需要一个强大而灵活的安全模型及其在数据访问库上的实现。

# 基本的安全模型

说到安全性，我们可以定义以下概念:

*   **身份**:这是请求访问数据的主体；它可以是系统的物理或逻辑用户、第三方应用程序、子系统、超级管理员等。
*   **Resource** :这代表了所有需要保护的东西，一个身份对这些东西的访问可以由一组规则决定。具体来说，资源是数据模型中每个实体的单独记录。
*   **Permission** :这是一个逻辑标识符，以唯一文本代码的形式表示一个或多个资源上允许的一组操作。
*   **安全上下文**:这是一组关于身份的信息，用于确定是否被授权执行某项操作。例如，安全上下文可以包含一组权限或一个复杂的数据结构，该数据结构定义了一组限于某些条件的权限。它通常与身份认证同时创建。
*   **安全域**:代表一组资源分组规则，用于限制一个或多个权限的应用。安全域的一些示例可以是“引用用户 U1 的资源集”、“引用租户 T1 的资源集”等。安全域的概念有助于形成安全上下文，在安全上下文中，一个身份对不同的资源组具有不同的权限。
*   **安全策略**:这是一组规则，根据请求访问的身份可用的权限集，确定授权或禁止访问特定资源。

下面是一个示例数据模型，显示了上面定义的所有概念:

```
type User {
 id: ID!
 firstName: String
 lastName: String
 permissions: []
}type UserPermission {
 userId: [ID!]
 permission: Permission!
}enum Permissions {
 VIEW_POSTS
 MANAGE_POSTS
}type Post {
 id: ID!
 userId: ID!
 content: String!
}
```

根据上面的模型，我们可以说每个用户是系统的一个**身份**，其帖子是要保护的**资源**。然后我们有两个**权限**、 *VIEW_POSTS* 和 *MANAGE_POSTS* ，它们可以分配给用户并标识他们可以做什么。

现在让我们假设系统中有两个用户，由以下两个用户配置定义:

```
const mattia : User = {
 id: ‘1’,
 firstName: ‘Mattia’,
 lastName: ‘Minotti’,
 permissions: [
  { permission: ‘MANAGE_POSTS’ }, 
  { permission: ‘VIEW_POSTS’ }
 ]
}const edoardo : User = {
 id: ‘2’,
 firstName: ‘Edoardo’,
 lastName: ‘Barbieri’,
 permissions: [
  { permission: ‘MANAGE_POSTS’, userId: [‘2’]}, 
  { permission: ‘VIEW_POSTS’ }
 ]
}
```

这个配置表明用户 *Mattia* 拥有阅读和管理所有系统帖子的权限，因为他的权限没有限制，而用户 *Edoardo* 可以阅读所有系统帖子，但只能管理他自己制作的帖子。

该示例隐式显示了应用于用户 Edoardo 的 *MANAGE_POSTS* 权限的**安全域**概念。事实上，它被分配给一组资源:id 为 2 的所有用户的帖子。

因此，我们可以想象，对于两个用户中的每一个，相关的**安全上下文**:

```
const mattiaSecurityContext = {
 userId: ‘1’,
 permissions: {
 ‘MANAGE_POSTS’: true,
 ‘VIEW_POSTS’: true
 }
}
const edoardoSecurityContext = {
 userId: ‘2’,
 permissions: {
 ‘MANAGE_POSTS’: [{ userId: ‘2’ }],
 ‘VIEW_POSTS’: true
 }
}
```

安全上下文只不过是身份信息的摘录，用于确定它是否可以访问资源。在这种情况下，每个权限都链接到一个安全域或值 true，这表示所有资源都没有域限制。

安全层的最后一个组件是**安全策略**。您想要保护的资源是 post 实体，因此您必须为该实体定义一个安全策略，该策略包含一组规则，这些规则决定了对它的授权或禁止访问。

```
const postSecurityPolicy = {
 domain: {
  userId: true
 },
 permissions: {
  MANAGE_POSTS: {
   create: true,
   read: true,
   update: true,
   delete: true
  },
  VIEW_POSTS: {
   create: false,
   read: true,
   update: false,
   delete: false
  }
 }
}
```

这样我们就定义了:所有拥有 *MANAGE_POSTS* 权限的用户都可以对其安全域中的实体执行所有 CRUD 操作；所有拥有 *VIEW_POSTS* 权限的用户只能读取其安全域内的实体。

# 实现呢？

设计这种安全模型的下一步是使用现代 GraphQL 技术实现它。

有很多 ORM 库，每一个都有它的优点和缺点，但是很少有一个结构化的安全方法。

在本博客的[下一部分](https://medium.com/p/70cd7e7f04f2)中，我们将探索一个名为 [Typetta](https://twinlogix.github.io/typetta/) 的非常新的 TypeScript ORM 库，它试图通过实现所描述的安全模型来解决上述问题。

请继续关注我们，了解更多有趣的高级编码方法，如果你喜欢这个博客**，请为它鼓掌**。