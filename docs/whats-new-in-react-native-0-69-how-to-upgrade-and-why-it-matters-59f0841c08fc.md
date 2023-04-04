# React Native 0.69 的新增功能—如何升级以及为什么重要

> 原文：<https://blog.devgenius.io/whats-new-in-react-native-0-69-how-to-upgrade-and-why-it-matters-59f0841c08fc?source=collection_archive---------0----------------------->

![](img/9440ecba6f71d954ec9a90b00f236588.png)

照片由[劳塔罗·安德烈亚尼](https://unsplash.com/@lautaroandreani?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

上周 [React Native 0.69 发布](https://reactnative.dev/blog/2022/06/21/version-069)。它具有许多重要的更新，这些更新有助于大大提高 React 本机应用程序的性能和内存使用。在这篇文章中，我不仅会简要介绍 React Native 0.69 中的新特性，还会简要介绍它的重要性。我也会快速覆盖*如何升级*。

# 最新消息:React 18

React Native 最新版本现在使用 [React 18](https://reactjs.org/blog/2022/03/29/react-v18.html) 。因为 React Native 与 React 内部有些耦合，所以 React Native 版本通常与 React 的特定版本绑定在一起，并且 React Native 的官方版本通常落后于 React 的最新版本。随着 0.69 版本的发布，React Native 现在使用的是 React 的最新和最好的版本。

![](img/d03ac1db07b776f8d0e0c6dfbc2245d2.png)

React 18 —最新 React 原生版本的一部分

## 为什么它很重要

由于两者之间的紧密耦合，React Native 并不总是能够利用 React 的最新版本，有时需要开发人员落后一两个版本。这意味着错过了重要的性能和 API 改进。

有了 React Native 0.69，开发人员可以享受 React 18 的许多新功能，如[增强的悬念功能](https://17.reactjs.org/docs/concurrent-mode-suspense.html)、[并发 React](https://17.reactjs.org/docs/concurrent-mode-intro.html) 、[新挂钩](https://reactjs.org/docs/hooks-reference.html)，一系列错误修复和性能改进，等等。此次更新是确保 React Native 与现代 React 开发同步的关键一步。

# 最新消息:捆绑的爱马仕

你在你的 React Native 项目中使用 Hermes 了吗？Hermes 是一个针对 React Native 优化的 JavaScript 引擎，与每个平台上的默认引擎相比，它的性能更好，占用的内存也更少。像 React 一样，Hermes 与您正在使用的 React Native 的特定版本相关联。从 0.69 开始，Hermes 与 React Native 捆绑在一起，这使得构建更简单，并确保您使用的是正确、兼容的 Hermes 版本。使用爱马仕是*选择加入*，所以确保[按照说明在你的项目](https://reactnative.dev/docs/hermes)中启用它。

![](img/567ed02cd3796d349418f2bb1b24a390.png)

## 为什么它很重要

Hermes 在反应本机性能方面向前迈出了一大步，并有助于启动时间、内存使用和包大小。因为它是一个完全不同的 JavaScript 引擎，所以总是有不兼容的可能。但由于爱马仕已经存在了相当长一段时间，大部分问题都已经解决了。在某些情况下，您可能需要更新其他项目依赖项，以确保与 Hermes 的兼容性。

我已经在几个项目中实现了它，并且对结果非常满意。你应该在打开后彻底测试你的应用程序，只是为了确保一切按预期运行，但我强烈建议你让它从你的 React 本机应用程序中获得最佳性能。

# 新功能:新架构的推出

[新架构](https://reactnative.dev/docs/new-architecture-intro)描述了 React Native 中一系列主要的、正在进行的架构变化，这些变化增强了性能、灵活性、内存使用等等。到目前为止，新架构的采用是自愿加入的，需要手动启用。

新架构有三个主要组件值得注意:两个需要在您的项目中手动启用， *Fabric* 和 *TurboModules，*以及一个运行在引擎盖下支持它们的组件: *JSI。*

JSI 是 JavaScript 本地代码之间的一个新的互操作层。在 JSI 之前，JavaScript 本地代码之间的消息通过异步 JSON 调用在“桥”上发送。JSI 支持 JavaScript 本地代码之间的本地同步通信，消除了对 JSON 编码/解码和异步消息传递的需求；在某些情况下，通过启用结构的共享所有权而不是在桥的两端镜像数据来减少内存需求。简而言之:更快&更好。

*Fabric 是一个*新的渲染引擎，是 React Native 的“影子树”的替代品，直接在 C++中运行。*阴影树*是 React 本地应用的视图层次的本地表示，类似于 React 在 web 上的*阴影 DOM* 。它跟踪层次结构的变化，并用于在屏幕上呈现本地视图。 *Fabric* 增强了 React 本机功能这一关键部分的性能、内存使用和稳定性。

*TurboModules* 是 JavaScript 和本地模块之间互操作性的新系统的名称。React Native 通过 JavaScript 公开本机 API 来实现本机功能。为了公开这些 API，开发人员围绕本机功能构建模块*包装器*，并通过 JavaScript 接口公开这些包装器。TurboModules 通过 JavaScript 规范使用动态的、类型安全的模块代码生成，使这种模块交互更加简单和高效，并且通过 JSI 完成所有这些。

新架构比这里总结的要多得多，所以如果你想知道所有可怕的细节，你可以在这里对新架构进行深入研究。

## 为什么它很重要

简而言之，新的架构意味着更好的性能、响应能力、灵活性和内存使用——为最终用户和开发人员带来全方位的更好体验。

切换到新架构与其说是“是否”的问题，不如说是“何时”的问题。虽然目前是可选的，但新的 React 原生开发专注于新的架构，我们可以预计在未来的某个时候，使用旧的架构将会受到阻碍，如果不是完全不可能的话。

我建议你现在就开始尝试新的架构。与 Hermes 一样，在启用它时，总是有潜在的不兼容性或其他问题，所以一定要彻底测试。

# 如何升级

升级 React Native 有时候有点棘手。*主要的*变化是更新您的`package.json`文件中 React Native 和相关模块的版本，然后运行您通常的包更新命令(通常是`yarn, pod install, etc`)——但这通常是不够的。

除了简单地升级 React 本地库之外，您通常需要进行其他项目和构建更新。为了获得您需要应用的更改的完整列表，请查看 [React 原生升级工具](https://react-native-community.github.io/upgrade-helper/?from=0.68.2&to=0.69.0)。使用该工具，您可以选择 React Native 的当前版本，该工具将向您显示升级 repo 所需的所有差异。

如果你想在这个时候启用新的架构，一定要[检查文档](https://reactnative.dev/docs/new-architecture-intro)——简单地升级到 0.69 不会自动启用新的架构。赫尔墨斯也是如此:如何在你的项目中启用它的说明在这里找到[。](https://reactnative.dev/docs/hermes)

老实说，我在升级我正在开发的应用程序时确实遇到了一些问题。幸运的是，我发现了一个讨论这个问题的 [GitHub 问题](https://github.com/facebook/react-native/issues/33976),并从线程中应用了一个变通方法来让它工作。有一个公开的 PR 来解决这个问题，但它还没有被合并，所以如果你对应用手动工作方法感到不安，你可能想等待一个点的发布。就我个人而言，我将升级放在一个单独的分支中，暂时关注这个问题。

# 重述:反应原生 0.69

React Native 0.69 带来了大量重要的改进和更新，其中性能和内存使用是一个主要主题。因为有些变化是选择加入的，所以您需要自己决定启用哪些变化以及启用的顺序。然而，无论您采取哪种升级途径或策略，这些变化都会给 React 原生平台带来令人兴奋的改进。这是成为 React 原生开发者的大好时机！

*Jonathan 在大大小小的创业公司中拥有超过 20 年的工程领导经验。在业余时间，他在做一个文档自动化平台，*[*Imagepipe.co*](http://imagepipe.co/)