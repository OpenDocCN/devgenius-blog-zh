# 解析一个 HTTP 请求

> 原文：<https://blog.devgenius.io/dissection-of-an-http-request-906ca90b70cc?source=collection_archive---------3----------------------->

![](img/6747442a14f2852fab66fa9d85a0cf64.png)

照片由[沙哈达特·拉赫曼](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

超文本传输协议又称 HTTP，是用于在整个互联网上发送和接收数据的协议。HTTP 请求是一台计算机通过使用 HTTP 协议发送给另一台计算机的消息。负责发出 **HTTP 请求**的实体被称为**客户端**，即您的浏览器。而向我们发回针对 HTTP 请求的 **HTTP 响应**的实体被称为**服务器**。

该协议在进行 API 调用时非常方便。当客户端(大多数情况下是您的浏览器)向服务器发出 API 调用时，会向客户端发回一个响应。响应包含客户端请求的数据，这通常被称为**资源**。

HTTP 协议有一个名为 **HTTPS 的扩展。**与 HTTP 不同，当使用 HTTPS 时，客户端和服务器之间的所有通信都被**加密**。HTTPS 确保客户端和服务器之间发送的所有数据都使用 SSL 证书加密。

## HTTP 请求组件

HTTP 请求由三部分组成。这些是:

*   请求行
*   标题
*   主体(可选)

## 请求行

请求行进一步分为三个部分。这些是，

*   一种方法。方法就像我们用来与服务器交互的命令。它告诉服务器它与资源有什么关系。例如，GET 方法从服务器检索资源，而 POST 方法用于向服务器发送资源。总共有 9 种 HTTP 方法，最流行的是 GET、POST、PUT、PATCH 和 DELETE。
*   资源的 URL 路径。资源的 URL 路径遵循方法。URL 路径标识服务器的资源。该 URL 实际上是相对于基本 URL 的。例如，考虑资源的 URL 路径是`/api/sign-up`，让我们假设它相对于一个基本 URL`https://xyz.com`，导致完整 URL 是`https://xyz.com/api/sign-up`
*   HTTP 协议的一个版本。

请求行的示例如下:

`POST /api/sign-up HTTP/1.1`

在上面的例子中，`POST`是指定服务器应该做什么的方法。`/api/sign-up`是服务器上的资源所在的 URL 路径。`HTTP/1.1`是正在使用的 HTTP 协议的版本。

# 头球

HTTP 头用于通过 HTTP 请求传递附加信息。HTTP 头由不区分大小写的名称、冒号、`:`和值组成。例如:

`Host : www.google.com`

在上面的例子中，`Host`是头的名字，而`www.google.com`是它的值。主机头用于指示服务器的域。

`Content-Type : application/json`

上面的头示例是`Content-Type`头，它指示请求体的媒体类型。值`application/json`指定请求体是 JSON 格式的。

## 身体

对于 HTTP 请求，请求正文是可选的。它包含我们将要发送到服务器的数据。请求正文可以包含任何内容，从用户的登录详细信息(如电子邮件和密码)到从调查表的输入字段收集的数据，再到用户上传的任何图像或文件。

如果请求体被添加到 HTTP 请求中，那么通常`Content-Type`和`Content-Length`头也被添加到 HTTP 请求的 headers 部分，以便指定请求体的性质。

让我们看一些例子。

`HelloWorld`

在上面的例子中，主体只包含一个文本`HelloWorld`。请求的主体可以包含一个简单的文本到 JSON 格式的更结构化的数据，如下所示:

```
{
    "firstName": "Rabi",
    "lastName": "Siddique",
    "homeTown": "Gujranwala" }
```

在上面的例子中，`Content-Type` header 可以用来指定请求体中包装的数据类型。因为数据是 JSON 格式的，所以`Content-Type`头的值将是`application/json`而`Content-Length`头的值将等于该数据的总大小。

## HTTP 请求的完整示例

```
POST /api/sign-up HTTP/1.1
Host: www.xyz.com
Content-Type: application/json

{   
    "firstName": "Rabi",
    "lastName": "Siddique",
    "email": "yabbadabba@xyz.com", 
    "password": "XXXXXXXXX"
}
```

上面的例子包含了 HTTP 请求的所有元素，请求行、头部和主体。请求行`POST /api/sign-up HTTP/1.1`使用 HTTP 版本 1.1 指定了通过 URL `api/sign-up`定义的资源的`POST`方法的使用。请求中使用的头是`Host`和`Content-Type`，而请求体包含一些 JSON 格式的用户数据。

## HTTP 响应

如开头所示，当客户机向服务器发出 HTTP 请求时，服务器会向客户机发回 HTTP 响应。HTTP 响应的结构类似于 HTTP 请求。

HTTP 响应包括:

*   状态行
*   标题
*   正文(可选)

在 HTTP 响应的情况下，消息头和消息体的工作方式与它们在 HTTP 请求中的工作方式相同。唯一的区别在于状态行。对于 HTTP 响应，请求行被替换为状态行。
状态行告诉我们 HTTP 请求的状态，即成功还是失败。状态行就像请求行一样，由 3 部分组成。这些是:

*   HTTP 协议版本
*   状态代码
*   状态文本

举个例子，

`HTTP/1.1 200 OK`

其中`HTTP/1.1`是 HTTP 协议版本，`200`是指示我们的 HTTP 请求状态的状态代码，`OK`是状态文本。
你可以在这里阅读更多关于状态码及其含义的信息[。](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

感谢您阅读帖子。:)

我们来连线:
[LinkedIn](https://www.linkedin.com/in/rabi-siddique-b6b4971a0/)
[Twitter](https://twitter.com/rabisiddique234)