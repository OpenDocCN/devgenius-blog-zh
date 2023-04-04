# 如何用 Go 刮网站

> 原文：<https://blog.devgenius.io/how-to-scrape-a-website-go-a3e9d8390c6d?source=collection_archive---------29----------------------->

![](img/051ee0c89667e64cd9e20234dbc7ba73.png)

Pathum Danthanarayana 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

抓取网站对于许多应用程序都很有用，无论您是构建一个个人项目来从网站中提取一些细节，还是构建一个网络爬虫。但是，在做任何网络抓取之前，要合乎道德。确保你没有造成[拒绝服务](https://en.wikipedia.org/wiki/Denial-of-service_attack)，并了解目标网站的条款和条件，以免自己陷入麻烦。不要害怕，因为这是相当无害的，但是如果您正在构建一个生产应用程序或试图将其用于您的业务，请确保您了解它的所有方面。现在，让我们进入技术细节。

# 发出 Http 请求

像任何编程语言一样，Go 也有 http 库，提供连接和检索内容的实用方法。看看 http 包-[https://golang.org/pkg/net/http/](https://golang.org/pkg/net/http/)。

http“get”方法向给定的 url 发出“GET”请求。默认情况下，超时值为 0。这意味着，它是不确定的，它永远不会超时，这可能会使您的应用程序挂起。一般来说，在创建客户端时更新超时是一种好的做法。

**设置超时**

创建新的客户端允许您更新默认配置，并使用创建的客户端来获取网页。这将确保您的应用程序不会无限期等待，如果目标网站需要永远连接。当这种情况发生时，它会在“err”中给出一个超时错误，您可以相应地处理它。

```
Received error while craping:  Get "[https://www.crawler-test.com/](https://www.crawler-test.com/)": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
```

超时设置看起来很直观，但是有一些隐藏的细节。http 调用包含-

```
Dial -> TLS Handshake -> Request -> Response.Headers -> Response.Body -> Idle
```

“http 客户端”的“超时”字段涵盖了所有这些方面，但是如果需要，您可以单独处理超时的每个部分。请务必在此处查看 cloudflare 关于超时[的指南。](https://blog.cloudflare.com/the-complete-guide-to-golang-net-http-timeouts/)

**设置标题**

连接到网站时，设置请求头通常会很有用。一个例子是在标题中设置“用户代理”字段。设置这些标题是一个很好的做法，这样网站所有者就可以看到客户端请求的细节。对上面的代码进行一些修改，以允许设置这样的头值。

这段代码让你连接到一个网页，并检索网页的内容。下面这个简单的代码可以让你打印网站内容，作为一个起点。但是，在检索到响应正文之后，您可以做很多事情，比如解析所有链接(web crawler 的主要用例)或统计页面中的单词数(用于创建搜索索引)等。

```
_, err = io.Copy(os.Stdout, response.Body)
```