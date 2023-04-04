# DNS 可靠性/停机时间

> 原文：<https://blog.devgenius.io/dns-reliability-downtime-d6aa90ef868f?source=collection_archive---------4----------------------->

![](img/574ac8766c4d6041fe98e1f787b2667d.png)

数字存在和服务在商业和社会中越来越不可或缺。因此，当一个重要网站(更不用说许多网站)出现意外的长时间停机时，它就变得更有新闻价值了。很少有大部分互联网变得无法访问的情况，其中一种情况是许多网站依赖的域名系统(DNS)服务提供商陷入瘫痪。

# 案例研究

Tim Greene 写了 2016 年 10 月 21 日的一个重大事件，当时发生了一个有新闻价值的事件，大多数网站对用户不可用。原因是为数不多的 DNS 服务之一 Oracle(以前的 Dyn)因拒绝服务攻击(DDOS)而变得无法访问。因此，虽然网站是活跃的，但用户如果不知道他们的互联网协议(IP)地址，就无法访问这些网站。结果就好像网站本身宕机了一样(Greene，2016)。

![](img/42b35c954273ae492625758f58dbe071.png)

# 域名系统

DNS 是让大众访问互联网的一个重要方面。它允许用户输入字母字符——[www.awh.net](http://www.awh.net/)——而不是实际的数字 IP 地址——(第 4 版)/字母数字(第 6 版)——来访问网站。您可以通过托管个人 DNS 服务器或使用第三方服务来设置 DNS 解析，这样就可以访问您的站点。

# DNS 冗余和健康检查

第一步是通过运行自托管 DNS 服务器的组合或使用多个互不依赖的 DNS 服务提供商来设置冗余。研究 DNS 提供商是必不可少的，因为一些 DNS 提供商只是中间人和其他提供商的客户，所以如果你不做你的研究，你可能会两次使用同一个 DNS 服务器(Gibson)。

一些服务提供商，如 AWS，提供保存日志和检查 DNS 服务器是否正确运行的服务，并允许在出现意外故障时进行应急设置。对于 AWS，该功能被称为亚马逊路由 53，它提供健康检查，并允许客户指定他们希望如何在 AWS 的生态系统中处理 DNS 故障。[点击这里查看](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover-configuring.html)。

使用像 AWS 这样的公司来托管你的网站和 DNS 解析要容易得多，但是有一个权衡，如果 AWS 宕机，你的网站将会消失，直到 AWS 恢复。使用相互独立的多家公司意味着需要做更多的工作来确保您的设置和部署过程与两家公司的界面兼容，但是停机的几率会降低。

# 资源

布恩，约瑟夫。(2020 年 7 月 18 日)。如何设置网站 DNS 配置设置？*helpdeskeek*。检索自[https://helpdesgeek . com/how-to/how-to-set-up-website-DNS-configuration-settings/](https://helpdeskgeek.com/how-to/how-to-set-up-website-dns-configuration-settings/)

谷歌云。 *DNS 最佳实践*。[https://cloud.google.com/dns/docs/best-practices](https://cloud.google.com/dns/docs/best-practices)

分析现代网络服务中的第三方服务依赖:我们从 Mirai 未来组合-戴恩事件中学到了什么？https://dl.acm.org/doi/pdf/10.1145/3419394.3423664

维基百科。 *IP 地址*。[https://en.wikipedia.org/wiki/IP_address](https://en.wikipedia.org/wiki/IP_address)

## 来源

格林蒂姆。(2016 年 10 月 21 日)。Dyn DD0S 攻击是如何展开的。*网络世界*检索自[https://www . network world . com/article/3134057/how-the-dyn-DDOS-attack-unfolded . html](https://www.networkworld.com/article/3134057/how-the-dyn-ddos-attack-unfolded.html)

云闪。*什么是 DNS？[https://www.cloudflare.com/learning/dns/what-is-dns/](https://www.cloudflare.com/learning/dns/what-is-dns/)T21*

吉布森，史蒂夫。现在安全了！# 795–12–01–20 DNS 整合。[https://www.grc.com/sn/SN-795-Notes.pdf](https://www.grc.com/sn/SN-795-Notes.pdf)

## -劳伦·米切尔，AWH 软件开发团队负责人