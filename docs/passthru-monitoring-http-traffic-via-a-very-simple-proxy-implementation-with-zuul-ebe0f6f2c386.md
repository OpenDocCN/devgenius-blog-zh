# Passthru:使用 Zuul 通过一个非常简单的代理实现来监控 HTTP 流量

> 原文：<https://blog.devgenius.io/passthru-monitoring-http-traffic-via-a-very-simple-proxy-implementation-with-zuul-ebe0f6f2c386?source=collection_archive---------1----------------------->

![](img/47235b09f2df088638fd4c4d97097f62.png)

曾几何时，有一个名为 [TCPMon](https://ws.apache.org/tcpmon/) 的工具用于检查 HTTP 流量，对于调试 web 服务(SOAP 和其他 HTTP 服务)非常方便。基本上，它充当一个代理，将流量转发到目标服务，同时记录请求/响应并通过 Swing UI 显示。这是我参与的许多项目的救命稻草。

不幸的是，TCPMon 不再被维护，而且在容器化的环境中使用也不太方便。此外，我找不到一个简单的即插即用的替代品来达到同样的目的。当然，像 Nginx 或 HAProxy 这样的代理可以很容易地监控和记录请求，但我发现它们在配置开发时并不方便。(老实说，我也发现 Lua 配置有点令人生畏)

# 第一次尝试:Servlet

所以我决定尝试使用 Spring Boot+科特林实现一个非常简单的代理。我最初的想法是实现一个简单的 servlet，将请求转发到目标主机(作为环境变量传递，所以在 Docker compose 环境中也很容易配置)。当流量通过时，servlet 会将请求记录到控制台。所以我把它命名为 ***直通*** 。

Passthru Servlet 应该处理传入的 Http 请求，重新创建、分派、处理响应并记录处理所有编码、头、参数和有效负载的内容。我对自己实现所有这些并不太放心。这些应该已经被比我更有时间和承诺的人解决了。

# Zuul Gateway

因此，我继续挖掘，发现了网飞的作品。我听说 Zuul 是 API 网关，但没有机会尝试。基本上，Zuul 是一个应用级网关，幸运的是，它可以直接与 Spring 集成。此外，Zuul 提供了 RequestContext 对象，使开发人员能够存储请求范围的数据(在线程本地对象中)。

我创建了一个 PassthruContext 类来记录请求响应信息，如下所示:

我需要做的一切似乎很简单:

1.  实现一个预过滤器来拦截请求并将其记录在上下文中。
2.  实现一个后置过滤器来处理响应并在上下文中记录。
3.  记录 PassthruContext(到控制台)

当然，它并不像我开始预期的那样顺利。在路上我必须克服一些障碍。在接下来的部分中，我将回顾这些步骤以及我必须解决的小故障。

# 预过滤器

这一部分非常简单，我所要做的就是提取请求方法、头和有效载荷，并将它们保存在上下文中。你可以看到下面的代码。

# 后置过滤器

在阅读了上一节之后，人们可以很容易地认为处理响应也很简单。我们将解析响应头、主体和状态代码，对吗？嗯，没那么快。在响应处理过程中，我们需要解决许多问题

## 使用响应输入流

关于 Java 输入流的一个不太有趣的事情是，如果您使用了一次输入流，就不能保证您能够重绕它。这是因为有些 InputStream 实现是缓冲的，而有些实现不是。对于 Zuul，可以重新读取请求输入流；但不是 Zuul 响应输入流

因此，一种方法是将请求读入缓冲区(即字节数组)；从原始输入流构造另一个输入流，并用新的输入流替换原始输入流。Zuul 还在上下文中提供了 responseBody 字段；因此，一旦解析了响应，也可以将主体设置为字符串。你可以看到下面的片段。

## Gzip 内容

在记录请求内容时，我们还必须处理另一个细节。现代 HTTP 交换内容以 Gzip 格式压缩，而不是纯文本格式。因此，我们将要使用的响应输入流很可能是在 Gzip 中。所以我们需要在记录之前提取 Gzip 内容，并把它作为输入流放回去。

你可以在下面看到我们如何在后置过滤器中做到这一点。

最后，我们的 post 过滤器看起来像这样:

## 带主体的 HTTP GET

到目前为止，我们能够处理大多数 HTTP 请求，这很好。如果我们处理有主体的 GET 请求(比如 Elasticsearch 查询)，我们需要考虑一个更微妙的问题。GET 请求中的 Body 是一个有争议的话题，许多实现都不支持它。不幸的是，Zuul 是在 HTTP GET 中省略主体的实现之一。在路由阶段，SimpleHostRoutingFilter 在传出的 HttpRequest 中忽略 GET 请求体。

我们可以通过以下步骤解决这个问题:

1.  禁用 *SimpleHostRoutingFilter*
2.  实现我们自己的路由过滤器并替换被禁用的过滤器。

我们可以禁用 *SimpleHostRoutingFilter* ，包括应用程序中的以下行

```
zuul.SimpleHostRoutingFilter.route.disable: true
```

然后，我们需要实现自己的路由过滤器，从现有的(禁用的)过滤器扩展而来。

这里我们创建了一个能够封装实体的 HttpRequest 对象。其余的代码是样板文件，复制了基本实现中的一些私有方法/字段。

## 将流量记录到控制台

这部分很简单。为了简洁起见，我将跳过这一部分。可以参考来源[代号](https://github.com/itasyurt/passthru)。

## 应用程序属性

我们有以下应用程序属性

```
zuul.routes.passhtru.url: ${passthru.target}
zuul.routes.passhtru.path: /**
zuul.SimpleHostRoutingFilter.route.disable: true

ribbon.eureka.enabled=false
```

前两行是将所有流量路由到目标服务器，其中第三行禁用 SimpleHostRoutingFilter，最后一行禁用 Eureka。

## 结论

在这篇文章中，我试图总结我使用 Spring Boot+Zuul +Kotlin 实现一个简单代理服务器的经验。你可以在这个 [repo](https://github.com/itasyurt/passthru) 中找到完整的源代码。请注意，这是一个非生产质量的努力，旨在自我学习和探索。