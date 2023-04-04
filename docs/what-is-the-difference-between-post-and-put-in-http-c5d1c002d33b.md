# HTTP 中 POST 和 PUT 有什么区别？

> 原文：<https://blog.devgenius.io/what-is-the-difference-between-post-and-put-in-http-c5d1c002d33b?source=collection_archive---------5----------------------->

## 有人说 POST 是用来修改资源的，PUT 是用来创建新资源的。也有人说应该反过来。事实上，这些都不完全正确。

![](img/fc61165e6fafa376c21acc4f199dd8a7.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [CardMapr.nl](https://unsplash.com/@cardmapr?utm_source=medium&utm_medium=referral) 拍摄的照片

最近有人问我 RESTful API 的定义，所以我也提到了标题中的问题。我一开始给的答案是 POST 方法是修改资源，PUT 方法是新建资源，最后在网上发现了一些不同的答案。

先说 RESTful API，这是一种相对成熟的针对互联网应用的 API 设计理论。它是由[罗伊·托马斯·菲尔丁](https://en.wikipedia.org/wiki/Roy_Fielding)在 2000 年提出的，是[表象状态转移](https://en.wikipedia.org/wiki/Representational_state_transfer)的简称。如果一个架构符合 REST 原则，它就被称为 RESTful 架构。

那么什么是休息原则呢？

首先要明确的是，每个[统一资源标识符(URI)](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) 代表一个资源，它是网络上的一个实体，或者具体的信息。例如 HTML 文档、图片、音频和视频等。，URI 指向资源的位置。然后，客户端和服务器可以简单地通过 URI 传递资源。在传输过程中，HTTP 动词(如 GET、POST、PUT、DELETE 等。)也被携带，并且这些动词还指示如何改变服务器资源的状态。比如 GET 用来获取资源，DELETE 用来删除资源。这也实现了*“表示层状态转换”*。

我们举个例子来说明，比如转账界面:

```
POST /account/1/transfer/100/to/2
```

对这个 URI 的一个典型误解是它包含动词。正如我们上面提到的，URIs 只代表资源，状态变化由 HTTP 动词(GET、POST、PUT、DELETE 等)反映。).

因此，我们需要将动词`transfer`改为名词`transaction`，并将信息放在请求体中:

```
curl -X 'POST' \
  '/transaction' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"from":1,"to":2,"amount":100}'  \
  --compressed
```

那么回到标题提出的问题，两个动词 POST 和 PUT 在资源上执行什么状态转换？

直接进入 [RFC 文档](https://www.rfc-editor.org/rfc/rfc9110.html#name-put)，上面写着:

> POST 和 PUT 方法之间的根本区别在于对封闭表示的不同意图。POST 请求中的目标资源旨在根据资源自身的语义处理封闭的表示，而 PUT 请求中的封闭表示被定义为替换目标资源的状态。因此，PUT 的意图是等幂的，对中介是可见的，即使确切的效果只有原始服务器知道。

从中提取的最关键的词是**幂等**。

那么什么是幂等呢？简单地说，如果一个相同的请求可以在一行中发出一次或多次，并且具有相同的效果，同时使服务器处于相同的状态，那么 HTTP 方法就是**幂等的**。换句话说，幂等方法不应该有任何副作用——除非这些副作用也是幂等的。正确实现后，`[GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET)`、`[HEAD](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD)`、`[PUT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT)`和`[DELETE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE)`方法是**幂等的**，而不是`[POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)`方法。

因此，PUT 方法意味着放置一个资源——用一个资源完全替换给定 URI 的资源，无论做多少次，结果都是一样的。比如一个可以放置资源的存储盒，每次调用都会完全替换里面的内容(无论是创建的还是更新的)。

POST 方法是指添加资源、修改资源、更新资源等。它不是幂等的。比如一个大的存储盒，每次调用都会给它增加资源，多次调用会不断填满。

所以简单断言 POST 方法是为了修改，PUT 方法是为了创建，或者反过来。这是不严谨的。应该根据动作是否是幂等的来做出选择。

你有什么看法？

# 参考

[1][https://stack overflow . com/questions/630453/post-and-put-in-http 的区别是什么](https://stackoverflow.com/questions/630453/what-is-the-difference-between-post-and-put-in-http)

*感谢阅读。如果你喜欢这样的故事，想支持我，请考虑成为* [*中等会员*](https://medium.com/@islizeqiang/membership) *。每月 5 美元，你可以无限制地访问媒体内容。如果你通过* [*我的链接*](https://medium.com/@islizeqiang/membership) *报名，我会得到一点佣金。*

你的支持对我来说非常重要——谢谢。