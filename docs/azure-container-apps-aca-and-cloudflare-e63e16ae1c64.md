# Azure 容器应用(ACA)和 Cloudflare

> 原文：<https://blog.devgenius.io/azure-container-apps-aca-and-cloudflare-e63e16ae1c64?source=collection_archive---------0----------------------->

在本文中，我们将配置并使用 Cloudflare 来保护运行在 Azure 容器应用程序上的面向客户端的 web 应用程序。

![](img/cd6092794ee99e9eed0abdb2f0cbc683.png)

您将需要以下:
-域名
-运行在 ACA
上的 Web 应用程序- Cloudflare 帐户

## 1.启动并运行您的 web 应用程序

在 ACA 中运行容器化的 web 应用程序非常简单和灵活。如果你是 ACA 的新手，跟随这个快速入门是一个很好的起点:
[https://learn . Microsoft . com/en-us/azure/container-apps/quick start-portal](https://learn.microsoft.com/en-us/azure/container-apps/quickstart-portal)

![](img/f966834e45283846f2401af21209380f.png)

图 1: ACA 快速入门

一旦您的 web 应用程序启动并运行，导航到**自定义域**页面，并点击**添加**按钮**。** 在这里，我们将输入我们的域名，并记下**域名验证**记录。

![](img/31147aa013febc1c081e2e74ff20af91.png)

图 2:添加自定义域

## 2.将您的网站添加到 Cloudflare

将您的网站添加到 Cloudflare 后，导航到 SSL/TLS 页面。为了从可用选项中启用端到端加密，我们将选择完整(严格)模式。

![](img/c97c8abdc652832fca679481a0478085.png)

图 3: SSL/TLS 配置 Cloudflare

启用完全 SSL/TLS 加密模式后，导航至**原始证书**页面，并点击**创建证书**按钮。在这里，我们将创建将在 Azure 中使用的可信数字证书。

![](img/11cd2705add75bcf69e2b4587ea2362e.png)

图 4:原始服务器证书

保留默认设置。

*   使用 Cloudflare 生成私钥和 CSR
*   私钥类型 RSA (2048)
*   主机名列表—您域名的顶点(**example.com**)和通配符( ***.example.com** )。

点击**创建**后，您将看到一个生成了您的原始证书和私钥的页面。

![](img/cb5c0ed3d7aa461463970a037513d957.png)

图 5:原始服务器-密钥和证书

将此文件保存在您的本地计算机上。用扩展名**保存原产地证书。pem** 和扩展名为**的私钥。按键**

![](img/e6f9734ae8cb5838cc95cb571a760867.png)

图 5:本地存储的证书

接下来，我们将使用密码保护我们的证书，并生成**。pfx** 通过运行 [openssl pkcs12](https://www.openssl.org/docs/man1.1.1/man1/openssl-pkcs12.html) 命令。

```
openssl pkcs12 -inkey cloudflare.key -in cloudflare.pem -export -out cloudflare.pfx
```

的。我们生成的 pfx 文件将在 Azure 容器应用程序门户中使用，以完成设置，但首先，我们将更新 Cloudflare 中的 DNS 记录，以指向 Azure 容器应用程序。

导航到 Cloudflare 中的 DNS 页面，并按照 Azure 容器应用程序门户的**域验证**部分中的说明添加 A 记录和 TXT 记录。(参见- >图 2:添加自定义域)

![](img/b8efe8bda8c98ac6c814c594d8d121f5.png)

图 6: DNS 配置— Cloudflare

# 3.将自定义域和证书添加到 ACA

导航回 Azure 门户，让我们从上次停止的地方继续。

![](img/31147aa013febc1c081e2e74ff20af91.png)

图 7:添加自定义域— ACA

单击 Validate 按钮后，您应该会看到一条验证通过的消息。

![](img/5e72989b1ad4cedede4061d7d5f5e3b0.png)

图 8:域验证— ACA

如果您在 Cloudflare DNS 配置中为 A 记录选择了代理选项，您将看到 A 记录的错误状态。您可以忽略此状态，并通过单击“下一步”按钮继续证书配置步骤。

![](img/7e7241261e5ffc93c115731b163cbb04.png)

图 9:域验证— ACA — Cloudflare 代理

在下一步中，屏幕将提示您选择证书。由于我们没有要绑定的任何证书，我们将单击**创建新的**链接，在下一个屏幕中，我们将添加**。我们之前生成的 pfx** 证书。

![](img/bc36dcef6655140e19ef327874ac8d92.png)

图 10:添加证书 ACA

验证证书后，我们将添加它。

![](img/09b678decf84dadaaf18ef66bbd1eecb.png)

图 11:添加自定义域— ACA

完成上述步骤后，我们将在自定义域列表中看到我们的域。

![](img/ba63f0df72b974be88fc316b30b0e21d.png)

图 12:列出自定义域— ACA

至此，我们完成了。通过在浏览器中输入我们的域，可以访问我们的 web 应用程序。

![](img/f88729cf3aa9057b23dc0db286e0ee49.png)

图 13:结束

如果您想使用任何其他子域，如 www，您可以使用相同的证书重复上述步骤。

# 原产地限制

上述场景中的 Azure 容器应用入口在公共 IP 地址上公开。这为绕过 Cloudflare 的流量打开了大门。目前，ACA 不提供可用于防止请求绕过 Cloudflare 的高级访问限制。
要解决这一问题，您需要修改应用程序代码，通过检查请求的来源，只允许来自 Cloudflare 的流量。

# 进一步阅读

*   [Azure 上基于容器的应用——Simple 和 KuberneteslessTo 要了解更多可用选项，请访问](/@gjoshevski/container-based-applications-on-azure-simple-and-kubernetesless-2d3cc17f6c8)
*   [Azure 容器应用中的自定义域名和证书](https://learn.microsoft.com/en-us/azure/container-apps/custom-domains-certificates)