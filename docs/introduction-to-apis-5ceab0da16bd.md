# API 简介

> 原文：<https://blog.devgenius.io/introduction-to-apis-5ceab0da16bd?source=collection_archive---------10----------------------->

## 应用程序编程接口(API)是编程语言中可用的构造，允许开发人员轻松创建复杂的功能。它们从你那里抽象出更复杂的代码，提供一些更简单的语法来代替它。

![](img/c19c63cbfc4b0a28b2fadaabfbe50ddd.png)

插头插座 API:你不是电工

> *“使用 API 和编写更高级的代码(如 JavaScript 或 Python)总是比试图直接编写低级代码(如 C 或 C++)来直接实现您想要的功能更容易。”*

# 基于 Web 的 API

特别是客户端 JavaScript 有许多可用的 API，为您在 JavaScript 代码中使用提供了增强的功能。它们可以分为以下几类:

*   **浏览器 API**:内置于网络浏览器，能够从浏览器和周围的计算机环境中暴露数据。
*   **第三方 API**:从网络上的其他地方检索数据。

## 浏览器 API

当编写 web 代码时，有许多可用的[浏览器 APIs】，它们通常与 JavaScript 一起使用。浏览器有可用的 API，比如](https://developer.mozilla.org/en-US/docs/Web/API)[地理定位 API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API) ，或者，在我看来，是最常用的 API 之一，即[获取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 。

它们都提供了接口。例如，*地理定位 API* 允许用户向 web 应用程序提供他们的位置，而*获取 API* 提供了一个通过网络获取资源的接口，与它的前辈( [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) )相比，它具有更强大和更灵活的功能子集。

## 第三方 API

如今，很少有应用程序(如果有的话)是独立的。您很可能需要构建内部 API 或使用第三方 API 来避免重复劳动。随着应用程序变得越来越大，越来越受欢迎，其他人会对利用您的技术或数据感兴趣，这将为您扩大应用程序的影响力创造机会。

> API 不再仅仅是为你的应用提供动力的工具，而是他们自己的产品或平台

随着 API 在技术领域迅速流行，我们已经看到在过去的二十年里它的可用性有了巨大的增长(参见[马拉默德的《分析 1959 年以来的 Novell 网络》](https://babel.hathitrust.org/cgi/pt?id=mdp.39015018454903&view=1up&seq=314&skin=2021))。让我们快速突出两个第三方 API 的例子:

*   [NASA API](https://api.nasa.gov/):让 NASA 数据，包括图像，对应用开发者来说非常容易获取。
*   [公共 API](https://github.com/public-apis/public-apis):用于软件和 web 开发的免费 API 的集合列表。

让我们快速浏览一下 NASA 的 API。NASA 最受欢迎的网站之一是[每日天文图片](http://apod.nasa.gov/apod/astropix.html)(事实上，涵盖所有联邦机构)。美国宇航局声称它像贾斯汀比伯的视频一样受欢迎。APOD(每日天文图片)API 端点(稍后将回到本术语)构建影像和相关元数据，以重新用于其他应用程序。用 NASA 的演示钥匙在这里尝试一下[(稍后还会回到这个学期)。只要确保为你的浏览器安装一个 JSON 格式化程序，以便更容易理解数据，我推荐](https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY) [JSONView](https://jsonview.com/) 。

**我们可以一起使用浏览器和第三方 API 吗？** 是的，我们可以！让我们使用前面提到的浏览器的 Fetch API，创建一个请求来获取 NASA 当天的天文图片。这里有一个快速[代码笔](https://codepen.io/ekqt/pen/yLpqqjP?editors=1011)重建他们的 APOD 网站。幕后发生了什么？一个基本的获取请求很容易设置。

在上面的例子中，我们使用最简单形式的`fetch()`,它接受一个参数——您想要的资源——并且不直接返回 JSON(稍后将详细介绍)响应体，而是返回一个代表整个 HTTP 响应的[响应](https://developer.mozilla.org/en-US/docs/Web/API/Response)对象。所以要获得 JSON 响应体内容，需要使用`json()`方法，该方法将解析响应体文本的结果作为 JSON 返回。你可以在这里阅读更多关于使用获取 API 的信息[。](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

# 网络是如何工作的

在我们继续前进之前，让我们先来探索一些关于网络如何工作的基本概念。当您从电脑或手机上的网络浏览器查看网站时会发生什么。

## 到底发生了什么？(一个过于简单的故事)

当您在浏览器中键入网址时:

1.  浏览器去 [DNS](https://developer.mozilla.org/en-US/docs/Glossary/DNS) (域名系统)服务器，找到网站所在服务器的真实地址(网站的 IP 地址)，然后
2.  浏览器向服务器发送一个 [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) (超文本传输协议)请求，要求服务器将网站发送回客户端(浏览器)，然后
3.  如果服务器成功地接受了请求，它将用一个“ [200 OK](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses) ”消息响应，并将网站发送回客户端，最后
4.  浏览器会为您显示网站。

**DNS 上的一个小提示** 真实的网址并不是你在地址栏中输入的用来查找你最喜欢的网站的漂亮且容易记忆的字符串。它们是看起来像这样的特殊数字:63.245.215.20。这些被称为 [IP 地址](https://developer.mozilla.org/en-US/docs/Glossary/IP_Address)，它们代表网络上的一个唯一位置。DNS 服务器将人类友好的域(如 mozilla.org)转换为数字 IP 地址(如 151.106.5.172)。

从技术上讲，网站可以通过 IP 地址直接访问。您可以尝试使用类似于 [IP Checker](https://ipinfo.info/html/ip_checker.php) 的工具复制 DNS 请求。

# 什么是超文本传输协议(HTTP)？

HTTP 是获取资源(如 HTML)的协议。它是为 web 浏览器和 web 服务器之间的通信而设计的，遵循[无状态](https://en.wikipedia.org/wiki/Stateless_protocol)和简单的[客户端-服务器模型](https://en.wikipedia.org/wiki/Client%E2%80%93server_model)。

**让我们做一个简单的 HTTP 请求** 发送一条 HTTP 消息:让我们试着用 NASA 的 [APOD API](https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY) 检查一下幕后发生的事情。要在浏览器上发出此请求: *1)* 打开一个新标签， *2)* 从浏览器打开 DevTools，然后查看网络请求， *3)* 访问 URL。

查看服务器发送的响应

我们可以使用 API 平台工具复制上述请求，如 [Postman](https://www.postman.com/product/what-is-postman/) : *1)* 转到 [Postman 的 web 应用程序](https://web.postman.co/)， *2)* 使用您的凭据创建一个帐户或登录并创建一个基本的 HTTP 请求， *3)* 发送一个带有 APOD API 的 URL[https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY](https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY)的“GET”请求。

## 识别网络上的资源

HTTP 请求的目标被称为“资源”(一个文档、一张照片、任何其他东西)。每个资源都由一个统一资源标识符(URI)来标识(URL 是 URIs 最常见的形式，被称为 web 地址)，在整个 HTTP 中用于标识资源。让我们来看一些 URL 的例子。

在这里，你可以找到更多关于 URL 结构的信息。

# 休息——好的和坏的

表述性状态转移(REST)是一种架构风格，它指导流程的设计和开发与 web 上的资源进行交互。符合大部分或全部[指导约束](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)的 API 被认为是 *RESTful API* 更全面的指导方针请参考[这里](https://github.com/byrondover/api-guidelines/blob/master/Guidelines.md)。

REST 利用了 HTTP 的所有优势，如[请求动词](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)、 [URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Resources_and_URIs) 、[媒体类型](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)、[缓存](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)等。由于 REST 服务像普通网站一样工作，与其他风格的 web 服务相比，它们更容易创建和使用。

每个 RESTful API 都以 [Fielding](https://en.wikipedia.org/wiki/Roy_Fielding) 所称的**空样式**开始。这一切都始于作为一个整体的系统需求*，没有约束*，然后逐步识别并应用约束到系统的元素，以允许影响系统行为的力量自然流动，与系统和谐一致。这种风格强调约束和对系统上下文的理解。因此，让我们快速了解一下我们应该从它的指导约束中了解什么。

## 客户端-服务器

*   **什么事？** — [关注点分离](https://en.wikipedia.org/wiki/Separation_of_concerns)是这个约束背后的驱动原则，将用户*界面*(客户端)与*数据存储*(服务器)分离开来。
*   **好处** —通过这样做，我们提高了*跨多个平台的可移植性*，并通过简化服务器组件提高了*可伸缩性*。这种分离允许每个组件独立发展。

## 无国籍的

*   **什么事？** —客户端-服务器通信本质上必须保持*无状态*，使得每个请求必须包含理解请求所需的所有信息(并且不能依赖于服务器上存储的任何上下文)。
*   **Good**—*Visibility*得到了改善，因为系统不必看得比请求本身更远就能确定其性质。*可靠性*得到了提高，因为它简化了从部分故障中恢复的任务。最后，*可伸缩性*得到了改善，因为它允许服务器为额外的请求快速释放资源，并且简化了实现，因为它不必管理跨请求的资源使用。
*   **不好的** —缺点是它可能会通过增加在一系列请求中发送的重复数据(每交互开销)来降低*网络性能*，因为这些数据不能留在共享上下文中的服务器上。此外，将应用程序状态放在客户端*减少了服务器对一致的应用程序行为的控制*，因为应用程序变得依赖于跨多个客户端版本的语义的正确实现。

## 隐藏物

*   **什么事？** —添加缓存组件，形成 REST 中的*客户端-缓存-无状态-服务器*架构风格。高速缓存充当客户端和服务器之间的中介，其中如果认为对先前请求的响应可高速缓存，则可以在响应稍后的请求时重用这些响应，如果请求被转发到服务器，则这些响应等同于并可能导致与高速缓存中的响应相同的响应。
*   **好的** —有可能部分或完全*消除交互*，提高*效率*和用户感知的*绩效*。
*   **坏的** —如果缓存中的陈旧数据与请求被直接发送到服务器时获得的数据有很大不同，缓存会降低可靠性。

## 统一界面

*   **什么事？** —组件接口之间的通用性或一致性的应用。
*   **好的** —简化了整体系统架构，提高了交互的可见性。实现与它们提供的服务是分离的，这鼓励了独立的可发展性。
*   **不好的** —它降低了效率，因为信息是以一种标准化的形式传输的，而不是一种特定于应用程序需求的形式。

## 分层系统

*   **什么事？** —由包含组件行为的分级层组成的体系结构，使得每个组件不能*看到与其交互的直接层之外的*。
*   **好的** —层可用于*封装*遗留服务，以及*保护*新服务不受遗留客户端的影响。通过支持跨多个网络和处理器的服务负载平衡，中介体还可以用来提高系统的可伸缩性。
*   **不好的** —它们增加了数据处理的开销和延迟，降低了用户感知的*性能*。对于支持缓存约束的基于网络的系统来说，这可以通过在中介处共享缓存的好处来弥补。将共享缓存放置在组织域的边界可以带来显著的性能优势。

# 结论

有关设计、构建和维护 RESTful APIs 的更多详细解释，请参见[此处](https://github.com/byrondover/api-guidelines/blob/master/Guidelines.md)。

感谢阅读。

本文原贴[此处](https://ekqt-blog.vercel.app/)。