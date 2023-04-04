# curl 命令中的-X 标志是什么？

> 原文：<https://blog.devgenius.io/what-is-the-x-aka-request-flag-in-a-curl-command-a489b02346d4?source=collection_archive---------15----------------------->

![](img/bc4f31181b10b289e9e3937f309b8326.png)

凯文·霍尔瓦特在 [Unsplash](https://unsplash.com/photos/Pyjp2zmxuLk) 上的照片。

见过 curl 命令中的`-X`标志吗？

你可能有过——这是比较常见的一种，毕竟，你是在这里读这篇文章的，不是吗？

好吧，让我们直奔主题吧。

# 手册页上写了什么

如果您看一下手册页上的内容，它会告诉您:

*(HTTP)指定与 HTTP 服务器通信时使用的自定义请求方法。将使用指定的请求方法，而不是其他使用的方法(默认为 GET)。请阅读 HTTP 1.1 规范，了解详细信息和解释。常见的附加 HTTP 请求包括 PUT 和 DELETE，但 WebDAV 等相关技术提供了 PROPFIND、COPY、MOVE 等。*

*通常你不需要这个选项。各种 GET、HEAD、POST 和 PUT 请求都是通过使用专用命令行选项来调用的。*

这个选项仅仅改变了 HTTP 请求中实际使用的单词，它并没有改变 curl 的行为方式。因此，举例来说，如果您想要进行适当的 HEAD 请求，使用-X HEAD 是不够的。你需要使用 [*-I，— head*](https://curl.se/docs/manpage.html#-I) *选项。*

*您用* [*-X，— request*](https://curl.se/docs/manpage.html#-X) *设置的方法字符串将用于所有请求，例如，如果您使用* [*-L，— location*](https://curl.se/docs/manpage.html#-L) *可能会导致意想不到的副作用，因为 curl 没有根据 HTTP 30x 响应代码更改请求方法，等等。*

*(FTP)指定使用 FTP 创建文件列表时使用的自定义 FTP 命令，而不是 LIST。*

*(POP3)指定一个自定义 POP3 命令来代替 LIST 或 RETR。*

*(IMAP)指定要使用的自定义 IMAP 命令，而不是 LIST。(在 7.30.0 中添加)*

*(SMTP)指定一个自定义 SMTP 命令来代替 HELP 或 VRFY。(在 7.34.0 中添加)*

*如果多次提供* [*-X，—请求*](https://curl.se/docs/manpage.html#-X) *，将使用最后一次设定值。*

*例子:*

```
curl -X "DELETE" https://example.com
 curl -X NLST ftp://example.com/
```

…老实说，这对您来说已经足够好了！

但如果不是，那么我将简要地用我自己的话来表达它的真正含义。

# 另一种解释

基本上，使用`-X`的第一种方式是作为一个标签，你可以添加到 curl 中，紧接着是一个 HTTP 方法(例如，`GET`、`DELETE`、`POST`、`PUT`等等)，然后是一个 url。

因此，您所做的就是指定一个 HTTP 方法，通过该方法针对您指定的 URL 执行请求。

例如，对于`POST`，您可能想要添加一个主体来实际发布到 URL。

在这种情况下，您实际上需要使用不同的标志:`-d`。

```
curl -X POST https://stuffinla.com/
   -H 'Content-Type: application/json'
   -d '{"user":"user_id","password":"hunter123"}'
```

你还可以看到另一面旗帜`-H`。那个用来指定你将要发布的请求头。

但是我们正在远离这里的事情。

看到那个`-X`了吗？这就是我们在本文中讨论的标志，您将看到`POST`紧随其后。那是因为我们正在向那个 URL `[https://stuffinla.com](https://stuffinla.com.)` [发帖。](https://stuffinla.com.)

您不需要在 HTTP 方法(例如`"POST"`)周围加上引号，但是您也可以这样做。

除了 HTTP 方法之外，`-X`支持的另一件事是指定您希望用来访问 URL 的协议类型。

因此，您可以说类似于`-X IMAP`的话来使用 IMAP 协议访问 URL。例如，您也可以对像`SMTP`这样的协议做同样的事情。

不过，对你们大多数人来说，你们可能只是想使用`-X`的第一种用法。

还有，还有一件事！不使用标志`-X`，也可以使用`--request`。这是完全一样的东西，但是形式更长(也许更令人难忘)。

如果你觉得这篇文章很有帮助或者只是喜欢阅读它，可以考虑[注册成为](https://tremaineeto.medium.com/membership)的一名媒体成员。

每月 5 美元，你可以无限制地阅读媒体上关于软件、技术等主题的报道。

我个人付费成为会员，能够每天查找特定的技术文章是非常宝贵的。

如果你用我的链接注册，我会得到一小笔佣金。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入 Medium—Tremaine Eto

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)