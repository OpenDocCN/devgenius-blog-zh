# Web 应用程序性能优化— HTTP 缓存

> 原文：<https://blog.devgenius.io/web-application-performance-optimization-http-caching-791eeda4509e?source=collection_archive---------5----------------------->

## HTTP 缓存的最佳实践是什么？

![](img/77dd46438c2cedeb87efa7c027bbcf78.png)

照片由[丹尼·米勒](https://unsplash.com/@redaquamedia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

本文将介绍 web 应用程序性能优化的一个非常重要的部分——HTTP 缓存。

有了 HTTP 缓存，我们甚至可以在不建立网络连接的情况下加快资源获取的速度。这不仅提高了应用程序的响应速度，还减轻了服务器的压力。

HTTP 缓存可以分为两类，一类是强缓存，另一类是协商缓存。

# 强大的缓存

如果有有效的强缓存，那么浏览器不会发送真正的请求，而是直接读取本地缓存。在 Chrome 中，本地存储位置可能是磁盘缓存或内存缓存。同时，HTTP 状态代码是 200。

有三个响应头可用于设置强缓存，即 Expires、Cache-Control 和 Pragma。

## 期满

它是在 HTTP/1.0 中添加的。它的值被设置为日期。当再次发出相同的请求时，浏览器会将系统时间与 Expires 值进行比较。如果超过，缓存将失效，请求将重新启动。否则，将使用缓存。

但是系统时间有很大的不确定性，会导致缓存时间不准确。

## 缓存控制

它是在 HTTP/1.1 中添加的。它的值更复杂，可以设置多个指令值:

*   `max-age`:`max-age=N`表示响应产生后[到 *N* 秒前](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#freshness)保持新鲜。
*   `s-maxage`:它还指示响应在多长时间内[保持](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#freshness)新鲜(类似于`max-age`)——但它是特定于共享缓存的，当`max-age`出现时，它们会忽略它。
*   `no-cache`:表示响应可以存储在缓存中，但每次重用前必须经过原服务器验证。并不是说“不缓存”。
*   `no-store`:这意味着任何类型的缓存(私有的或共享的)都不应该存储这个响应。
*   `immutable`:表示响应在[新鲜](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#freshness)时不会更新。
*   `public`:表示响应可以存储在共享缓存中，如中间代理、CDN 等。
*   `private`:表示响应只能存储在私有缓存中(例如浏览器中的本地缓存)。

除了这些，还有`must-revalidate`、`proxy-revalidate`、`must-understand`、`no-transform`等指令。关于他们的更多信息，你可以查看一下。

## 杂注

它用于在没有 Cache-Control HTTP/1.1 头的情况下向后兼容 HTTP/1.0 缓存。它的语法只能是`Pragma: no-cache`。和`Cache-Control: no-cache`有异曲同工之妙。

当两者都存在时，`Cache-Control`优先于`Expires`使用。和`Pragma`应该仅用于向后兼容。

# 协商缓存

如果强缓存未使用或已过期，将尝试协商缓存。协商的缓存将发送真正的请求，但只携带一些信息到服务器进行验证。如果它仍然有效，将直接使用本地缓存。同时，HTTP 状态代码是 304，没有任何主体。

设置协商缓存可以使用 Last-Modified 和 ETag 响应头。

## 上次修改/如果修改-自

它是在 HTTP/1.0 中添加的。`Last-Modified`设置在响应头中，`If-Modified-Since`设置在请求头中。它们的值表示响应内容的最后修改时间，精确到秒。

使用方法:当发出第一个请求时，服务器会将`Last-Modified`添加到响应头中。浏览器再次启动时，会将`Last-Modified`的值作为`If-Modified-Since`的值，与服务器协商是否有变化，如有变化则重新下载。

但是不够精确。例如，当文件的修改时间小于一秒时，`Last-Modified`的值不变，这也导致浏览器使用过期的缓存。或者，如果修改后的文件内容与原文件一致，浏览器会丢弃缓存，重新下载。

## ETag/如果不匹配

它是在 HTTP/1.1 中添加的。类似于`Last-Modified/If-Modified-Since` , `ETag/If-None-Match`分别是响应头和请求头。它们的值代表该响应内容的标识符，类似于文件指纹。拥有唯一的内容标识符解决了我上面提到的`Last-Modified/If-Modified-Since`缺陷。所以浏览器会更喜欢用`ETag/If-None-Match`。

此外，`ETag/If-None-Match`还支持强验证器和弱验证器来满足不同的情况，可以通过`'W/'`来设置，详见 [MDN](https://developer.%20mozilla.org/en-US/docs/Web/HTTP/Headers/ETag#directives) 。

# 结论

当面临强缓存或协商缓存时，浏览器客户端更喜欢使用“较新”版本的 HTTP 响应/请求头。例如，优先使用`ETag/If-None-Match`而不是`Last-Modified/If-Modified-Since`。

在实践中，根据不同的响应类型选择不同的缓存类型。例如，不常改变的资源倾向于使用强缓存。如果您的 web 应用程序是 [SPA](https://en.wikipedia.org/wiki/Single-page_application) ，那么根条目的 HTML 通常被设置为协商缓存，而那些 JS 和 CSS 文件通常被设置为具有足够大的时间值的强缓存。这是因为那些 JS 和 CSS 文件名的后缀通常由打包工具(例如 Webpack)添加到文件哈希中，并且每次更新时，根条目 HTML 将指向最新的 JS 和 CSS 文件。

网络条件不可靠，所以 HTTP 缓存可以大大提高应用程序性能，尤其是当响应内容很大时。那么你在使用 HTTP 缓存吗？欢迎和我分享你的想法！

*感谢阅读。如果你喜欢这样的故事，想支持我，请考虑成为* [*中等会员*](https://medium.com/@islizeqiang/membership) *。每月 5 美元，你可以无限制地访问媒体内容。如果你通过* [*我的链接*](https://medium.com/@islizeqiang/membership) *报名，我会得到一点佣金。*

你的支持对我来说非常重要——谢谢。