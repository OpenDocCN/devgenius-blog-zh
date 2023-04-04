# Expo 46 —新增内容及其对 React 本地开发的重要性

> 原文：<https://blog.devgenius.io/expo-46-whats-new-and-why-it-matters-for-react-native-development-d086549581d4?source=collection_archive---------7----------------------->

[Expo](http://expo.dev) 是一个跨平台开发 [React 原生](http://reactnative.dev)应用的伟大工具——随着 Expo 46 的发布，它会变得越来越好。在本帖中，我们将深入探讨第 46 届世博会的新特点，更重要的是，为什么它们如此重要。

![](img/8f6f4c693acae6540895504af1510f14.png)

鸣谢:GitHub 上的[博览会](https://github.com/expo/expo)

我承认我写这篇文章有点晚了，因为 Expo 46 是在 8 月发布的。我决定现在写这篇文章，因为我以前没有升级过(稍后会详细介绍)，只是在一个新项目中尝试了一下。世博会走过了漫长的道路——让我们看看它是如何走过来的！

# 怎么样

我们将了解 46 届世博会的一些主要新功能，但还有更多！如需完整的变更列表，请参见关于发布 Expo 46 的[博客文章。](https://blog.expo.dev/expo-sdk-46-c2a1655f63f7)

## 支持 **React 原生 Skia**

Skia 是一个高性能的 2D 图形库，在许多谷歌产品中充当图形引擎。如果你想给你的 React 原生应用程序添加任何类型的动画和交互性，Skia 是一个很好的选择。 [React Native Skia](https://shopify.github.io/react-native-skia/) 项目通过简单的 API 为 React Native 带来了 Skia 支持，Expo 46 增加了对 React Native Skia 的开箱即用支持。

用发行说明的话说:

> “这个库提高了你在 React Native 中所能做的事情的上限——它是一个真正的“游戏改变者”，可以用 React 产生高性能的视觉效果”——Expo 博客

![](img/0a9842fdd207b768b30016672bc56a60.png)

信用:[在 GitHub 上反应原生 Skia](https://github.com/Shopify/react-native-skia)

**重要原因:** Skia 的高性能动画引擎确实能够在 React 本地应用中实现新型 2D 动画和交互性。

注意，当我们谈论 React Native 中的动画时，[有许多不同的*种类*，它们确实需要不同的工具](https://javascript.plainenglish.io/3-approaches-to-building-awesome-animations-in-react-native-dd86b6ad7aa6)。例如，对于 UI 元素的过渡和动画，您可能希望使用类似于 [Reanimated](https://docs.swmansion.com/react-native-reanimated/) 的工具。所以不要认为 Skia 是 React 本地应用中所有动画的解决方案。也就是说，对于 2D 互动动画公司来说，Skia 真的是一个游戏改变者！

## **支持 React 原生 FlashList**

如果您曾经在 React Native 中构建过任何类型的组件列表，那么您可能对随之而来的性能和行为缺陷很熟悉。React Native 提供了一个内置的`[FlatList](https://reactnative.dev/docs/flatlist)`组件，它试图通过回收和重新呈现列表项，并添加对页眉、页脚和其他组件行为的标准化支持来解决一些性能和行为问题。

尽管相对于简单的实现有所改进，`FlatList`仍然会在带有渲染缓慢的项目的大型列表上表现出性能问题。幸运的是，对于`FlatList`有一个几乎是现成的替代品，它极大地提高了性能&内存使用:`[FlashList](https://shopify.github.io/flash-list/)`——并且在 Expo 46 中开箱即用！

**为什么重要:**在移动应用程序中使用列表可能会很棘手，并且是性能问题的常见来源，这可能会使应用程序看起来难以使用和不完善。对`[FlashList](https://shopify.github.io/flash-list/)`的支持是提升 Expo 46 中列表呈现性能的一个很好的方式，几乎可以直接使用。

## **支持 React 原生 0.69 和 React 18**

使用 Expo 的一个棘手的方面是，它经常与一些重要依赖项的特定版本相耦合。问题的严重程度可能会有很大的不同，这取决于具体的依赖项和所讨论的版本。随着 Expo 46 的发布，开发者可以使用 React Native 的(几乎)最新和最棒的版本，0.69 版和 React 18 版。

**重要原因:** React Native 0.69 本身就是一件大事，因为它为 React Native 带来了*重大改进*，特别是在性能方面，这得益于“新架构”。我已经在这里写了 React Native 0.69 的[改进。将这些改进应用于世博会应用是性能方面的一大进步。](/whats-new-in-react-native-0-69-how-to-upgrade-and-why-it-matters-59f0841c08fc)

在本文的开头，我提到我没有马上更新到 Expo 46。那是因为我最近做的项目根本没有用 Expo，因为缺乏对 React 原生 0.69 和 React 18 的支持！随着 Expo 46 带来 React Native *大部分是最新的*(现在有 React Native 的 [0.70 版本)，希望从 React Native 获得最新和最大性能优势的开发人员应该能够通过 Expo 再次这样做。](/whats-new-in-react-native-0-70-how-to-upgrade-and-why-it-matters-67aa35d75e37)

# 回顾:第 46 届世博会

[Expo](http://expo.dev) 是 React Native 开发的一个很棒的工具，随着 Expo 46 的发布，它实现了一些很棒的新特性和主要的性能提升。如果你在过去一年左右没有密切关注 React Native 和 Expo，那你就错过了——它们都取得了巨大的进步。现在是给 Expo & React Native 一个尝试的好时机。

要查看新功能的完整列表以及如何升级，请查看世博会博客上的[公告。](https://blog.expo.dev/expo-sdk-46-c2a1655f63f7)

*原载于*[*Blixtdev*](https://blixtdev.com/expo-46-whats-new-and-why-it-matters/)*。*

乔纳森拥有超过 20 年的大型和小型创业公司的工程领导经验。如果你喜欢这篇文章，请考虑加入 Medium 来支持 [*乔纳森和其他成千上万的作者*](https://medium.com/@jonnystartup/membership) *。*