# HTML 元素和发送数据——使用 Wix Velo 在谷歌地图上使用多个标记

> 原文：<https://blog.devgenius.io/html-elements-and-sending-data-multiple-markers-on-google-maps-with-wix-velo-2f4e8a6f3336?source=collection_archive---------8----------------------->

## Wix Velo 历险记

![](img/f718c66f27deb02356f7849c3c81440c.png)

欢迎回到我的用 Wix Velo 在 Google 地图上显示多个位置的系列文章。我真的很感谢你花时间来看看我的教程，我很乐意在下面的评论中听到你的意见。

# 我们这个系列的目标

我们这个系列的目标是建立一个网站，将多个动态地图数据点的显示完全集成到一个谷歌地图显示中。我们希望能够更新一个位置，并让它几乎立即显示在我们的网站上。如果你想了解为什么这很重要，这里是我的介绍。

**工作示例:**我们将使用的示例显示在[这里](https://www.creativeappnologies.com/multi-location-maps)。

**Github / Code Repo:** 对于觉得有用的人来说，这里的[是公开的 Github Repo](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo)。

在这一过程中，我将分享一些关于安全性考虑的技巧，以及一些有帮助的观察，希望能为您节省大量的故障排除时间。每集的完整列表可以在每集的底部找到。

一如既往，希望这些内容有所帮助。当你花时间为一集鼓掌、订阅我的故事或使用[我的推荐链接](https://appnologyjames.medium.com/membership)注册 Medium 时，这对我意味着绝对的世界。它让我知道我的故事正在帮助你❤️.

尽情享受，随时在 twitter 上给我发 [DM，在](https://twitter.com/blockchainvalu1)[LinkedIn](https://www.linkedin.com/in/jameshinton84/)/[Github](https://github.com/jimtin)上发表想法和评论😃

# 在这一集中

这一集我超级兴奋。我们最终将通过 HTML 组件把我们的动态位置数据连接到我们的谷歌地图。

到本集结束时，你将能够将地图位置放入你的收藏中，并让它几乎立即在你的地图上更新。这是一项强大的功能，将为您和您的客户带来巨大的好处！

以下是我们将要讨论的内容:

1.  设置我们的 HTML 组件来发送和接收数据
2.  设置我们的父页面来发送和接收数据
3.  集成了一些与 Wix 日志平台一起工作的故障排除功能
4.  检索和处理我们的地图数据
5.  快速试用一下——耶！

让我们开始吧。

# 设置 HTML 组件以接收数据

正如我在以前的文章中提到的，HTML 组件实际上是网页中的网页。这是一个奇怪的概念，所以如果它让你的大脑有点弯曲，我完全理解。我也没有真正的概念部分的解决方案，它有点不切实际(不是一个真正的单词)。

不管怎样，这意味着要从 HTML 组件获取信息，我们必须从父页面发送和接收消息。在下一节中，我们对父页面做完全相同的事情。

## 快速警告

正如我在上一期中所讨论的，HTML 组件(iFrames)存在一些问题。之前我提到了一些安全隐患，今天，请注意你的父站点和一个 HTML 组件之间的消息传递经常会有一些问题。

在这一集里，我们将会遇到一些需要注意的事情:

1.  当从 HTML 组件->父页面发送消息时，您需要指定一个目标 URL。懒惰的程序员会简单地使用一个`"*"`名称…但是这样做使得恶意实体有可能拦截消息。安全最佳实践是**总是**指定你的目的地 URL，即使消息没有安全值。相信我，它会让你在未来免去一吨的心痛！
2.  当您设置 HTML 组件来接收消息时，默认行为是接收所有消息。这意味着您可能会收到来自网页上其他 iframe(HTML 组件)的消息。例如，如果你运行谷歌 Chrome 和 MetaMask 加密钱包，你会突然看到自己收到 MetaMask 的消息，检查它是否仍在工作(是的，这本身就是一个安全问题)。在这一集里，我们将根据我们在第 2 集里配置的`target`变量过滤我们的消息。

## HTML 组件消息接收代码

我们的第一站是设置我们的 HTML 组件来发送消息。这非常简单，使用来自 [Wix Velo 教程](https://support.wix.com/en/article/velo-working-with-the-html-iframe-element)的代码的修改版本。请执行以下操作:

1.  导航到您的 HTML 元素并选择“编辑代码”
2.  导航到第二个`<script>`标签
3.  在功能`initMap()`中插入以下内容:

您可以在这里看到，我们根据第 2 集设定的目标立即过滤了传入的消息。

更新您的代码，虽然什么也不会发生，但我们可以为现在收到消息而感到兴奋！

# 设置 HTML 组件以发送数据

我们的消息传递系统的下一个组件是从我们的 HTML 组件->父页面发送消息。正如前面所讨论的，我们总是希望确保我们也清楚发送我们的消息的 URL，因为这防止消息到达非预期的(咳咳，特别是当这是恶意的)接收者。我们还将以此为契机，开始构建我们的故障排除解决方案，稍后将对此进行更广泛的讨论。

## 获取父页面 URL

我们将使用编程方法来获取我们的 URL。这有几个好处。

1.  它将在 Wix 编辑器和我们的现场工作
2.  大规模实施要简单得多
3.  我们可以包括一些错误检查，以确保它确实是一个 iframe

为此，我们实现了以下内容:

1.  在我们的第二个`<script>`标签中，在`window.initMap = initMap;`上方创建一个名为`getParentURL();`的函数
2.  在`script`的顶部创建一个变量来存储 URL: `myURL = "null"`
3.  在此之下，设置 myURL 变量:`myURL = getParentURL();`(您可以很容易地将第 2 行和第 3 行组合在一起，我只是为了演示我的观点而将它们分开)
4.  用以下内容填充`getParentURL()`功能:

按“更新”并继续。

## 创建发送到父功能

指定 URL 后，我们现在将创建页面消息。我们将利用这个机会设置我们的故障排除功能。在刚刚创建的`getParentURL()`函数下实现以下内容:

按“更新”并继续。

# 设置父页面以发送数据

接下来，我们将设置父页面来发送数据。为此，使用您在第 1 集创建的 HTML 元素名称(我的名称是`googleMapsFrame`)。这是在主页面上您的`$w.onReady`函数下要实现的函数

# 设置父页面以接收数据

现在我们将设置我们的父页面来接收来自 HTML 元素的数据。在这部分代码中，我们希望有一种方法来区分以下内容:

1.  一条命令消息，我们称之为`cmd`
2.  一条调试消息，我们称之为`debug`

这允许我们区分指示页面做某事的能力(a `command`)和更新站点进度的能力(T13)。为此，我们将实现几个 switch 语句，以允许发生一些基本的逻辑分支。

首先，我们需要让页面知道如何处理传入的消息。这是在`$w.onReady`函数中完成的，所以更新您的`$w.onReady`函数，如下所示:

接下来，我们需要实现处理消息的函数。在您的`$w.onReady`功能下实现此功能:

如果你仔细观察，你可能会注意到一件令人兴奋的事情发生了。我们包含了一个名为`retrieveLocationData()`的函数，当我们从我们的 HTML 框架接收到一个`cmd`时就会触发这个函数。你可以在第 11 行看到！

这非常令人兴奋，所以让我们现在就实现这个功能吧！

# 检索位置数据

这个函数调用第 2 集的后端代码。这是我们第一次突破动态墙->从我们的集合中检索数据，并将其传输到我们的 live 页面。

由于我们之前所做的工作，我们只需要几行代码:

1.  将这一行放在`$w.onReady`函数之上，导入我们之前创建的函数:`import {getLocationData} from 'backend/queryLastTenLocations'`
2.  在`$w.onReady`函数下面添加这段代码

你注意到这个函数的作用了吗？它检索我们的位置信息，并将其发送到我们的 HTML 框架！

那有多好！我们现在真的在进步！

# 处理 HTML 框架中的接收数据

下一步是在 HTML 框架中处理我们收到的数据。如果你想遵循数据格式，[这里有链接](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo/blob/main/locationFormat.json)。

实际上，我们需要在我们的`initMap`职能中采取以下步骤:

1.  处理一个“无位置”长度，这将在 HTML 元素初始化时发生。如果发生这种情况，向父页面发送一个命令来更新位置
2.  当接收到事件数据时，提取`midpoint`
3.  从`midpoint`中取出`midpointLocation`
4.  用提取的位置更新`locations`数组
5.  初始化谷歌地图
6.  画出我们的中点标记
7.  对于每个位置，提取位置
8.  标出每个位置

这是一个需要经历的过程😅，所以下面是要更新的代码:

# 是时候尝试一下了！

让我们花点时间庆祝我们的进步！

如果你发布你的网站并且看一看，你应该能看到一些运动。

干得好！

但是，如果您仔细观察，您可能会注意到一些问题:

1.  向我们的集合添加新位置不会更新地图
2.  地图的缩放级别不会自动更新。如果标记放置在离中点太远的地方，它在框架中是不可见的

这些都是很好的观察结果，也是我们下一集也是最后一集要讲的内容。

# 结束第四集

多么惊人的一集！我们终于展示了来自不同地点的标记，看起来棒极了！你做得很好。

在这一集里，我们用大量的代码讲述了很多东西。你能走到这一步真的很了不起。一如既往，我真的很感激你能熬过这一关。如果它对你有帮助并且有价值，请考虑给我鼓掌或者订阅我的电子邮件。

在我们的最后一集里，我们将展示一幅动态更新的美丽完整的地图来结束我们的系列。

## 剧集列表:

这一系列剧集的完整列表链接如下，帮助你快速浏览❤

1.  [建立我们的网站](https://appnologyjames.medium.com/setting-up-our-website-multiple-markers-on-google-maps-with-wix-velo-d110a3542994)
2.  [查询位置集合并计算中点](https://appnologyjames.medium.com/querying-location-collection-and-calculating-midpoint-multiple-markers-on-google-maps-with-wix-d93e3389a12a)
3.  [显示地图&标记](https://appnologyjames.medium.com/displaying-map-marker-multiple-markers-on-google-maps-with-wix-velo-7a9f2209fc7a)
4.  [HTML 元素&发送数据](https://appnologyjames.medium.com/html-elements-and-sending-data-multiple-markers-on-google-maps-with-wix-velo-2f4e8a6f3336)
5.  [自动更新和调整地图大小](https://appnologyjames.medium.com/auto-updating-and-resizing-map-multiple-markers-on-google-maps-with-wix-velo-874b68c77483)