# 使用 nodemailer 在 NodeJS 中发送电子邮件

> 原文：<https://blog.devgenius.io/use-nodemailer-to-send-email-in-nodejs-3902e3b43d00?source=collection_archive---------1----------------------->

## 在[我的技术文章](https://yumingchang1991.medium.com/technical-article-structure-on-medium-954850e1ef4d)中查看我所有的其他帖子

![](img/8a76d16f827eb3ecde1fc7fe231d9439.png)

[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

作为后端开发人员，我们都需要从服务器向客户端发送电子邮件。在本文中，我将向您展示如何在 Node.js 中使用`nodemailer`来实现这一点。

一旦您掌握了这些知识，将所学知识应用到您的 Express 服务器上应该是小菜一碟。(剧透:只需将代码放在 express route 中的一个即可)

# 什么是`nodemailer`？

`nodemailer`是一个 node 包，使得在 Node 中发送邮件变得更加容易。

# 首先，我们来谈谈礼仪

正如 web 服务器遵循 HTTP 来传递消息一样，也有用于电子邮件通信的协议:

*   SMTP ，简单邮件传输协议，是*通过互联网发送*电子邮件的指南集合。这是我们将在本文中使用的协议
*   **IMAP** ，互联网消息访问协议，是*从服务器接收*电子邮件的指南集合。日期、发件人和主题最初从服务器下载，而内容仅在用户打开邮件时下载
*   **POP3** ，邮局协议，是从服务器接收电子邮件的指南集合。3 代表版本三，这是该标准最广泛使用的版本。POP3 以其对互联网连接的低依赖性而闻名。它将电子邮件从服务器传输到客户端，这样即使我们没有连接到互联网，我们也可以阅读电子邮件

如果你对更详细的比较感兴趣，看看[*IMAP vs pop 3 vs SMTP——终极比较*](https://www.courier.com/guides/imap-vs-pop3-vs-smtp/) 。

# 履行

我发现 nodemailer 的实现非常简单。所以直接看一下代码:

没那么可怕吧。

# 值得你注意的事情

## Gmail 附带免费的 SMTP 服务，但也有不好的一面

如果你有一个 Gmail 账户，你可以直接用`nodemailer`使用那个账户。然而，这样做有几个缺点。

**先说**，出站邮件有限制:每天 500 封，每个收件人算 1 封出站邮件。

**其次**，Gmail 有更安全的认证。当您将代码投入生产时，生产服务器很可能不在您现在所在的位置。它可能在一百英里之外，或者在一个不同的国家。谷歌将检测到这种位置差异，并阻止此类呼叫。因此，不建议在生产中使用 Gmail。(无论如何，我们可以用它来学习)

在制作中，我们经常求助于提供邮件递送服务的公司，如 [SendInBlue](https://www.sendinblue.com/why-sendinblue/) 和 [MailChimp](https://mailchimp.com/pricing/marketing/) 。

**最后**，您可能需要从谷歌账户中心配置应用代码，以用作`nodemailer`中的认证密码。否则，您可能会看到错误消息:*用户名和密码不被接受*。

## 端口 25、467、587、2525 可以用作通信端点

然而，端口 467 已经过时，端口 25 经常被云服务提供商和 ISP 阻止。

它离开我们港口 587 & 2525。前者使用最广泛，后者通常在端口 587 不可用时作为替代。

## Nodemailer 可以发送 HTML 格式的内容、日历邀请、附件等等

[查看他们的文档](https://nodemailer.com/message/)了解如何在邮件选项中使用它们

祝您愉快！