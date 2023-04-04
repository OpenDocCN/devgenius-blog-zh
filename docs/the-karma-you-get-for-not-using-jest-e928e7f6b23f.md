# 你不使用 Jest 的报应

> 原文：<https://blog.devgenius.io/the-karma-you-get-for-not-using-jest-e928e7f6b23f?source=collection_archive---------7----------------------->

在 Karma 上使用 Jest 测试框架的好处

![](img/a5f06797ae353db3ea709477455ea3ff.png)

Jest 徽标

## 什么是玩笑？

Jest 是由脸书设计的 JavaScript 测试框架，旨在简化 JavaScript 单元测试的执行。它支持多框架，可以和 Angular、React、Vue.js、Node 等一起使用。

本文的重点将放在角度上。

## 什么是因果报应？

我相信和 Angular 合作过的人都知道什么是因果报应。刷新一下，Karma 是最初 AngularJS 团队创建的单元测试框架，在你安装 Angular 的时候就预装了。

## 为什么要用笑话代替因果报应呢？

根据我使用 Karma 的经验，我发现它非常慢，因为启动和加载 web 浏览器然后执行每个单元测试需要时间。

使用 Jest 和 Karma 的好处是:

*   更快的单元测试:因为 Jest 是一个无浏览器的测试框架，所以运行单元测试需要更少的时间。比较测试执行时间，我发现 Jest 运行单元测试的速度是 Karma 的 2 到 3 倍(T2)。随着您的项目变得越来越大，测试用例数量激增，这可能会非常激烈。
*   **简单配置** : Jest 被宣传为开箱即用的框架。只需下载并运行它。虽然对于我的项目，我必须进行配置(因为我正在开发的应用程序在结构上相对复杂)，但对于大多数项目来说，它在下载后可以完美运行。
*   **更简单的 CI/CD 集成**:尽管 Karma 支持多种产品的 CI/CD，但它需要一些配置，而且它并不支持所有的 CI/CD 产品。不可能在 Bamboo (Atlassian CI/CD 服务器)上使用 Karma。对 Jest 来说幸运的是，因为它只是在命令行中运行，所以很容易与每个 CI/CD 管道集成。
*   Jest 可以很容易地与 Jasmine 一起使用:当我将 Karma 改为 Jest 时，我担心我必须改变我编写单元测试的方式来适应这种转换。谢天谢地，我所有的单元测试都运行得很完美。

## 如何从因果报应转变为笑话

在命令行中运行以下命令，将 Karma 替换为 Jest:

```
> npm install jest @types/jest --save-dev
> ng add @briebug/jest-schematics
```

**注意:**如果您的项目中安装了不是来自 node_modules 的库。您必须在您的 *packages.json* 文件中添加以下内容:

```
"jest": {
    "moduleDirectories": [
        "node_modules",
        "lib"
    ]
}
```

**跑笑话**

现在，您可以运行以下命令来运行具有代码覆盖率的单元测试:

```
> npm test -- --coverage
```

## 外卖食品

这样做的目的是为开发人员腾出更多的时间。如果您从 Karma 过渡到 Jest，它将在容易集成和单元测试执行速度方面带来巨大的回报。这使您能够继续提供优秀和高效的产品！

希望这有所帮助！