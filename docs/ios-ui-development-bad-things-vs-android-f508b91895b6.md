# iOS UI 开发坏事 vs 安卓

> 原文：<https://blog.devgenius.io/ios-ui-development-bad-things-vs-android-f508b91895b6?source=collection_archive---------22----------------------->

![](img/cb4d8a69ef46079a42cd4195e9479401.png)

卡斯帕·卡米尔·鲁宾在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我开始 iOS 开发的时候有 2 年的 Android 开发经验。我认为在 iOS 中开发 UI 比在 Android 中更容易。在这一点上，我只是看到了 iOS UI 的一些东西，比如通过 segue 连接不同的视图控制器，这产生了 iOS 具有简单的 UI 开发的想法。但是当我开始 iOS 开发的时候，我发现从开发的角度来看，iOS 的 UI 存在一些最糟糕的地方。

让我们来讨论一下 UI 的一些东西，与 Android 相比，这些东西并不好。
Android 设计可以通过拖拽&和 XML 文件来完成。XML 文件设计是复杂 UI 设计的最佳方式。与 Android 相比，iOS 有一个很好的阻力&下降，直到我们做出一个正常的用户界面。iOS 没有 UI 构建那样的 XML 文件。我们只有一个设计选择。对于制作复杂的设计来说，拖拽&是最糟糕的事情。

Android 有传统的边距和填充属性。在 iOS 中可以实现 Margin 但是我从来没有在 iOS 中见过 padding attr。

在开始使用 iOS 之前，我使用过 Android 中的 ScrollView，但我从未在谷歌上搜索过 Android 中的 ScrollView。使用 ScrollView 是小菜一碟。一个开发者不需要担心在 Android 中使用 ScrollView。iOS 开发者花了太多时间使用 ScrollView。如果您将错过一个约束，滚动视图将不起作用。在复杂 UI 中，有时很难找到缺失的约束。

Android 开发人员使用了两个属性 wrap_content 和 match_parent，一切都会好的。Android 给了其中一个 attrs 之后就再也不会问你身高了。我们从不担心这个视角的高度。说到 iOS，每次我们都需要指定视图的高度。否则，ios 会显示红色/警告。每次指定高度都会产生额外的负担。iOS 对某些视图使用固有内容大小，但不是对所有视图。它会自动计算大小。

这个帖子到此为止。以上是我对 iOS 设计问题的个人想法。随意评论。感谢您阅读本文！