# AR 开发的 Sceneform 去哪了？

> 原文：<https://blog.devgenius.io/where-did-sceneform-for-ar-development-go-8e6b7cae36c2?source=collection_archive---------2----------------------->

![](img/2f1864a620a6e6915bc8386d725c6f7e.png)

马特·诺布尔在 [Unsplash](https://unsplash.com/) 上的照片

我只是想看看以前的 bug 是否被 Sceneform 修复了…而且我听说 Sceneform 是开源的？死了？不太确定，所以我写这个故事是为了告知/讨论 Sceneform 和其他替代方案的状态。

# 我的校园经历

我记得我是 2018 年开始用 Sceneform 的。我最初对它的兴趣是两个事物的交叉，所以可以用维恩图来表达。首先，我有 Android 开发背景，其次，我过去和现在都对增强现实非常感兴趣。Sceneform 似乎合并了这两者，以便为程序员提供开发 AR 应用程序的软件工具。

第一次使用 Sceneform 时，它非常棒，因为它使用了我熟悉的 Android Studio 和 Java。我开始学习 sfa 和 sfb 格式，以及 Android Studio 中的 3D 模型查看器，它是 Sceneform 的一部分。

# 使用 Sceneform 开发时的一些问题

当我使用 Sceneform 时，我确实开始注意到一些奇怪的错误，有些(据我所知)是因为无法将 3D 模型从 FBX 转换到 sfa，然后再转换到 sfb。我对此进行了研究，因为很明显我会假设有一个解决方案，然而当时 Sceneform 最初的失败是缺乏使用它开发的程序员的文档。这意味着要花无数的时间去理解一些事情，比如为什么 Sceneform 的一个核心特性不能工作。这往往是版本问题，所以我会尝试不同版本的 Sceneform，最终让它正常工作。到这个时候，我已经很累了，因为我研究并试图找到核心功能错误的解决办法，而不是花时间开发我的应用程序想法。

大约一年后，我再次访问 Sceneform，发现在将 3D 模型从 sfa 转换到 sfb 时有一个错误。这是第一步，这样 3D 模型就可以在 AR 世界中互动。没有这一点，在我看来是毫无意义的，因为这是让你的 AR 应用变得生动并开始使用 Sceneform 开发 AR 应用的必要条件。

# Sceneform 的现状

正如我在开始时所写的，今天早些时候我看了 Sceneform 的最新消息，希望看到那些错误已经被修复，并且我了解到 Sceneform 已经开源。在阅读了一些 reddit 帖子后，一些人说他们使用了开源版本的 Sceneform，并不认为它有那么好……或者实际上他们包含了一个表情符号来表达同样的意思。其他一些帖子暗示它已经死了，因为它不再被支持。

> 谷歌的一些从事 AR 工作的软件工程师证实，谷歌不再支持 Sceneform，并且已经开源。如果你想继续使用它，你可以，但是如果你想开始一个新项目，**建议使用 ARCore via Unity 而不是**。对 ARCore SDK 的开发和支持仍然存在，这很好！

# 谷歌 AR 开发的未来

![](img/3e50cd2fffcafb871b85605e8ac85cf5.png)

由[弗洛里安·奥利佛](https://unsplash.com/@florianolv)在 [Unsplash](https://unsplash.com/) 上拍摄的照片

很遗憾，Sceneform 的支持没有继续下去，因为我认为它对喜欢主要编码而不是 Unity 方法的开发人员来说是很棒的，然而 Unity 很棒，但我更熟悉纯编码而不是拖放方法+脚本。**另一种选择是基于物理的渲染，在 Android 的 Filament 中:**[https://google.github.io/filament/Filament.md.html](https://google.github.io/filament/Filament.md.html)

希望那些读过这篇文章的人已经被告知 Sceneform 不再被支持了…或者至少从开源的角度来看，根据 reddit 上的一篇帖子，这看起来并不令人惊讶。如果您最近一直在使用 Sceneform 进行开发，请随时留下您对它的体验和想法的评论。

关于这方面的更多信息，这篇 StackOverflow 帖子非常有趣:[https://stack overflow . com/questions/62453399/Google-scene form-is-it-deprecated-any-replacement/62694454 # 62694454](https://stackoverflow.com/questions/62453399/google-sceneform-is-it-deprecated-any-replacement/62694454#62694454)

# 跟着我看 https://www.tiktok.com/@theinspiringprogrammer?抖音 lang=en⏰

***如果你喜欢这个内容，请随时加入我的邮件列表:***[https://www.subscribepage.com/x9b5l0](https://www.subscribepage.com/x9b5l0)

谢谢！