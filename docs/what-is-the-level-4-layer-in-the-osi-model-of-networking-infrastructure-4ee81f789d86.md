# 网络基础设施的 OSI 模型中的第 4 层是什么？

> 原文：<https://blog.devgenius.io/what-is-the-level-4-layer-in-the-osi-model-of-networking-infrastructure-4ee81f789d86?source=collection_archive---------11----------------------->

![](img/0b7f93c8fb7555de28e73ab27f986a23.png)

米格尔·Á拍摄的照片。派素上的帕德里纳。

让我们谈谈 OSI 模型，或开放系统互连模型。

[Cloudflare](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/) 很好地描述了 OSI 模型:*“开放系统互连(OSI)模型是由国际标准化组织创建的概念模型，它使不同的通信系统能够使用标准协议进行通信。简单地说，OSI 为不同的计算机系统能够相互通信提供了标准。*

OSI 模型可以被视为计算机网络的通用语言。它基于将通信系统分成七个抽象层的概念，每一层都堆叠在最后一层之上。”

OSI 模型由 7 层组成。这样划分是为了描述每一层网络的分工。因此，这些层在其设计和实现中可以保持独立，原则上不与任何其他层紧密耦合。

在本文中，我们将讨论 OSI 模型的第 4 层。

第 4 层被称为**传输层**。

首先，听说过 TCP(传输控制协议)和 UDP(用户数据报协议)吗？这些是传输层的一部分，所以你知道传输层是非常重要的。

这一层实质上确保了通过它发送的数据的完全传输。换句话说，第 4 层负责向运行在主机上的应用程序进程传送数据。

传输层能够跟踪数据段并重试任何失败的数据段(在 TCP 的情况下，也就是说；这不适用于 UDP)。这样，它不仅能够管理流控制，还能够管理分段和去分段以及错误控制。

如果您使用 AWS 或者想了解更多，值得注意的是网络负载平衡器(NLB)有一个第 4 层负载平衡器类型。这与应用程序负载平衡器(ALB)形成对比，后者具有第 7 层负载平衡器类型。

网络负载平衡器的协议侦听器是 TCP、UDP 和 TLS，而应用程序负载平衡器的协议侦听器是 HTTP、HTTPS 和 gRPC。

[](https://medium.com/geekculture/what-is-the-level-7-layer-in-the-osi-model-of-networking-infrastructure-dad00a731a43) [## 网络基础设施的 OSI 模型中的第 7 层是什么？

### 让我们谈谈 OSI 模型，或开放系统互连模式，以及第 7 级在网络术语中的含义。

medium.com](https://medium.com/geekculture/what-is-the-level-7-layer-in-the-osi-model-of-networking-infrastructure-dad00a731a43) 

> 对于什么是传输层，网上有一些很好的解释。我分享几个特别好的:
> 
> 传输层这一层处理建立、维护和拆除连接的机制。传输控制协议(TCP，著名的 TCP/IP 协议族)在这一层运行。用户数据报协议(UDP)也在这一层运行。TCP 和 UDP 之间的最大区别是 TCP 是面向连接的，这意味着它会跟踪数据传送尝试，并在遇到问题(丢失的数据包、损坏的数据、无序到达的数据包)时尝试恢复。UDP 不是面向连接的。如果 TCP 是一个递送驱动程序，它将不会留下一个没有签名的包。如果 UDP 是送货司机，当包裹被扔向你家时，卡车可能会减速。

—/u/[**JuanGolbez**](https://www.reddit.com/user/JuanGolbez)

> 第 4 层—传输层:这是主机通信的方式。在第 3 层，你建立一条从设备到设备的路径，第 4 层是一种数据传输的语言。TCP 和 UDP 住在这里。TCP 有很多错误检查来确保数据包安全到达，如果没有，就需要重新传输。那是 TCP 的工作。UDP 就是不管，它让你发流量不用担心讹误。这也是港口生活的地方。因此，您在端口 80 上运行 web 服务器，而您在另一台计算机上的 web 浏览器查看托管的网页。

— /u/ [**Stunod7**](https://www.reddit.com/user/Stunod7)

顺便说一下，努力记住 OSI 模型的所有 7 层？这里有一个很好的肺炎装置给他们:**请不要碰史蒂夫的宠物鳄鱼**。物理层、数据链路层、网络层、传输层、会话层、表示层、应用层。

如果你觉得这很有帮助或者只是喜欢阅读这篇文章，考虑[注册成为一个媒体成员](https://tremaineeto.medium.com/membership)。每月 5 美元，你可以无限制地阅读媒体上关于软件、技术等主题的报道。如果你用我的链接注册，我会得到一小笔佣金。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入 Medium—Tremaine Eto

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)