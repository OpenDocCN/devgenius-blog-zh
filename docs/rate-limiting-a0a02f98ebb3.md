# 限速

> 原文：<https://blog.devgenius.io/rate-limiting-a0a02f98ebb3?source=collection_archive---------3----------------------->

![](img/fb4c93462715828c1694405950876310.png)

[影像学分](https://unsplash.com/photos/dBI_My696Rk?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

## 设计一个 API 速率限制器来限制超过给定阈值的用户

# 漏桶算法

我喜欢把漏桶想象成沙漏里的沙子。不管你放多少沙子，它都会以恒定的速度滴下。

![](img/767e34e406a061911d2d64f0427a9b5e.png)

[图像学分](https://unsplash.com/photos/5Hl5reICevY?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

这个算法就是这样。每个用户都有一个独立的沙漏，当他们被大量并发请求淹没，填满玻璃桌面时，新的请求将被拒绝。恒定的滴沙速度平滑了*突发*请求，并以近似平均的速率处理它们。

您需要一个计数器来知道正在进行的*请求的数量，并需要一个队列来将传入的请求推入。队列(桶)的容量是您准备接受的“突发”流量。*

当请求进来时，计数器(每用户)需要增加，并且在请求处理后减少。我们可以将这些数据保存在 Memcahed/Redis 中。虽然这种算法提供了流量平滑和内存占用较小的优点，但它有几个限制。

*   它有两个参数(平均速率和脉冲串),这两个参数并不总是容易调整。
*   无论您选择什么(Memcached/Redis)，您都只能支持有限的原子操作，比如`GET`、`SET`和`INCR`。并且该算法需要多个不同的操作，这些操作不能自动完成。此外，建立一个锁定机制来执行这些操作会降低整个处理过程的速度，并且可能无法伸缩。
*   填满队列的旧请求会使最近的请求变得饥饿，并且不均匀的延迟会带来麻烦。

# 固定窗口算法

在这个算法中，系统使用一个固定的`n`秒的窗口。在给定的窗口中，如果请求在给定的阈值内，则在该给定的阈值之后，它们被允许和拒绝。这是一个简单的方法，解决了漏桶算法的两个局限性。首先，你不需要多重操作，只有`INCR`就够了。第二，它会丢弃旧的请求来服务新的请求。

然而，这提出了一个不同的问题。假设，我们有一个固定的窗口大小`1`秒，阈值是`5`。`5`请求在`0.59th`秒到达，`5`请求在`1.01th`秒到达。这种算法将允许这样的情况，这将导致处理两倍的请求率。

# 滑动窗口算法

![](img/a68f2c8605ad4a73aa350f5e66cc17ef.png)

[图像演职员表](https://unsplash.com/photos/qbP_kUO8tyY?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

这是对*固定窗口算法*的稍微修改。在这种方法中，假设当前请求来自于`1.2`秒。这意味着，当前窗口只完成了`20%`(考虑同样的`1`第二个窗口大小)。因此，我们将检查前一个窗口的最后一个`80%`。如果结合这两个部分的请求数量超过阈值，则该请求将被拒绝。该算法的这一关键差异使“突发”流量受到控制。

# 结论

滑动窗口算法可以证明比其他算法执行得更好，同时保持较小的内存占用。仅使用原子操作就可以使用 Redis/Memcached 来维护计数器。

但是，对于每个请求，我们都需要查询 Redis 服务器，这可能会阻塞服务器。更重要的是，它会降低合法查询的速度。为了缓解这个问题，一旦需要使用时间戳阻止客户端信息，每个服务器可以在本地维护客户端信息。在此之前，只查询本地存储的数据来拒绝请求，这肯定有助于减轻大规模攻击。

可以有更复杂的方法来进一步提高性能，但最终这都取决于您可以投入的时间和精力以及它为您的系统增加的价值。

# 参考

*   [漏桶—维基百科](https://en.wikipedia.org/wiki/Leaky_bucket)
*   [我们如何构建能够扩展到数百万个域的速率限制](https://blog.cloudflare.com/counting-things-a-lot-of-different-things/)
*   [速率限制的替代方法](https://www.figma.com/blog/an-alternative-approach-to-rate-limiting/)
*   [如何设计一个可扩展的速率限制算法](https://konghq.com/blog/how-to-design-a-scalable-rate-limiting-algorithm/)
*   [利用 Redis 排序集合实现更好的速率限制](https://engineering.classdojo.com/blog/2015/02/06/rolling-rate-limiter/)