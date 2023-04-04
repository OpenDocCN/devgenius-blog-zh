# 当您的时间有限时，平衡最佳实践

> 原文：<https://blog.devgenius.io/balancing-best-practices-when-your-time-is-limited-74cdd0959824?source=collection_archive---------4----------------------->

## 运输玩具项目的关键是选择最有影响力的最佳实践，这些实践将在未来为您节省时间和痛苦。

🔔这篇文章最初发布在我的网站上，[MihaiBojin.com](https://MihaiBojin.com/projects/golang/best-practices-vs-time?utm_source=Medium&utm_medium=organic&utm_campaign=top-promo)。🔔

![](img/f4b10e77812bb018fc6abe2009a1bb24.png)

由[蒂姆·埃文斯](https://unsplash.com/@tjevans?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在我的上一篇文章中，我谈到了[专注于产品创造](https://mihaibojin.com/building-products-vs-non-functional-requirements?utm_source=Medium&utm_medium=organic&utm_campaign=rss)并避免无休止的技术研究和其他非功能性(但有趣)任务的陷阱。

这对我来说很难，因为我喜欢做这项研究！

唉，我给自己定了一个目标，我要去实现它！

> 我已经开始在公共场合做一个更大的项目。我将在构建它的时候写下它，所以如果你对跟随这样的旅程感兴趣，请继续关注。另外，如果你愿意，你可以[订阅我的时事通讯](https://mihaibojin.medium.com/subscribe)！

我正试图将我在各种科技公司 17 年的职业生涯中积累的软件开发技能与我在业余时间建立一个项目所需的零碎技能联系起来。为一家大公司在一个足够大的团队中开发软件需要一定的质量关口，这些关口是我在日常工作中非常自豪坚持的！然而，作为一个试图在我剩下的**很少的个人时间里启动一个项目的孤独的开发者，我必须做出一些权衡来推出一些东西。**

Gergely 谈到了将软件交付生产的极端情况。许多项目以 YOLO 的方式开始，手动将二进制文件复制到生产服务器，并祈祷它不会崩溃(结果有一天发现它崩溃了)。

尽管这是开始一个项目的最务实的方式(这都是关于代码的，以后再担心其他事情)，**我强烈相信 CI/CD 管道和可再生/可重新配置的环境！**

因此，这个项目的一个硬性规定是 main 上的每个提交都要到达生产环境，而不需要任何手动跟进。

手头的任务是找到一个邮件陷阱服务，它可以充当 [MX 主机](https://en.wikipedia.org/wiki/Message_transfer_agent)，拦截所有发送到我的域的电子邮件，并将它们保存在数据库中以供进一步处理。我想这个技术术语是“入站解析 Webhook ”,但我更喜欢“邮件陷阱”。

一开始，我以为这是容易的一步。我所要做的就是找一个像 [Mailgun](https://www.mailgun.com/) 或 [Sendgrid](https://sendgrid.com/) 这样的服务来拦截我所有的电子邮件，并将它们保存到数据库中。然而，问题是这两种服务的免费计划都非常有限，而且费用随着邮件数量的增加而飙升。

由于使用这些服务都需要一个学习过程，而且这是一个不会产生收入的个人项目，所以我需要留意现在和未来的任何成本。它只需要一个垃圾邮件机器人找到我的领域，以创造一个头痛…

所以，我决定从头开始写！

令人震惊，我知道！😀

这与我之前的想法不一致，也许是正确的，但从好的方面来看，这种方法有几个好处:

*   我学到了很多关于 SMTP 的知识——它比你想象的要复杂得多。
*   我在刷新我的 Golang 技能。
*   我没有在某一天醒来看到一张巨额账单，而是冒着服务不堪重负、停止处理请求的风险；这是一个我暂时可以承受的风险！

解决了这个问题，下一步就是选择一个技术堆栈并完成它。之前提到过 Golang，但不是从那里开始的。我的第一次尝试是将 NodeJS 与 [Mailparser 库](https://nodemailer.com/extras/mailparser/)一起使用；但是，我发现它很麻烦。它返回了可序列化和不可序列化的混合对象，后者我不容易存储在数据库中。

再一次，这归结于时间，我觉得使用 Golang 构建相同的特性更容易，与必须发布所有的 NodeJS 及其许多依赖项相比，生成微小的静态链接的二进制文件(和容器)具有额外的长期好处。

我最初的想法是依靠谷歌云或亚马逊 AWS 来部署和运行容器。这并没有产生太好的效果，因为[light sail 容器服务的公共端点只支持 HTTPS，它不支持 TCP 或 UDP 流量](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-container-services)；Google Cloud Run 的情况与此非常相似(参见“HTTPS 网址”)。

此时，我面临着一个决定。

回到在 Kubernetes 上部署(成本昂贵)，获得一台服务器，并安装 [Nomad](https://www.nomadproject.io/) (酷但耗时)，或者使用虚拟机。

我决定让事情尽可能简单，尝试[数字海洋的水滴](https://www.digitalocean.com/products/droplets)——便宜，启动快，他们有一个[的地形提供商](https://registry.terraform.io/providers/digitalocean/digitalocean/latest/docs)。

我使用 Terraform 来管理我的项目所需的任何基础设施和配置。如果集装箱管理解决方案成功的话，Terraform 将会是管理整个流程的最佳选择。然而，我被更重的 VMs 卡住了，即使是今天的[超快鞭炮微型 VMs](https://firecracker-microvm.github.io/) 。此外，重新配置虚拟机没有我希望的那么快(即时读取)。所以我选择了一个混合的解决方案:让虚拟机永久运行(但是能够使用我的 Terraform 配置重新配置它；虚拟机确实会出现故障或被移动并不时重启)，并在每次提交时在运行的虚拟机上 SCP 更新的二进制文件。

我以为我已经拥有了数字海洋的 Droplets 所需要的一切，直到我发现他们的[浮动 IP 不支持 SMTP 流量](https://www.digitalocean.com/community/questions/smtp-and-floating-ips)。

由于我运行一个(可能更多)MX 主机，我想重用相同的公共 IP，而不改变 DNS 记录。此外，万一虚拟机需要重新配置，IP 可能会发生变化，此时 DNS 缓存可能会出现问题。

这一切都让我想起了我在赫茨纳的老朋友。十年前，我曾经从他们那里租了一台服务器，我很高兴看到他们仍然存在并蓬勃发展！他们的硬件是基于欧洲，这是一个隐私加分。他们的云虚拟机速度超快。他们有一个[平台提供商](https://registry.terraform.io/providers/hetznercloud/hcloud/latest/docs)。他们的浮动 IP 支持 SMTP 流量(当然，你首先要为使用他们支付 4 欧元/月，但是，嘿，你不能拥有一切！)

答对了。

让我转入一个题外话。**构建软件从来没有完美的解决方案；凡事总有取舍！**我可以轻松地坚持使用 Digital Ocean，并在每次虚拟机重启时获得一个新的 IP。我怀疑这是如此罕见的事件，它将是一个非问题。然而，如果你构建的东西预期会失败，你将永远没有动力去完成它们。因此，我梦想我的项目将成为一个成功的网站，有许多游客，将需要许多虚拟机来处理所有的入站电子邮件流量。因此，我计划在坚实的基础上灵活地实现这一目标。我目前没有使用*海兹纳的浮动 IP 地址*，因为我不想每月花费翻倍。然而，当我完成这个项目时，我将开始招致这一成本(并且可能通过在不同的可用性区域中旋转服务的副本而再次加倍)，以使它成为产品级的。

所有这些都过去了，我有了自己的筹码:

*   [Hetzner Cloud](https://console.hetzner.cloud/) (2 个 vcpu/2gb RAM，每月 5 欧元；这比 Lightsail 的 7 美元 0.25 VCPU 的价格便宜了大约四倍)
*   Hetzner 浮动 IPs (4 欧元/月，可选)
*   [Cloudflare](https://www.cloudflare.com/) 用于 DNS 记录(免费)
*   [Terraform](https://www.terraform.io/) 用于提供基础设施和部署二进制文件(GitHub 上的免费状态托管——这对于 2+工程师团队来说是不允许的，但是，嘿，它对我很有效！)
*   [Golang](https://go.dev/)+[go-SMTP](https://github.com/emersion/go-smtp)用于处理收到的邮件
*   [Goreleaser](https://goreleaser.com/) 打包 Debian 发行版的二进制文件和一些 SystemD 脚本来管理安装/重装流程
*   [Google Cloud Firestore](https://firebase.google.com/products/firestore) 作为数据库(对我的流量水平免费)
*   [Google Cloud Logging](https://cloud.google.com/logging) 用于集中日志摄取和处理(对我的流量级别免费)
*   当然还有 GitHub 来托管我的代码(免费)

唷，比我想象的要长！

我只把它放在叙述中，因为开发有用的代码摘录非常耗时。如果你对技术故障/教程感兴趣，请在 Twitter 上告诉我。

下次见，米海

如果你喜欢这篇文章，并想阅读更多类似的文章，请订阅我的简讯。我每隔几周就发一封！