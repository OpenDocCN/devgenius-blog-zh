# 下一个 GraphQL API 的三个入门库

> 原文：<https://blog.devgenius.io/three-starter-repositories-for-your-next-graphql-api-234c8be43078?source=collection_archive---------10----------------------->

大家好！感谢您的加入。

今天，我想分享三个入门库，以启动您的下一个 graph QL API——从身份验证到用户成员资格，这些开源库旨在让您轻松启动并运行您的项目。

![](img/410fee0c29968d193b9fdd77cac417c9.png)

# 最底层的

当我开始新项目时，我想使启动 API 变得容易，所以我决定将项目的各个部分分支到适合一般情况的可重用存储库中。

这个想法是，每项服务都可以通过一些配置来启动，并在生产中开箱即用——或者作为任何定制项目的起点。

## 技术堆栈

*   GraphQL
*   阿波罗联合会
*   表达
*   结节
*   以打字打的文件
*   Mongo 数据库

# GraphQL 网关

服务是一个全功能的 Apollo 网关，可以作为下一个 API 的入口。它预构建了一些现代行业标准技术，随着项目的发展，您的 API 很可能需要这些技术！

![](img/341f7847622939e17364d94ec9a69778.png)

## 文件上传路由和服务

将文件上传到联邦 GraphQL API 时，您很可能需要将文件发送到处理文件上传和服务的服务，但是您不应该在联邦 API 中将文件上传服务器直接暴露给公共互联网。

默认情况下，该服务可以从文件上传服务向联邦服务和代理文件服务发送文件。无需向互联网公开文件上传服务，您可以从网关发送和接收文件。

## 内置和自动 JWT 授权

只需将`Authorization`报头中的承载令牌传递给网关，网关就会向所有子图提供认证状态和有效负载(来自解码后的令牌)。

## 全球背景

如上所述，网关有能力向外部子图提供数据——我称之为全局上下文。令牌有效负载会自动添加到全局上下文，以及您想要提供给外部子图的任何自定义数据。

## 仓库

[](https://github.com/The-Devoyage/graphql-gateway) [## GitHub-The-devo yage/graph QL-gateway:一个阿波罗网关，准备启动。支持超图…

### 一个易于启动和定制的 Apollo 网关，带有预建的授权检查和文件上传/服务…

github.com](https://github.com/The-Devoyage/graphql-gateway) 

让我知道你的想法！很多时间已经投入到这些项目中，如果你发现自己有一些额外的美元，你可以分享一些爱，并获得麻省理工学院许可版本的服务，如你所愿，在这里！

[](https://thedevoyage.gumroad.com/l/graphql-gateway) [## GraphQL 网关-安全地启动您的 API

### GraphQL GatewayThe Gateway 是 Devoyage 的 Code Genesis 产品之一，用于启动和运行您的企业网站…

thedevoyage.gumroad.com](https://thedevoyage.gumroad.com/l/graphql-gateway) 

# GraphQL 帐户

`@the-devoyage/graphql-accounts`库可以让你快速添加认证(相对于授权，授权由上面的网关处理)到任何 API。

![](img/ce399cb2b778ba8351f8a8160ca01a7b.png)

## 注册、登录和基础知识

开箱即用的服务有解析器来处理每个帐户系统需要的基本内容。注册一个帐户，双因素电子邮件验证，密码重置，只是这项服务预建的解决方案的一部分。

## 电子邮件网络挂钩

事件将触发 web 挂钩，以便在发生重要事件时，例如密码重置通知或验证码时，您可以向用户发送自定义消息。

## 仓库

[](https://github.com/The-Devoyage/graphql-accounts) [## GitHub-The-devo yage/graph QL-accounts:一个现成的帐户服务，支持基于密码的…

### 一个现成的帐户服务，支持使用 JSON Web 令牌进行基于密码的身份验证。按原样使用或作为…

github.com](https://github.com/The-Devoyage/graphql-accounts) 

如果你想下载一个麻省理工学院的许可版本或发送一些爱，请查看下面的 Gumroad 链接。

[](https://thedevoyage.gumroad.com/l/graphql-accounts) [## 帐户-注册和登录

### GraphQL AccountsThe GraphQL Accounts 服务是 Devoyage 的 Code Genesis 产品之一——预构建、完全…

thedevoyage.gumroad.com](https://thedevoyage.gumroad.com/l/graphql-accounts) 

# GraphQL 用户

`@the-devoyage/graphql-users`服务被设计为与上面的账户服务(或者任何真正的联合账户服务)一起工作！).

![](img/ec83945a73a6b6edc73c41e3dd339860.png)

## 保存用户信息

虽然帐户系统非常适合处理鉴定，但用户服务可以帮助存储访问您服务的用户的更多数据，包括姓名、联系信息、地址等。

## 用户成员资格

用户可以邀请其他人使用用户会员系统管理他们的帐户。邀请用户可以控制谁可以访问他们的帐户以及被邀请用户在帐户中的访问程度。

## 仓库

[](https://github.com/The-Devoyage/graphql-users) [## GitHub-The-devo yage/graph QL-users:即装即用的用户微服务…

### 用户微服务是现成的用户身份验证和数据存储服务，也可以用作…

github.com](https://github.com/The-Devoyage/graphql-users) 

如果你喜欢，你可以购买下面的 MIT 授权版本。

[](https://thedevoyage.gumroad.com/l/graphql-users) [## GraphQL 用户-用户和成员 API

### GraphQL 用户 GraphQL 用户服务是 Devoyage 的 Code Genesis 产品之一——预构建的、功能齐全的……

thedevoyage.gumroad.com](https://thedevoyage.gumroad.com/l/graphql-users) 

# 更多存储库

哦，还有更多！如果你喜欢它的发展方向，请不要离开，因为接下来我将分享另外两个库来启动你的下一个 API，包括一个文件上传 API 和一个自动电子邮件 API。

如果你想直接进去，去看看这家店—[https://app.gumroad.com/thedevoyage](https://app.gumroad.com/thedevoyage)

一如既往的感谢！下次见！