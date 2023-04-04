# HTTP，嗯？

> 原文：<https://blog.devgenius.io/http-huh-70d6add8ed3c?source=collection_archive---------8----------------------->

## HTTP 代表超文本传输协议。酷，它是什么，为什么它很重要？

![](img/99ced877ab410f090fa8c7524be85ce4.png)

简单地说，就是客户机和服务器使用 HTTP 协议互相交谈，就像我们使用电话互相交谈一样。在让你更加困惑之前，让我们来看看什么是客户机和服务器。

**客户端**:用户在浏览器上浏览网页获取信息。

**服务器:**另一台你看不见的电脑(存储数据)正等着给你你在网上要的任何数据。

让我们创建一个场景来更好地理解这是如何工作的。

场景:我想去脸书(并滚动几个小时 lol)。客户端和服务器通信(使用 HTTP 协议)的过程如下所示。

1.  你在浏览器里输入网址->[**https://www.facebook.com**](https://www.facebook.com)(注意是以 HTTP 开头的)。浏览器需要它来与服务器对话。
2.  发生的第二件事是，浏览器查找该域名，并试图找到该网站的匹配 IP 地址。
3.  一旦浏览器定位到 IP 地址，它将发送一个 Http **GET** 请求(请求数据)。我们需要告诉服务器我们想要**获取**数据，而不是**发送**(发送数据)。
4.  然后，服务器发回对我们请求的响应，称为 **HTTP 响应**。在这个过程中，服务器会将您在请求中请求的所有数据返回给您。如果我们需要的话，它会有额外的信息，以及浏览器将呈现到屏幕上的 index.html 文件(脸书页面)

我没有提到这是步骤，但是如果我们不小心在 URL 中将 facebook 拼错了->[**https://www.facbook.com**](https://www.facbook.com)我们的请求会被认为是糟糕的请求。

作为开发人员，我们需要知道我们的请求是好是坏。服务器使用状态代码让我们知道这一点。如果我们输入了错误的 URL(拼错了 facebook)，我们的服务器会给我们发送一个 **404 状态码**。意味着没有找到该页面。一个好的 URL 服务器会在响应旁边发送一个 **200 状态码**，这意味着一切正常。还有其他的状态代码需要查找，但这是您经常会遇到的两个主要的状态代码。

简而言之，这就是客户端通过 HTTP 协议与服务器对话的方式。我只是触及了这个话题的表面。请随意深入搜索“什么是 HTTP？”。

我用过的一个资源:[什么是 HTTP？](https://www.khanacademy.org/computing/computers-and-internet/xcae6f4a7ff015e7d:the-internet/xcae6f4a7ff015e7d:web-protocols/a/hypertext-transfer-protocol-http)

我根据这个[前端开发者](https://roadmap.sh/frontend)指南来写这些帖子，以帮助初学者理解这些话题。尽情享受吧！

还有，我最近成为了 Fiverr 上的自由职业者(用我的技能去帮助别人)。有机会的话来看看我的演出。:)

[五环链接](https://www.fiverr.com/ajeasmith/convert-your-psd-design-to-a-website)