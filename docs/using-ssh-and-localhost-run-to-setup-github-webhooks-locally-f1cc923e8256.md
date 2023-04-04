# 使用 SSH 和 localhost.run 在本地设置 GitHub webhooks

> 原文：<https://blog.devgenius.io/using-ssh-and-localhost-run-to-setup-github-webhooks-locally-f1cc923e8256?source=collection_archive---------6----------------------->

GitHub Webhook 使警报能够在存储库上定义的活动发生时被发送到外部 web 服务器。每当存储库采取某个动作时，就可以触发 Webhooks。

要设置 Webhooks，您需要提供一个可公开访问的地址，这就是为什么如果您的服务器运行在云上，设置 GitHub Webhooks 会很容易，因为您有一个可以向其发送通知的公共 IP 地址。

如果你的应用运行在本地，你如何告诉 GitHub 将 webhook 事件传输到你的本地应用？互联网无法访问您的本地应用程序。要在本地系统上获得通知，我们需要的是隧道，即在您的本地计算机和可公开访问的端点之间建立链接。创建隧道的服务有很多，可以使用 ngrok、localhost.run 等等。在本教程中，我将使用 localhost.run。

![](img/51e1912536f58cbce2d6520955eb8280.png)

# **设置 localhost.run**

第一步是创建一个 ssh 密钥对来保护您的系统和远程系统之间的连接，您可以使用以下命令创建它，并在每次查询时按 enter 键。

> ssh-keygen -t rsa -b 2048

![](img/6c294491b724f410583128c373801f73.png)

**使用 localhost.run 创建 HTTP 隧道**

我们可以使用本地主机。运行以在本地系统和他们的系统之间创建一个隧道，这里我允许端口 80 监听外部世界，并将请求传输到端口 5000，这是我的应用程序端口。

> ssh -R 80:本地主机:5000 本地主机.运行

![](img/b99a33aab99b81108fc8ce59831106e5.png)

localhost.run 返回一个链接，通过这个链接，我们可以将请求从外部发送到本地端口 80。

我们需要给 GitHub Webhook 提供这个链接，以便每当有人向资源库推送新代码时发送通知。

![](img/c16f2dcb72b373e44a6ee7163933d708.png)

**我们做了什么？**

我们已经创建了一个设置，当有人将新代码推送到存储库时，GitHub Webhook 将向我们的本地端口 80 发送通知，localhost.run 将接收通知并发送到端口 5000。如果我们的应用程序运行在 5000 端口上，它将收到代码已经在存储库上更改的通知。

如果我们的应用程序不能理解 webhook 通知怎么办？

如果应用程序不支持 webhooks 通知接收，那么我们可以手动运行一个 shell 脚本，每当有请求到达端口 5000 时，我们只需运行“git pull”命令，该命令将使用新代码更新当前代码。

为了在端口 5000 上接收请求，我们需要“netcat ”,这是一个用于读取传入 TCP/UDP 请求的实用程序。并且在永久 while 循环中运行它将使它保持活动状态，以便在回购上进行进一步的更改。下面是简单的脚本。

> #!/bin/sh
> while:
> do
> echo ` NC-LV 5000>null`
> echo ` git pull`
> done

每当请求到达端口 5000 时，nc 命令将完成执行，git pull 将运行，在 while 循环的帮助下，它将再次等待新的请求。

![](img/3b843436bb80a397227accee79271051.png)

*注意-名称解析即将失败，因为端口 5000 不代表任何应用程序。*

# 结论

我们已经创建了一个设置，GitHub 可以将 Webhooks 通知发送到我们的本地系统，使用该通知，我们可以将它发送给 Jenkins 等工具，这些工具可以通过通知触发并执行后续任务，或者如果我们不想要第三方应用程序，我们可以使用 Netcat，它可以接收 GitHub Webhooks 的通知并执行后续任务，如使用存储库更新当前代码。

一定要鼓掌！！