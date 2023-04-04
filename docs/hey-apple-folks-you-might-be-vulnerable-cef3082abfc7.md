# 警告苹果公司的人，你可能很容易受到攻击

> 原文：<https://blog.devgenius.io/hey-apple-folks-you-might-be-vulnerable-cef3082abfc7?source=collection_archive---------5----------------------->

> 苹果在推出 iOS、iPadOS、macOS 和 watchOS 的带外更新几周后，又推出了另一项产品安全更新。

![](img/620d052bf2e342c69045350c6a8fc963.png)

[邦宇王](https://unsplash.com/@bangyuwang?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/apple-logo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在推出 iOS、iPadOS、macOS 和 watchOS 的带外更新几周之后，苹果公司又推出了另一个针对 iPhone、iPad 和智能手表的安全更新。此次更新解决了一个至关重要的零日漏洞，苹果声称这个漏洞正在被世界各地故意利用。

谷歌威胁分析小组的 Clement Lecigne 和 Billy Leonard 将该漏洞命名为 CVE-2021–1879。这是一个 WebKit 错误，可能会使攻击者处理恶意设计的 web 内容，从而可能导致普遍的跨站点脚本攻击。

[](https://medium.com/purple-team/learn-cross-site-scripting-attacks-from-scratch-3adff0331d1d) [## 从头开始学习跨站点脚本攻击

### 跨站点脚本(XSS)攻击是一种注入，其中将报复内容注入到一般的…

medium.com](https://medium.com/purple-team/learn-cross-site-scripting-attacks-from-scratch-3adff0331d1d) 

攻击者可以通过使用专门设计的 web 内容来利用该漏洞。由于跨站点脚本缺陷，他们可能会迅速将用户重定向到他们创建的欺诈性页面，个人或企业帐户的网络钓鱼登录信息，或发送恶意软件来监视用户，或从云服务中窃取数据。

Apple 确认该问题可能被故意利用，并为以下设备提供了修复程序。

***iOS 12.5.2 —手机 5s、iPhone 6、iPhone 6 Plus、iPad Air、iPad mini 2、iPad mini 3、iPod touch(第六代)。*
*iOS 14.4.2 — iPhone 6s 及更高版本，以及 iPod touch(第七代)。*
*IP ados 14 . 4 . 2—iPad Pro(所有型号)，iPad Air 2 及以后，iPad 5 代及以后，iPad mini 4 及以后。
Watch OS 7 . 3 . 3—Apple Watch Series 3 及更高版本。***

苹果似乎也在尝试以不同于其他操作系统更新的方式提供 iOS 安全更新。iOS 14.4.2 往往是受益于这种实现的更新类型。

攻击者意识到，在发现零日漏洞、发布补丁和最终用户部署补丁来修复问题之间存在不可避免的延迟。真正想忽略或推迟操作系统更新的人只会增加攻击者的黄金机会。安全专业人员需要一种机制来限制对公司云服务的访问，然后才在计算机上安装新的补丁。漏洞修复一经发布，基于云的安全解决方案就允许公司对所有用户实施访问策略。与此同时，苹果设备的用户应该尽快安装更新，以最大限度地降低与该漏洞相关的风险。

参考:[https://the hacker news . com/2021/03/apple-issues-urgent-patch-update-for . html](https://thehackernews.com/2021/03/apple-issues-urgent-patch-update-for.html)