# 让我们用 SWR 简化 React 项目中的数据获取逻辑

> 原文：<https://blog.devgenius.io/lets-simplify-the-logic-of-data-fetching-in-your-react-project-with-swr-155c00dd54bf?source=collection_archive---------7----------------------->

![](img/79e458362f46493429676f978f9bb5be.png)

资料来源:vercel

我们知道 React 正在带来一个快速的用户界面，而无需做太多工作来专门优化性能。但是，仍然有一些方法可以让它变得更快。当我在谷歌代码之夏[项目](https://youtu.be/ODHz5-kjR3U)工作时，我从一位同事那里了解到了 SWR。如果你对我的 GSoC 项目感兴趣，可以参考这篇[文章](https://anjulashanaka.medium.com/gsoc-2022-rebuild-openmrs-cohort-builder-final-evaluation-64b093b68a61)。在这篇文章中，让我们简要了解一下 SWR。

## 什么是 SWR？

> “SWR”这个名字来源于`stale-while-revalidate`，一种由 [HTTP RFC 5861](https://tools.ietf.org/html/rfc5861) 推广的 HTTP 缓存失效策略。SWR 是一种策略，首先从缓存返回数据(陈旧)，然后发送获取请求(重新验证)，最后是最新的数据。

react 的工作原理基本上是使用 [React 钩子](https://reactjs.org/docs/hooks-intro.html)。换句话说，SWR 为远程数据获取提供了 React 钩子。

SWR 是由 [Next.js](https://nextjs.org/) 背后的同一个团队创造的。

## 但是为什么是 SWR 呢？

有了 SWR，组件将不断自动获得一系列数据更新。由于这个原因，用户界面将总是快速和反应性的。

SWR 帮助您简化项目中的数据获取逻辑，并且具有所有这些开箱即用的惊人特性:

*   **快速**、**轻量级、**和**可重用**数据提取
*   内置**缓存**并请求重复数据删除
*   **实时**体验
*   SSR / ISR / SSG 支持
*   打字稿准备好了
*   反应自然

SWR 涵盖了速度、正确性和稳定性的所有方面，帮助您建立更好的体验:

*   快速页面导航
*   间隔轮询
*   数据依赖性
*   焦点的重新验证
*   网络恢复的再验证
*   局部变异(乐观用户界面)
*   智能错误重试
*   分页和滚动位置恢复
*   反应悬念

还有很多[更多的](https://swr.vercel.app/docs/getting-started)。

## 入门指南

通常，您需要使用 [npm](https://www.npmjs.com/) 或[纱线](https://yarnpkg.com/)安装 SWR。你可以跑

```
yarn add swr
```

或使用 npm:

```
npm install swr
```

在您的 React 目录中。

现在让我们了解如何使用 SWR。为此，我使用了我自己使用的代码片段。获取一些位置。

在上面的例子中，`useSWR`钩子接受一个`key`字符串和一个`fetcher`函数。`key`是数据的唯一标识符(通常是 API URL，在这种情况下是`/ws/rest/v1/location`)，并将被传递给`fetcher`。`fetcher`可以是任何返回数据的异步函数，可以使用原生 fetch 或者像 [Axios](https://www.npmjs.com/package/axios) 这样的工具。我曾经[从](https://openmrs.org/) [openMRS ESM 框架](https://www.npmjs.com/package/@openmrs/esm-framework)获取 openMRS 。

钩子根据请求的状态返回 3 个值:`isLoading`、`data`和`error`。

现在，您可以像这样使用 react 文件中的这个钩子，

基于你的使用有不同的钩子，例如我们可以使用[useswrimutable](https://swr.vercel.app/docs/revalidation#disable-automatic-revalidations)用于不改变的数据。这个请求肯定只是从缓存中加载，根本不会影响网络。

我喜欢的另一个特性是**焦点重新验证**特性。

当您重新聚焦页面或在选项卡之间切换时，SWR 会自动重新验证数据。这对于立即同步到最新状态非常有用。这有助于在陈旧的移动标签或进入睡眠状态的笔记本电脑等场景中刷新数据。

SWR API 为您提供了启用/禁用上述功能的选项。您可以访问[选项](https://swr.vercel.app/docs/options)页面了解更多详情。

就这样，我们来到了本文的结尾。我们在这篇文章中瞥了 SWR 一眼，你可以为我查阅文档中的[细节](https://swr.vercel.app/docs/getting-started)解释。别忘了看看我的其他文章。下次再见了。在那之前保持安全！✌️

想要联网吗？
Linkedin:[https://www.linkedin.com/in/anjula-sack/](https://www.linkedin.com/in/anjula-sack/)脸书:[https://www.facebook.com/anjula.shanaka/](https://www.facebook.com/anjula.shanaka/)
Github:[https://github.com/anjula-sack](https://github.com/anjula-sack)

# 参考资料:

[1].开始使用。[2022]SWR[在线]。发布时间:[https://swr.vercel.app/docs/getting-started](https://swr.vercel.app/docs/getting-started)【2022 年 11 月 6 日发布】