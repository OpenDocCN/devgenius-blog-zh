# 使用 Wix Velo 查询位置集合并计算中点——谷歌地图上的多个标记

> 原文：<https://blog.devgenius.io/querying-location-collection-and-calculating-midpoint-multiple-markers-on-google-maps-with-wix-d93e3389a12a?source=collection_archive---------10----------------------->

## Wix Velo 历险记

![](img/f718c66f27deb02356f7849c3c81440c.png)

欢迎回到我的用 Wix Velo 在 Google 地图上显示多个位置的系列文章。我很兴奋也很感激你选择看我学到的东西，我希望这篇教程对你有所帮助。

这是我们五集系列的第二集，我希望你会觉得它很有价值！

# 我们这个系列的目标

我们在这个系列中的目标是建立一个网站，将多个动态地图数据点的显示完全整合到一个[谷歌地图](https://www.google.com/maps)显示中。我们希望能够更新一个位置，并让它几乎立即显示在我们的网站上。如果你想了解为什么这很重要，这里是我的介绍。

**工作示例:**我们将使用的示例显示在[这里](https://www.creativeappnologies.com/multi-location-maps)。

**Github / Code Repo:** 对于觉得有用的人来说，这里的[是公开的 Github Repo](https://github.com/jimtin/dynamic-multi-location-maps-wix-velo)。

在这一过程中，我将分享一些关于安全性考虑的技巧，以及一些有帮助的观察，希望能为您节省大量的故障排除时间。每集的完整列表可以在每集的底部找到。

一如既往，希望这些内容有所帮助。当你花时间为一集鼓掌、订阅我的故事或使用[我的推荐链接](https://appnologyjames.medium.com/membership)注册 Medium 时，这对我意味着绝对的世界。它让我知道我的故事正在帮助你❤️.

尽情享受，并随时在 twitter 上给我发 [DM，在](https://twitter.com/blockchainvalu1)[LinkedIn](https://www.linkedin.com/in/jameshinton84/)/[Github](https://github.com/jimtin)上发表想法和评论😃

# 在这一集中

我们将介绍位置信息的初始查询和处理。这是这一集完整代码的链接。

我们将涵盖以下内容:

1.  设置我们的后端`.jsw`文件来包含我们的后端功能
2.  数据集的查询
3.  给定一组位置，计算数据的中点
4.  将结果结合在一起

我们开始吧！

# 后端文件设置

在所有现代 web 开发中，前端和后端开发是有区别的。这两者对网站的运作都至关重要，而且有一大堆关于它们对整体用户体验有什么贡献的讨论。

对于这个项目，我们将尽可能多地在后端进行处理，原因如下:

1.  **用户体验。**数据处理通常是计算密集型的。以减轻我们用户的负担(并节省他们的电池寿命！)尽可能地，我们希望将尽可能多的数据处理转移到我们的后端。我们在这个例子中所做的计算都不是超级密集的，但是这样做是很好的实践。
2.  **信息安全。**在后端处理的数据通常比前端处理更安全。通过将后端作为我们的处理媒介，并且只向用户呈现“他们的”信息，我们限制了恶意或无意暴露数据的机会。

我们是这样做的:

1.  在 Wix 编辑器上，选择 Dev 模式(通常在屏幕的右上角)
2.  在左侧，导航至“公共和后端”设置
3.  “后端”部分，单击+按钮
4.  选择“新建 Web 模块”
5.  创建一个名为`queryLastTenLocations.jsw`的文件

现在，您应该已经设置好了后端文件。干得好！

# 数据集查询

## 关于 Wix 的一些注释

Wix 有一些非常棒的特性，这使得它非常适合 web 开发。这里有两个与我们的项目相关:

1.  基于`node.js`这意味着我们几乎所有的代码(讽刺的是除了 HTML 元素)都可以用 javascript 编写，这确实减少了我们的开发时间。
2.  为我们的开发提供一整套功能齐全的 API

这两个因素的结合真的会帮助我们。

## 异步查询

Wix 广泛使用异步函数([如果你想了解这个](https://blog.risingstack.com/mastering-async-await-in-nodejs/)，这里有一篇很棒的文章)。这些由关键字`await`和`async`表示。根据一般经验，所有 Wix 数据查询都是异步的。这意味着，如果我们需要对我们请求的数据执行顺序(或者称为同步)操作，我们需要等待(即`await`)数据返回，然后再继续前进。

如果你不这样做，你会以类似于`promise unresolved`的错误结束，或者你会发现你的代码会告诉你它是`unable to perform operation on empty array`。

如果你在发展过程中遇到了这些错误，快速看一下你的承诺链——你可能会发现错误而不会太心痛。

## 查询集合

说完了，我们开始吧！

1.  导入我们将用于查询数据集的库`import wixData from 'wix-data';`
2.  创建一个函数，我们将需要它来查询数据集的最后十个条目。该函数将是异步的，并且不被导出(即在该文件之外可用)。我们将没有参数，因为我们希望查询每次都做完全相同的事情。我已经调用了函数`async function getLastTenEntries(){}`

使用第 1 集中关于您收藏的详细信息，填写以下详细信息:

1.  查询集合，以不超过 10 个条目的降序 createdDate 进行过滤。
2.  如果结果的数量大于 1(即集合非空)，则将字段名转换成对我们的应用程序有意义的名称
3.  捕捉发生的错误
4.  返回结果

代码如下:

从集合中检索条目的代码

# 中点计算

事实证明，当涉及到地图位置时，有多种方法可以计算中点。在不深入辩论的情况下，你可以从几个方面来思考这场讨论:

1.  我们希望中点与所有位置的距离相等吗？这可能意味着一些人的旅行时间很长，而另一些人的旅行时间很短。
2.  平衡精度需求和计算资源。
3.  考虑到地球的形状，它不是一个完美的球体。

如果这是一个你感兴趣的话题，这里有一些有趣的链接:[这里](http://www.geomidpoint.com/calculation.html)，这里[这里](https://www.makeuseof.com/tag/5-apps-find-halfway-points-meet-faraway-friends/)，如果你需要超级精确，还有一套[很棒的库](https://www.ffi.no/en/research/n-vector/n-vector-downloads)。

为了我们的目的，我使用了一种足够精确的方法来使我们的地图在 10 个坐标上居中。它相当简单，相当准确，不需要大量的坐标知识。

我们计算它的过程如下:

1.  将我们的十进制度数位置转换成弧度
2.  将我们的弧度位置转换成笛卡尔坐标
3.  从我们的笛卡尔坐标计算平均坐标
4.  将我们的平均坐标还原成弧度
5.  将我们的弧度坐标转换回十进制度数
6.  用我们的十进制度数返回一个对象

(需要特别注意的是，如果我们愿意，这种方法还允许我们对我们的位置进行加权。与本项目无关，但在其他地方可能对你有帮助)。

下面是完成这一切的代码:

# 结合在一起

我们的最后一步是将我们的 10 个位置和中点组合成一个 JSON 对象。我们还需要明确我们的目标，这个概念将在第四集进一步解释。这是我们将呈现给网站前端部分的对象。

我们还需要确定我们的数据所针对的目标。当我们开始填充 HTML 元素时，这将在教程 xxx 中变得更加相关。

这些步骤是:

1.  使用`getLastTenEntries()`函数查询最近 10 个位置的数据集
2.  将这些位置传递给`calculateMidpoint(locations)`函数来计算它们的中点
3.  创建一个对象来存储 10 个位置、中点和目标
4.  返回此对象

代码如下:

# 做一个快速测试

让我们测试一下这个函数，确保它能正常工作。

1.  选择 getLocationData()旁边的 Wix 编辑器上的“播放”按钮
2.  一旦屏幕弹出，再次按下播放，看看结果。你应该得到一个显示“位置”、“中点”和“目标”的对象。

记下数据的格式。

# 结束第二集

第二集到此结束！我们已经谈了很多了！

在我们的下一集，我们将深入我们的 HTML 元素，并准备好接收我们的位置！

## 剧集列表:

这一系列剧集的完整列表链接如下，帮助你快速浏览❤

1.  [建立我们的网站](https://appnologyjames.medium.com/setting-up-our-website-multiple-markers-on-google-maps-with-wix-velo-d110a3542994)
2.  [查询位置集合并计算中点](https://appnologyjames.medium.com/querying-location-collection-and-calculating-midpoint-multiple-markers-on-google-maps-with-wix-d93e3389a12a)
3.  [显示地图&标记](https://appnologyjames.medium.com/displaying-map-marker-multiple-markers-on-google-maps-with-wix-velo-7a9f2209fc7a)
4.  [HTML 元素&发送数据](https://appnologyjames.medium.com/html-elements-and-sending-data-multiple-markers-on-google-maps-with-wix-velo-2f4e8a6f3336)
5.  [自动更新和调整地图大小](https://appnologyjames.medium.com/auto-updating-and-resizing-map-multiple-markers-on-google-maps-with-wix-velo-874b68c77483)