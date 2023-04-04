# 自动更新和调整地图大小——使用 Wix Velo 在谷歌地图上添加多个标记

> 原文：<https://blog.devgenius.io/auto-updating-and-resizing-map-multiple-markers-on-google-maps-with-wix-velo-874b68c77483?source=collection_archive---------12----------------------->

## 与 Wix Velo 一起冒险

![](img/f718c66f27deb02356f7849c3c81440c.png)

欢迎回到我用 Wix Velo 在谷歌地图上显示多个位置系列的最后一集！

我们已经完成了这个系列，从零开始建立了一个网站，我们终于准备好把它带回家了！

## 让我们开始吧！！！

# 我们这个系列的目标

我们这个系列的目标是建立一个网站，将多个动态地图数据点的显示完全集成到一个谷歌地图显示中。我们希望能够更新一个位置，并让它几乎立即显示在我们的网站上。如果你想了解为什么这很重要，这里是我的介绍。

**工作示例:**我们将使用的示例显示在[这里](https://www.creativeappnologies.com/multi-location-maps)。

**Github / Code Repo:** 对于觉得有用的人来说，这里的[是公开的 Github Repo](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo)。

在这一过程中，我将分享一些关于安全性考虑的技巧，以及一些有帮助的观察，希望能为您节省大量的故障排除时间。每集的完整列表可以在每集的底部找到。

一如既往，希望这些内容有所帮助。当你花时间为一集鼓掌、订阅我的故事或使用[我的推荐链接](https://appnologyjames.medium.com/membership)注册 Medium 时，这对我意味着绝对的世界。它让我知道我的故事正在帮助你❤️.

尽情享受，随时在 twitter 上给我发 [DM，在](https://twitter.com/blockchainvalu1)[LinkedIn](https://www.linkedin.com/in/jameshinton84/)/[Github](https://github.com/jimtin)上发表想法和评论😃

# 在这一集中

这一集是最后一集。顶点石。一个所有事情都汇集在一起的地方。我们一起做了大量的工作，现在我们将完成它。

以下是我们将要讨论的内容:

1.  提交表单时触发地图更新
2.  调整地图大小以确保所有位置都在框架内

太棒了。

# 触发地图更新

我们需要两样东西来动态更新我们的 HTML 元素。

1.  **触发。**我们需要一个事件来触发下一个动作的发生。
2.  **数据。**我们需要知道要传递给 HTML 框架的数据。幸运的是，我们在之前的剧集中已经完成了所有这些工作。

当检查我们的触发器时，我们需要考虑一些事情。

1.  确保我们仅在信息成功输入后捕获信息
2.  *确保每次提交新位置时，触发器只触发*一次*(否则，我们会得到一个不断刷新的地图)*
3.  *确保总是能够*输入新位置**

*这意味着我们不能使用标准的触发器，如“页面加载”或“当提交按钮被按下时”。这两种选择都会违背我们的一个考虑。“页面加载”只会在页面第一次被浏览或者我们刷新整个页面时加载地图(这很麻烦)“当提交按钮被按下时”,在新条目被输入之前就有加载我们的位置的风险。虽然我们当然可以设计我们的代码流来绕过这些问题，但这很容易出错，并可能引入更多的错误。*

*幸运的是， [Wix CRM API](https://www.wix.com/velo/reference/wix-crm) 有如下功能:`onWixFormSubmitted`。阅读这个函数的文档，我们会发现如下内容:*“当站点访问者单击* `*WixForms*` *元素上的 submit 按钮并且 Wix 表单被成功提交到服务器时触发的事件。”**

*赢了！*

*这个优秀的小 API 函数符合我们的标准:*

1.  *只有成功提交后才会触发*
2.  **每次成功提交时，它触发一次**
3.  **这是内置在表单中的，所以*总是*可用**

**记住这一点，让我们更新我们的`$w.onReady`函数，以包含一个处理 Wix 表单提交的方法。**

**代码如下:**

**如果您正在阅读本系列教程，您可能会注意到，我们现在已经将 Wix 表单触发连接到检索位置数据的函数。**

**那有多好！**

**如果你现在“预览”你的网站，你应该注意到你已经可以看到这个工作。**

****太酷了！****

# **调整谷歌地图的大小**

**我们的下一个挑战是确保我们的地图总是调整大小，以包括所有的标记。**

**我们可以再次通读我们的文档。[谷歌地图 Javascript API](https://developers.google.com/maps/documentation/javascript/overview) 有一个[很棒的小功能](https://developers.google.com/maps/documentation/javascript/reference/coordinates)叫做`LatLngBounds()`，它是这个目的的救命稻草。**

**它允许我们将一个位置数组传递给函数，然后要求它适应地图的边界以包含所有的位置。几乎正是我们需要的。**

**将它添加到您的 HTML 元素代码中，直接在`locations.forEach(location =>`部分下:**

**在你的编辑代码上点击“更新”,就应该完成了。**

# **非常激动人心的时刻**

**我们已经到了一个非常激动人心的关键时刻！**

**在这一点上，如果你发布你的网站，你应该有一个功能齐全的谷歌地图，它会随着新位置的输入而更新。更好的是，你已经定制了你在地图上使用的图标。**

**恭喜你😃**

**以下是完整的代码文件供参考:**

1.  **[Github 回购](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo)**
2.  **[父页面代码](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo/tree/main)**
3.  **[querylasttenlocations . jsw](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo/blob/main/queryLastTenLocations.jsw)**
4.  **[HTML 元素](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo/blob/main/htmlframe.html)**

# **更进一步**

**从这一点来看，我们肯定可以走得更远。我们可以这样做:**

1.  **使用一些谷歌优秀的定制选项来定制地图本身的外观**
2.  **进一步定制标记，也许可以区分中点和激动人心的位置**

**我相信你能想到更多，我也很想听听！**

**为什么不花点时间把它们写在下面的评论里呢？如果你在 twitter 上给我一个完整代码的截图，我也会很高兴——我会和你一起庆祝的！**

# **结束第五集**

**完成这个系列做得很好！我希望它能帮助你解决你所面临的任何挑战！**

## **剧集列表:**

**这一系列剧集的完整列表链接如下，帮助你快速浏览❤**

1.  **[建立我们的网站](https://appnologyjames.medium.com/setting-up-our-website-multiple-markers-on-google-maps-with-wix-velo-d110a3542994)**
2.  **[查询位置集合并计算中点](https://appnologyjames.medium.com/querying-location-collection-and-calculating-midpoint-multiple-markers-on-google-maps-with-wix-d93e3389a12a)**
3.  **[显示地图&标记](https://appnologyjames.medium.com/displaying-map-marker-multiple-markers-on-google-maps-with-wix-velo-7a9f2209fc7a)**
4.  **[HTML 元素&发送数据](https://appnologyjames.medium.com/html-elements-and-sending-data-multiple-markers-on-google-maps-with-wix-velo-2f4e8a6f3336)**
5.  **[自动更新和调整地图大小](https://appnologyjames.medium.com/auto-updating-and-resizing-map-multiple-markers-on-google-maps-with-wix-velo-874b68c77483)**