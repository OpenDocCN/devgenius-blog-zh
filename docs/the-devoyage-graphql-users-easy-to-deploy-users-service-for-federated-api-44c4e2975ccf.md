# ` the-devoyage/graphql-users` —联邦 API 的简易用户服务

> 原文：<https://blog.devgenius.io/the-devoyage-graphql-users-easy-to-deploy-users-service-for-federated-api-44c4e2975ccf?source=collection_archive---------8----------------------->

大家好！感谢您的加入！

今天，我想分享一个知识库，您可以使用它来扩展下一个(或现有的！)您构建的 API。特别是通过向您的平台添加用户和用户成员。

用`graphql-users`很简单。让我向您展示这个存储库，您可以使用它来为您的 API 启用这些特性。

![](img/ec83945a73a6b6edc73c41e3dd339860.png)

# `GraphQL Users` API

将`the-devoyage/graphql-users`库与您现有的 API 一起部署，以便在您的平台中启用用户和成员特性。让我解释一下它是如何工作的。

## 最底层的

这是这个用户和成员微服务的高级概述。

1.  有人使用您现有的身份验证系统登录，并获得一个 ID。
2.  将帐户 id 和帐户电子邮件安全地传递给这个新用户服务，以便用户“登录”。
3.  `graphql-users`服务返回一个代表用户和帐户的 JWT(签名的 JSON Web 令牌)。如您所愿，使用 JWT 验证请求！

就是这样！用户登录并与帐户相关联，并且用户现在能够与该 API 提供的其他功能交互，例如:

*   用户可以向任何人发送和撤销成员资格，允许其他人管理他们的帐户。
*   用户可以接受或拒绝管理其他用户帐户的邀请。
*   用户可以切换到邀请的帐户。
*   角色的范围是帐户，允许用户在帐户之间维护不同的角色和权限。

这是一个灵活的系统，允许许多可能的用例。

想了解更多的功能和更少的技术？

[](https://thedevoyage.gumroad.com/p/bring-a-website-to-life-for-your-business-idea) [## 向您的 Web 应用程序添加用户和成员

### 欢迎来到 Devoayge，一个提供 web 开发产品和服务的地方！今天，我想分享一个代码起源…

thedevoyage.gumroad.com](https://thedevoyage.gumroad.com/p/bring-a-website-to-life-for-your-business-idea) 

# 入门指南

使用这个存储库就像将它与任何现有的 API 一起旋转一样简单——只需要一些配置。

## 下载源代码

这是一个开源项目，有两个选项可以满足您的需求。

**GNU GPL 许可**

对于免费和开源项目，请查看资源库。

[](https://github.com/The-Devoyage/graphql-users) [## GitHub-The-devo yage/graph QL-users:即装即用的用户微服务…

### 用户微服务是现成的用户身份验证和数据存储服务，也可以用作…

github.com](https://github.com/The-Devoyage/graphql-users) 

**麻省理工学院许可证**

如果你需要更多的灵活性来私下使用它，你可以在这里购买 MIT 许可下的最新版本！

[](https://thedevoyage.gumroad.com/l/graphql-users) [## GraphQL 用户-用户和成员 API

### GraphQL 用户 GraphQL 用户服务是 Devoyage 的 Code Genesis 产品之一——预构建的、功能齐全的……

thedevoyage.gumroad.com](https://thedevoyage.gumroad.com/l/graphql-users) 

## 环境变量

调整环境变量以适应您的项目。

```
NODE_ENV=development
BACKEND_PORT=5002
MONGO_URI=mongodb://sun:sun@localhost:27017/sun
JWT_ENCRYPTION_KEY=abcdefg
MAILER_URI=[http://mailer:5008/send](http://mailer:5008/send)
```

## 旋转起来

您可以使用 npm 来启动它，或者使用提供的`Dockerfile`来启动服务。

```
npm run dev
```

## 告诉网关

最后，确保告诉网关新用户服务的位置。

# 它是如何工作的

现在您已经将服务安装在现有 API 的旁边，您可以立即开始使用它。

## 帐户 ID +电子邮件

如上所述，这个微服务被设计成紧挨着你现有的 API 使用。在您的用户通过身份验证后，保留相关的“帐户”id。它可以是任何唯一的 Mongo ObjectId 来表示帐户。

如果您没有生成唯一帐户 id 的认证系统，请查看 [graphql-accounts！](https://github.com/The-Devoyage/graphql-accounts)

## 登录用户

是时候第一次使用新用户服务了。

要将帐户 id 和电子邮件传递给用户服务，您需要将它们附加到一个`context`头。建议在验证帐户后，从网关服务等服务中执行此操作。

```
JSON.stringify({
  auth: {
    account: { _id: OBJECT_ID; email: ACCOUNT_EMAIL },
    isAuth: boolean,
  }
});
```

## JWT 代币

登录操作会返回一个签名的 JWT 令牌，允许您将帐户与用户相关联。您可以通过环境变量向`graphql-users`服务提供加密密钥，允许您在现有的 API 中验证 JWT。

## 其余的

是的，还有很多——用户成员资格、邀请、角色……查看存储库的自述文件以了解更多信息。

# 仓库

嘿，谢谢你检查这个用户服务！目的是您可以直接从存储库中使用它作为您的下一个用户管理平台——或者使用它作为您的下一个 API 的定制修改的起点。

[查看存储库](https://github.com/The-Devoyage/graphql-users)——或者— [展示你的爱](https://thedevoyage.gumroad.com/l/graphql-users)如果你发现它有帮助/结束使用它！

好了，这就是今天的全部时间，但是请继续关注关于这个库和其他项目的更深入的帖子，它们可以帮助您启动您的下一个开发周期，

谢谢你，尼克—