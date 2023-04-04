# 众所周知的位置:保留的 URI 路径前缀

> 原文：<https://blog.devgenius.io/well-known-locations-a-reserved-uri-path-prefix-5277b2f37db7?source=collection_archive---------0----------------------->

## 它在域验证和电子邮件安全中的应用

![](img/38a1e6220d62f0cdb96d287086142ef6.png)

由[延斯·约翰森](https://unsplash.com/@jens_johnsson?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你知道吗？众所周知的 URIs 是那些以/开头的。众所周知的/在其路径中，格式为/。知名/ <后缀>。它由 [RFC 8615](https://tools.ietf.org/html/rfc8615) 定义。它们用于信息发现、策略和域验证等。提议的标准保留了路径前缀/的使用。众所周知的/在 HTTP，HTTPS，WS 和 WSS 方案。

> [**第 1 节**](https://tools.ietf.org/html/rfc8615#page-3) **:** 为了解决这些用途，本备忘录在 HTTP、HTTPS、WebSocket (WS)和安全 WebSocket (WSS) URIs 中为这些“众所周知的位置”保留了路径前缀，“/。众所周知/”。需要为这种元数据定义资源的未来规范可以注册它们的使用，以避免冲突并最小化对源的 URI 空间的影响。

目前[注册的后缀](https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml)包括用于域验证的“ *acme-challenge”、“PKI-Validation”*和提供电子邮件安全传输策略的“*MTA-STS . txt”*。

## 托管服务提供商

一个众所周知的 URI 的简单例子是'[](https://github.com/Automattic/hosting-provider)**'*后缀。它提供了谁是给定域或子域主机的提示。虽然这没有被广泛部署，但是它适用于 Word Press 托管的域，*

```
*$ curl [https://wordpress.com/.well-known/hosting-provider](https://wordpress.com/.well-known/hosting-provider)
WordPress.com*
```

# *SMTP MTA —严格的传输安全*

## *mta-sts.txt*

*为了改善电子邮件生态系统和控制垃圾邮件，多年来已经采用了几种方法 [SPF](https://en.wikipedia.org/wiki/Sender_Policy_Framework) 、 [DKIM](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail) 、 [DMARC](https://dmarc.org/overview/) 、 [ARC](https://www.rfc-editor.org/rfc/rfc8617.html) 。最新加入该列表的是在 [RFC 8461](https://tools.ietf.org/html/rfc8461) 和 [TLS-RPT](https://tools.ietf.org/html/rfc8460) 中定义的 MTA-STS。MTA-STS 与此相关，因为它结合使用 DNS TXT 记录和众所周知的 URI 进行[策略发现](https://tools.ietf.org/html/rfc8461#page-7)。*

> *策略本身是一组键/值对(类似于[RFC5322]中的头字段)，通过固定的“众所周知的”[RFC5785]路径中的 HTTPS GET 方法提供。著名的/mta-sts.txt "由策略主机提供。策略主机 DNS 名称是通过在策略域前面加上“mta-sts”来构建的。*
> 
> *因此，对于策略域“example.com ”,完整的 URL 是"https://mta-sts.example.com/。知名/mta-sts.txt”。*

*谷歌在其安全博客(2019)中宣布 Gmail 实现了 MTA-STS。有了众所周知的 gmail.com 的 URI，可以得到如下的政策:*

```
*$ curl [https://mta-sts.gmail.com/.well-known/mta-sts.txt](https://mta-sts.gmail.com/.well-known/mta-sts.txt)
version: STSv1
**mode: enforce**
mx: gmail-smtp-in.l.google.com
mx: *.gmail-smtp-in.l.google.com
max_age: 86400*
```

*微软也在研究 MTA-STS，他们目前对 outlook.com 和 T21 的政策如下:*

```
*$ curl [https://mta-sts.outlook.com/.well-known/mta-sts.txt](https://mta-sts.outlook.com/.well-known/mta-sts.txt)
version: STSv1
**mode: testing**
mx: *.olc.protection.outlook.com
max_age: 604800*
```

*另一方面，不深入 MTA-STS 的细节，通过引入 HTTPS 和著名的 URIs 来保护电子邮件的方法听起来并不太好。邮件/ SMTP 曾经独立于 Web / HTTP，但现在不是了。*

# *域验证*

## *极限挑战*

*[自动证书管理环境](https://tools.ietf.org/html/rfc8555)或 ACME 是一个用于自动验证和颁发证书的协议。它为域所有者提供了多种方法来证明对域的控制。一个这样的方法是 HTTP-01，它使用后缀' *acme-challenge '。*在这种方法中，ACME 服务器将向需要证书的域发送 HTTP 请求。请求的 URL 将采用以下格式:*

```
*http://<domain>/.well-known/acme-challenge/<token>*
```

## *pki 验证*

*同样，对于向域所有者颁发证书的认证机构，CA/浏览器论坛的[基线要求](https://cabforum.org/wp-content/uploads/CA-Browser-Forum-BR-1.3.8.pdf)(第 3.2.2.4.6 节)允许通过对网站进行商定的更改来验证域。这种方法使用后缀“ *pki 验证*”，并要求在众所周知的位置下放置一个特殊文件，*

```
*http://<domain>/.well-known/pki-validation/<file>*
```

# *安全问题*

*不幸的是，尽管 RFC 保留了对众所周知的位置的使用，但是 Web 服务器并没有将它们与其他位置区别对待。这导致恶意演员[利用*的存在。知名*目录](https://news.netcraft.com/archives/2018/01/29/the-hidden-well-known-phishing-sites.html)。*

*从众所周知的 URIs 的使用中可以明显看出，它们对安全非常重要。而且，它们的位置应该得到适当的保护，因为有多种方法可以利用它们。对网络来说，这绝对不是一件好事，但这是需要解决的问题。*

> *知名地点不是对网络不利吗？它们是，但是由于各种原因——包括技术和社会原因——它们有时是必要的。这份备忘录为他们定义了一个“沙箱”,以降低碰撞风险，并最大限度地减少对现场已有 URIs 的影响。*

*我们使用 ACME 为我们产品中的子域自动生成 TLS 证书。由于这需要使用众所周知的 URI 作为 HTTP-01 质询的一部分，我们已经采取措施禁止访问众所周知的位置，除非质询需要。*

## *仅当文件存在时*

*控制对众所周知的位置的访问的一种快速方法是添加一个 Web 服务器配置来检查文件(如果它存在的话)。这可以分别在 *nginx* 和 *httpd* 与 [**if (-f )**](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#if) 和[**<If-f>**](https://httpd.apache.org/docs/2.4/mod/core.html#if)指令上实现。nginx 服务器的配置示例如下:*

```
*location /.well-known/ {
  if (!-f "/secure/path/to/file") {
    return 400;
  }
}*
```

*这也可以很容易地与需要访问众所周知的位置的工具集成。在[证书机器人](https://certbot.eff.org/docs/using.html#renewing-certificates)的情况下，要自动更新证书，可以使用选项 **-前挂钩**和 **-后挂钩**，如下所示:*

```
*certbot renew --pre-hook "touch /secure/path/to/file" --post-hook "rm -f /secure/path/to/file"*
```

**在第 0 根，我们提供了一个解决方案，* [*第 0 根安全网络— 0SNet*](https://www.0snet.com/) *，它使用 TLS 客户端证书来保护组织的内部 web 应用。看看我们的产品吧，它很容易部署，也可以在* [*AWS*](https://0snet.info/#install.aws) *、* [*、GCP*](https://0snet.info/#install.gcp) *和*[*Azure*](https://0snet.info/#install.azu)*上看到。**