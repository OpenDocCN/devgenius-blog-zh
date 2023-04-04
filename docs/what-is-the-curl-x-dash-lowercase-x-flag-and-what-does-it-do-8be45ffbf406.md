# 什么是 curl -x(破折号小写 X)标志，它有什么作用？

> 原文：<https://blog.devgenius.io/what-is-the-curl-x-dash-lowercase-x-flag-and-what-does-it-do-8be45ffbf406?source=collection_archive---------5----------------------->

![](img/f49f044e9478a00a0b84c05e7404e38f.png)

[Shamin Haky](https://unsplash.com/@haky) 在 [Unsplash](https://unsplash.com/photos/RIk-i9rXPao) 上拍摄的照片。

如果您在这里，那么您可能想知道您正在使用或正在分析的某个`curl`调用中的`-x`标志是什么。

让我们举一个例子来分析一下。

```
curl -x http://10.1.23.4:1234 https://supercoolamazingnicewebsite.com
```

我们在这里做的是使用 curl 访问一个网站，`https://supercoolamazingnicewebsite.com`，但是我们也使用前面提到的`-x`标志并在它后面指定这个神秘的`[http://10.1.23.4:1234](http://10.1.23.4:1234)`。

简单地说，我们告诉 curl 点击这个超级酷的网站，然后在端口`1234`上使用指定的代理`[10.1.23.4](http://10.1.23.4:1234)`。

然后，请求将通过代理到达指定的 URL。

除了`-x`，还可以用`--proxy`。这一点更清楚，并且将实现与`-x`完全相同的事情。不过，很多人似乎喜欢简洁，所以你可能会经常看到这个`-x`。

下面是来自 [curl 手册页](https://man7.org/linux/man-pages/man1/curl.1.html)的官方解释。

> 使用指定的代理。
> 
> 代理字符串可以用 protocol://前缀指定。没有指定协议，否则 http://将被视为 http 代理。使用 socks4://、socks4a://、socks5://或 socks5h://请求使用特定的 socks 版本。
> 
> socks 代理支持 Unix 域套接字。为主机部分设置 localhost。例如 socks 5h://localhost/path/to/socket . sock
> 
> 7.52.0 中为 OpenSSL、GnuTLS 和 NSS 添加了通过 https://协议前缀的 HTTPS 代理支持。
> 
> 从 7.52.0 开始，无法识别和不受支持的代理协议会导致错误。以前的版本可能会忽略该协议，而使用 http://来代替。
> 
> 如果代理字符串中没有指定端口号，则假定端口号为 1080。
> 
> 此选项会覆盖设置代理使用的现有环境变量。如果有一个环境变量设置了代理，您可以将 proxy 设置为""来覆盖它。
> 
> 通过 HTTP 代理执行的所有操作都将透明地转换为 HTTP。这意味着某些特定于协议的操作可能不可用。如果您可以使用— proxytunnel 选项通过代理建立隧道，则情况并非如此。
> 
> 代理字符串中可能提供的用户名和密码是 curl 解码的 URL。这允许您使用%40 传入特殊字符，如@或使用%3a 传入冒号。
> 
> 代理主机可以用与代理环境变量相同的方式指定，包括协议前缀( [http://)](/)/) 和嵌入的用户+密码。
> 
> 如果多次使用该选项，将使用最后一个选项。
> 
> 示例:
> 
> curl-proxy[http://proxy . example](http://proxy.example)https://example.com

希望你现在知道`-x`是什么意思了！不算太坏，对吧？

如果你觉得这篇文章很有帮助或者只是喜欢阅读它，可以考虑[注册成为](https://tremaineeto.medium.com/membership)的媒体会员。

每月 5 美元，你可以无限制地阅读媒体上关于软件、技术等主题的报道。

如果你用我的链接注册，我会得到一小笔佣金。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入 Medium—Tremaine Eto

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)