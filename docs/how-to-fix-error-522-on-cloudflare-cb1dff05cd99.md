# 如何修复 Cloudflare 上的错误 522

> 原文：<https://blog.devgenius.io/how-to-fix-error-522-on-cloudflare-cb1dff05cd99?source=collection_archive---------12----------------------->

![](img/7d066c0d128a4e15fc7f6c93a70e0809.png)

Cloudflare 团队在分享错误的可能原因以及如何解决这些问题方面做得非常好。我很快概述了以下这些；

# 522 错误的原因

> 当 Cloudflare 联系原始 web 服务器超时时，会出现错误 522。两种不同的超时会导致 HTTP 错误 522，具体取决于它们在 Cloudflare 和原始 web 服务器之间发生的时间:
> 
> 1.在建立连接之前，原始 web 服务器不会在 Cloudflare 发送 SYN 的 15 秒内将 SYN+ACK 返回给 Cloudflare。
> 
> 2.建立连接后，原始 web 服务器不会在 90 秒内确认(ACK) Cloudflare 的资源请求。
> 
> 来源: [Cloudflare 支持](https://support.cloudflare.com/hc/en-us/articles/115003011431-Troubleshooting-Cloudflare-5XX-errors#:~:text=Error%20522%3A%20connection%20timed%20out,-Error%20522%20occurs&text=Before%20a%20connection%20is%20established,resource%20request%20within%2090%20seconds.)

这篇 [Quora 帖子](https://qr.ae/pvcKqB)涵盖了连接超时消息的更多原因。

# 我是如何解决错误的

就我而言，我意识到我有两个 A 记录集。第二个是在我的域名处于赎回宽限期时自动添加的——我没有及时续费。

Cloudflare 早些时候给我发了一封电子邮件，说我的名称服务器不再指向 Cloudflare，我的 DNS 记录将在 7 天内从 Cloudflare 系统中完全删除。这证明了我为什么会发现新的 DNS 记录—可能是由 Cloudflare 添加的。

然而，当我试图通过添加第二个 A 记录来重现这个问题时，我的应用程序运行得很好。我不知道为什么我不能得到同样的错误。

*伟大的一周即将到来！*

*最初发布于*[*https://Belinda marionk . hash node . dev*](https://belindamarionk.hashnode.dev/how-to-fix-error-522-on-cloudflare)*。*