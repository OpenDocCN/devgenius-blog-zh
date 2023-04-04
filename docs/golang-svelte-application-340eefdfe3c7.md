# Golang +苗条应用程序

> 原文：<https://blog.devgenius.io/golang-svelte-application-340eefdfe3c7?source=collection_archive---------1----------------------->

# 语境

我将写下我使用 Golang 和 Svelte 开发应用程序的经历。本文将向您介绍一种开发 POC 应用程序或尝试一个想法的方法。我开发了一个待办事项应用程序，并想告诉大家我是如何开始并完成部署的。本文的基本思想是如何以最具成本效益的方式开发和部署一个潜在的想法。如果你正在考虑开发一个想法，这篇文章将建议一个快速开发、高性能且成本可预测的技术栈。

# Todopad 应用程序

首先，这里有一个应用程序的链接

> [https://todopad.in](https://todopad.in)

这个应用程序是我需要的东西，所以我有能力快速添加我的待办事项并标记它们。有了标签，我希望能够创建报告，并看到我在哪里花费了最多的精力。本文将带您了解技术选择、部署选择以及在云中运行应用程序。

# 源代码

[](https://github.com/madhanganesh/todopad) [## 一个简单的 Todo 应用程序

### cd api source _scripts/dev.sh 转到 run main.go API 服务器在端口 8080 启动 cd web npm 运行 dev 导航到…

github.com](https://github.com/madhanganesh/todopad) 

# 基本设计

这是一个简单的应用程序，具有典型的用户界面，应用编程接口和存储。

![](img/2eeff33efc3872ccea0cada6756955da.png)

使用 nginx，UI 作为 SPA 作为静态站点。该 API 还使用 nginx 的反向代理特性对外公开。这种方法的优点是 SPA(单页应用程序)非常灵活，它清楚地将 FE 和 API 问题分开。SPA 是独立开发和部署的。这也使移动应用程序能够轻松使用相同的 API。

# 技术选择

![](img/20df6ba78b8fcdb4a67e1fc579136c6b.png)

下面的部分解释了为什么选择的技术更适合这种应用。

# 为什么网络要苗条？

[苗条](https://svelte.dev/)是当今发展 SPA 最简单、最有趣的方式。它正确地抽象了 web 的开发，并且感觉使用诸如 CSS、HTML 和 JS 之类的本地 web 技能是很自然的。资产的捆绑包大小和运行时性能优于目前该领域的任何其他框架。它突出显示编辑器中未使用或未声明的样式。这是清理代码、重构和更早修复问题的一件轻而易举的事情。这种“开发经验”是使一项技术交付速度更快、错误更少的原因。Svelte 可以用不复杂的组件来思考，而且想法很容易转换成代码。观看这个[视频](https://www.youtube.com/watch?v=AdNJ3fydeao)，从马嘴中听到这项技术。

# 为什么选择 Golang for API

[Golang](https://go.dev/) 是最适合写 API 的技术。在 [todoapp](https://github.com/madhanganesh/todopad/tree/main/api) 中，我没有使用任何框架，只使用了标准的库 API。Golang 使您能够在没有认知负担和较少摩擦的情况下编写代码来开始开发和测试。Golang 具有很高的性能，同时消耗很低的 CPU 和内存(最大 25MB)。由于资源的使用与云中的成本成正比，Go 可以用更少的云成本很好地完成工作。

# 为什么选择 SQLite 用于数据库

[SQLite](https://github.com/mattn/go-sqlite3) for Golang library 是一个用于 SQLite 数据库的 Golang 瘦包装器。SQL 是 todo 应用程序的正确选择，因为它需要查询各种 todo 状态，并且还具有创建报告的功能。SQLite 在提供存储和查询的同时，也增加了额外的数据库成本。只需使用调配的虚拟机的磁盘即可启动。对于这种成本应该最小的应用程序，SQLite 是一个不错的选择。使用 SQLite 的其他主要优势:

*   编写单元测试轻而易举。您只需使用“:memory:”作为数据库和代码库可以使用数据库进行验证，没有模仿！。在 todopad 应用程序中，我进行了[单元测试](https://github.com/madhanganesh/todopad/blob/main/api/repository/todo_test.go)，每个测试创建一个新的数据库，运行一个完整的迁移脚本并执行测试用例。
*   SQLite 速度极快。你可以看到，如果尝试使用[托多帕德](https://todopad.in)。
*   管理部署非常简单。我已经在下面的章节中介绍过了。

查看此[视频](https://www.youtube.com/watch?v=XcAYkriuQ1o)，它生动地介绍了如何将 SQLite 作为生产数据库。

# 数字海洋水滴的部署

[Digital Ocean Droplet](https://www.digitalocean.com/products/droplets) 是一项在云中运行虚拟机的伟大技术。定价模型简单且可预测，速度快，用户界面简单直观。DO 也有一个[应用平台](https://www.digitalocean.com/products/app-platform) (PAAS)，它也非常适合快速部署和运行应用。我选择 VM 而不是 PAAS 的原因是 SQLite。使用 droplets，您可以连接外部存储并从应用程序访问。因为我想使用 SQLite，所以 droplet 是最好的选择。与 PAAS 的解决方案，你必须使用数据库服务，如 Postgres，这将额外花费最低 15 美元/月。显然，像这样的应用程序需要非常快速地部署，如果你是单一的开发人员，你将没有时间来设置和管理所有的基础设施。幸运的是，数字海洋有很好的文档，可以管理水滴，因为细节是方便的。为了开发一个应用程序，你需要管理以下事情。

*   域名注册
*   用 TLS 服务静态站点和反向代理配置 nginx
*   将 API 配置为 Linux 系统服务
*   自动部署网站和 API 代码

前两项是一次性活动，很容易管理，我将在下面的章节中介绍。对于部署，来自开发/构建机器的一个简单的 shell 脚本将完成我所介绍的技巧。

# 域名注册

我用 [Godaddy](http://godaddy.com) 创建了一个域名。在 Godaddy 中，您唯一需要的配置是将创建的域名服务器更改为 digital ocean。这些步骤如下所示:

[](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars#registrar-godaddy) [## 如何从通用域名注册机构指向数字海洋域名服务器|数字海洋

### DNS(域名系统)是一个命名系统，它将服务器的域名(如 example.com)映射到 IP 地址(如…

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars#registrar-godaddy) 

# 配置 Nginx

在 DO droplet 中部署 nginx 和配置免费的 TLS 证书非常容易。这里清楚地记录了这一步骤，并且很容易做到:

[](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04) [## 如何在 Ubuntu 18.04 上用 Let's Encrypt 保护 Nginx | digital ocean

### Let's Encrypt 是一个证书颁发机构(CA ),它提供了一种获取和安装免费 TLS/SSL 证书的方法…

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04) 

这将确保您有 nginx 设置和一个免费的 SSL 证书配置。

# 将 API 配置为系统服务

API 需要配置为系统服务，然后从 nginx 调用。应该有文件 create*/lib/systemd/system/go web . service*，内容是

```
[Unit]
Description=goweb[Service]
Type=simple
Restart=always
RestartSec=5s
ExecStart=/root/apps/bin/todopad-api
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=gowebservice
Environment="PORT=8080"
Environment="SECRET_KEY=<tbd>"
Environment="DATABASE_URL=/root/apps/bin/todopad.sqlite"
Environment="MIGRATIONS_DIR=/root/apps/bin"[Install]
WantedBy=multi-user.target
```

注意代码所需的环境变量在这里传递。最后，在 nginx 配置中为该服务添加一个反向代理，如下所示:

```
{ ... location /api/ {
    proxy_pass [http://localhost:8080/](http://localhost:8080/);
  }
}
```

# 部署脚本

一旦 nginx 配置完成，API 服务开始运行，我们就需要部署静态站点和服务二进制文件。这可以通过一个简单的 shell 脚本来完成。让我们看看应用程序的文件夹结构是如何维护和部署的。

![](img/19cf8b57dbcd5c4bd30bff64c9a1c17b.png)

这个简单的 [deploy.sh](https://github.com/madhanganesh/todopad/blob/main/deploy.sh) 在回购中。下面来自 root 的命令将把静态资产和 golang 二进制文件部署到给定的 DO 机器上。

```
sh ./deploy.sh
```

# 数据库迁移

数据库迁移是简化开发、测试和部署的一个基本要素。对于这个应用程序，我使用了 goose

[](https://github.com/pressly/goose) [## GitHub - pressly/goose:数据库迁移工具。支持 SQL 迁移和 Go 函数。

### Goose 是一个数据库迁移工具。通过创建增量 SQL 更改或 Go 函数来管理您的数据库模式…

github.com](https://github.com/pressly/goose) 

迁移脚本在 [repo](https://github.com/madhanganesh/todopad/tree/main/api/_scripts/migrations) 中维护。Golang API 每次启动时都会运行迁移。启动期间的代码如下所示:

![](img/742632240e338e441799d35e2b16413c.png)

从环境变量中读取 migrationDir。

# 测试

借助 SQLite 和自动数据库迁移，测试变得轻而易举。为 todopad API 编写了两种测试。

1.  [单元测试](https://github.com/madhanganesh/todopad/blob/main/api/repository/todo_test.go)
2.  [集成测试](https://github.com/madhanganesh/todopad/tree/main/api/test.i)

看看上面回购链接中的这些测试。集成测试实际上启动了一个 web 服务器并发出 API 调用。测试初始化在一个例程中启动 web 服务。

![](img/3da4f5f0f503d1128c2463c51602b17f.png)

注意 databaseURL 是作为内存中的(":memory ")传递的，而迁移目录是从 repo 本地目录传递的。

# 表演

借助 1 个 VCPU 和 1 GB 的单个虚拟机，系统能够轻松满足 700 个请求/秒。这个系统可以通过添加更多的液滴来轻松扩展。

```
plow "https://gokarma.in/api/todo?pending=true" -c 300 -n 20000  -H Authorization:"Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2NDIzMDc1MzUsImlzcyI6InRvZG9wYWQiLCJ1c2VyaWQiOjEsImVtYWlsIjoidGVzdEB0ZXN0LmNvbSIsIm5hbWUiOiJ0ZXN0In0.ijZNLd6xMJcx7khZo8AMTnSBmibCVXEgShefW2fmaWI"

Benchmarking https://gokarma.in/api/todo?pending=true with 20000 request(s) using 300 connection(s).
@ Real-time charts is listening on http://[::]:18888

Summary:
  Elapsed    28.338s
  Count        20000
    2xx        20000
  RPS        705.749
  Reads    0.429MB/s
  Writes   0.214MB/s

Statistics   Min      Mean      StdDev       Max   
  Latency   5.34ms  423.054ms  338.233ms  3.358065s
  RPS       327.75   701.53      115.7     905.98  

Latency Percentile:
  P50           P75        P90        P95       P99      P99.9     P99.99  
  265.044ms  602.817ms  708.102ms  809.729ms  2.10436s  3.22351s  3.352244s

Latency Histogram:
  235.321ms  8068  40.34%
  335.083ms  6320  31.60%
  595.078ms  2614  13.07%
  720.454ms  1973   9.87%
  1.02349s    623   3.12%
  1.623215s   167   0.84%
  2.290774s   214   1.07%
  3.278695s    21   0.11%
```

你可以在这里看到使用 Postgres [的比较结果。\](https://github.com/madhanganesh/todopad/tree/main/api)

顺便说一句，plow 是一个易于使用的 mic 基准性能工具。

[](https://github.com/six-ddc/plow) [## GitHub - six-ddc/plow:具有实时 web UI 和终端的高性能 HTTP 基准测试工具…

### Plow 是一个 HTTP(S)基准测试工具，用 Golang 编写。它使用优秀的 fasthttp 代替 Go 默认的 net/http due…

github.com](https://github.com/six-ddc/plow) 

# 运行成本

使用上述技术运行应用程序的成本是每月 6 美元。你可以从最便宜的 DO 液滴开始。DO 还提供了一个高吞吐量的卷，可以在其中存储 SQLite 数据库文件。这将把 SQLite 文件外部化到一个可靠的挂载上，对于一个 2 GB 的文件，每月增加 0.2 美元的成本。

![](img/a3657a103ab2e53e2575025dab0bb245.png)

# 谢谢

感谢阅读到目前为止。基本上是想提供一套现成的技术选择和可预测的低成本模型。这将有助于你快速、廉价地开发/验证你的想法。此外，随着您的应用程序消耗越来越多，建议的堆栈可以轻松扩展。请发表您的反馈，如果您正在使用[应用程序](https://todopad.in/)，请在回购中添加任何[问题](https://github.com/madhanganesh/todopad/issues)或问题