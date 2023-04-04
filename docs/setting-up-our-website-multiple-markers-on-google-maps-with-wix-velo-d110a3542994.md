# 设置我们的网站——使用 Wix Velo 在谷歌地图上设置多个标记

> 原文：<https://blog.devgenius.io/setting-up-our-website-multiple-markers-on-google-maps-with-wix-velo-d110a3542994?source=collection_archive---------11----------------------->

## Wix Velo 历险记

![](img/f718c66f27deb02356f7849c3c81440c.png)

对于企业和用户来说，与地图数据的集成从未像现在这样重要。随着我们的系统变得越来越互联，信息在位置方面的语境化为企业提供了难以置信的价值。

整合的一种方法是在地图上显示位置。这可能是静态位置，如您的业务运营网站，也可能是您的弹出式商店的位置。对于这些类型的应用程序，Wix 有一个很棒的元素，可以直接拖放到您的网站中。这里是他们优秀教程的链接。

一个复杂得多的实现是在地图上添加动态和变化的位置。这对于动态更新您的交付驱动程序的位置非常有用，或者在我的情况下，显示安全封条的审计跟踪。它涉及一系列的编码工作、添加 HTML 元素以及连接到数据库集合。

Wix 提供的[教程](https://www.wix.com/velo/forum/coding-with-velo/example-multiple-markers-google-maps)很不错，但是，我觉得一个深入的、循序渐进的教程会增加很多价值。这就是了。

享受❤️

# 我们这个系列的目标

我们这个系列的目标是建立一个网站，将多个动态地图数据点的显示完全整合到一个[谷歌地图](https://www.google.com/maps)显示中。我们希望能够更新一个位置，并让它几乎立即显示在我们的网站上。如果你想了解为什么这很重要，这里是我的介绍。

**工作示例:**我们将使用的示例显示在[这里](https://www.creativeappnologies.com/multi-location-maps)。

**Github / Code Repo:** 对于觉得有用的人来说，这里的[是公开的 Github Repo](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo)。

在这一过程中，我将分享一些关于安全性考虑的技巧，以及一些有帮助的观察，希望能为您节省大量的故障排除时间。每集的完整列表可以在每集的底部找到。

一如既往，希望这些内容有所帮助。当你花时间为一集鼓掌、订阅我的故事或使用[我的推荐链接](https://appnologyjames.medium.com/membership)注册 Medium 时，这对我意味着绝对的世界。它让我知道我的故事正在帮助你❤️.

尽情享受吧，随时在 twitter 上给我发个 [DM，在](https://twitter.com/blockchainvalu1) [LinkedIn](https://www.linkedin.com/in/jameshinton84/) / [Github](https://github.com/jimtin) 上发表想法和评论😃

# 在这一集中

在这一集里，我们将讲述项目的初始设置。如果您有使用 Wix 编辑器的经验，希望这非常简单。这一部分不需要任何代码，如果您已经设置好了，可以直接跳过它。

我们将涵盖以下内容:

1.  设计页面
2.  设置我们的表单来收集位置
3.  用正确的权限设置我们的数据库集合

我们开始吧！

# 设计页面

正如你在[演示](https://www.creativeappnologies.com/multi-location-maps)中看到的，我们的页面设计是一个简单的标题、地图和 Wix 表单。所有这些元素都可以从 Wix 编辑器中添加。

首先，使用 Wix 编辑器设置以下内容:

1.  顶部的文本框使用你喜欢的标题字体。对于文本框，输入对您有意义的标题。
2.  一个空的 HTML 框架。我们将在未来的一集中对此进行补充。给 HTML 元素标上一个有意义的名字，并写下来。我把我的叫做`googleMapsFrame`，我们将在第四集使用它
3.  Wix 表格。我们想要的字段是(至少):纬度和经度，这两个都是十进制数字，因为我们使用十进制度作为输入，以及位置的标题。我们还添加了一个 ReCAPTCHA(稍后会详细介绍)，一个隐私政策的勾选框和一个提交按钮，我们称之为“添加位置”。写下你的表单的名字——我的表单被创造性地命名为`wixForm1`——因为我们将在第 5 集使用它

## 关于安全性的说明

在我们开始之前，请注意一些安全性问题。

1.  小心 HTML 元素。 HTML 元素(也称为内嵌框架或 iframes)是一种 HTML 元素，它在文档中加载另一个 HTML 页面。实际上，你是在你现有的网页上添加一个新的网页，它们被广泛用于网络跟踪器之类的东西。有趣的是，HTML 元素也可以用来加载网页中的恶意代码，获取您以前的良性网页，使其变得邪恶。请阅读[的这篇](https://www.techtarget.com/whatis/definition/IFrame-Inline-Frame#:~:text=An%20inline%20frame%20(iframe)%20is,web%20analytics%20and%20interactive%20content.)文章和[的这篇](https://ostraining.com/blog/webdesign/against-using-iframes/)文章，快速了解相关情况。实际上，这意味着我们需要为我们的设置采取一些降低风险的措施，我将在本系列中详细阐述。如果你需要为你自己的设置进一步讨论这个，给我一个 DM，我们可以聊天。
2.  **限制文本框输入。明确你允许在你的文本框中输入什么是最好的做法。这限制了恶意或邪恶的代码被用来对付你的能力。这也提高了我们网站和代码的效率。对于那些程序员来说，它允许提交给数据库的对象从一开始就被格式化成我们需要的类型。对于我们这里的简单站点，请确保应用了以下限制:将纬度和经度设置为数字框，允许十进制数字。对于位置标题，将其设为文本框，并将字符数限制在 20 个以内。**
3.  **添加 ReCAPTCHA。Wix 表单具有添加 ReCAPTCHA 的内置功能。这将限制某人向表单发送无意义条目的能力，因为它会过滤掉非人类实体。这是一个低成本、高收益的选择，所以我们将其添加到表单中。**
4.  **隐私政策。**一般来说，每当我使用面向互联网的表单时，我都会添加隐私政策。这是一个很好的实践，让你清楚你在用你可能收集的任何数据做什么。

# 设置数据库集合

设计好我们的页面后，是时候设置数据库集合了。对于我们的特定示例，我们希望浏览该页面的任何人都可以访问这些数据。

您的具体实现可能会有所不同，在实现之前审查这一点是**始终**的最佳实践。当您的代码由于访问错误而停止工作时，它将节省您浪费的时间，并且如果私人数据由于不正确的权限设置而暴露，它将节省您的大量时间。

在 Wix 中，这就像转到集合并更改访问一样简单，所以现在就这么做。

当你在那里时，记下以下事项。这些名称和 ID 区分大小写:

1.  集合名称
2.  集合 ID
3.  纬度、经度和位置标题的字段标题
4.  纬度、经度和位置标题的字段 ID

对于我的，我没有做很多配置，所以我得到了一些非常通用的。以下是我的:

1.  **收藏名称:**提交一个有趣的地点
2.  **收款 ID:** 支付表单 07
3.  **字段标题:**经纬度和位置标题
4.  **字段 id:**数字字段，数字字段 2，短答案字段

完成后，点击 Wix 编辑器上的“发布”。

# 添加一些位置

现在是我们添加一些位置的时候了。多刺激啊！

仔细检查并添加至少 10 个位置。这样做时请注意，以便表单按预期工作。

如果你正在寻找一些灵感，[下面是我从](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo/blob/main/starter-locations)开始的 10 个。它们都在澳大利亚美丽的城市悉尼周围，那是我曾经生活过的地方。

# 检查您的数据库集合

最后一步是检查您的数据库集合。确保表单中的条目已经进入集合，并且至少有 10 个条目。

# 结束第一集

第一集基本就这样了。我们实现了以下目标:

1.  设计页面
2.  设置我们的表单来收集位置
3.  用正确的权限设置我们的数据库集合

随着我们的网站外观和感觉的完成，我们准备开始展示我们的位置。我等不及了！！！

## 剧集列表:

这一系列剧集的完整列表链接如下，帮助你快速浏览❤

1.  [建立我们的网站](https://appnologyjames.medium.com/setting-up-our-website-multiple-markers-on-google-maps-with-wix-velo-d110a3542994)
2.  [查询位置集合并计算中点](https://appnologyjames.medium.com/querying-location-collection-and-calculating-midpoint-multiple-markers-on-google-maps-with-wix-d93e3389a12a)
3.  [显示地图&标记](https://appnologyjames.medium.com/displaying-map-marker-multiple-markers-on-google-maps-with-wix-velo-7a9f2209fc7a)
4.  [HTML 元素&发送数据](https://appnologyjames.medium.com/html-elements-and-sending-data-multiple-markers-on-google-maps-with-wix-velo-2f4e8a6f3336)
5.  [自动更新和调整地图大小](https://appnologyjames.medium.com/auto-updating-and-resizing-map-multiple-markers-on-google-maps-with-wix-velo-874b68c77483)