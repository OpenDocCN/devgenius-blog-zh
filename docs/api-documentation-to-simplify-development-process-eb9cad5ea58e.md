# 简化开发过程的 API 文档

> 原文：<https://blog.devgenius.io/api-documentation-to-simplify-development-process-eb9cad5ea58e?source=collection_archive---------36----------------------->

![](img/a855876162af8382a94336918da5ee8f.png)

由[韦斯利·廷吉](https://unsplash.com/@wesleyphotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

在我作为软件工程师的职业生涯中，我总是需要从产品的前端或后端处理 API 端点。
在客户端，我点击一个现有的端点，并在需要时发送相关数据，而在服务器端，我不仅需要创建端点，还需要添加当有人点击这个端点时会发生什么的逻辑。

> 我从来没有真正注意到寻找处理当前端点的特定文件(在服务器端)是多么令人疲惫，直到我从事一个具有多个服务和代理连接的大型企业产品。

让我解释一下。
当我试图调试或实现一个新功能时，我发现自己在寻找客户端正在进行的 API 调用，以及它通过使用 Chrome 网络选项卡到达了哪个端点。一旦找到端点，我就转到我的 IDE，开始搜索处理当前端点的类/文件。

我使用的是 MVC 框架，所以它应该简化了这个过程，但是如果控制器文件有一个没有意义的坏名字，会发生什么呢？或者您正在开发一个庞大的企业产品，该产品有超过 3 个不同的服务通过代理连接，并且有如此多的控制器文件？或者某些端点的名称非常相似？

如果您拥有的唯一信息是客户端当前的 api 调用，那么很难找到这个端点被处理的位置。

> 我甚至无法开始解释这种文件搜索是多么令人疲惫和沮丧。

![](img/e4274861c70c03a608421487d578e353.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> 我发现自己每天花大约 45 分钟去找一个文件！

## **适可而止**

很快，我厌倦了所有其他开发人员似乎都在忍受的文件搜索过程，并开始思考如何简化这一过程，让我继续享受我的工作。

**第一步:**
由于我使用的是 Spring MVC 框架，我确信一定有办法处理它。我开始在网上挖掘，发现了一个非常基本和简单的 Spring 内置函数，它可以让我获得我的应用程序正在使用的所有`mapping routes`。

```
import org.springframework.web.method.HandlerMethod;import org.springframework.web.servlet.mvc.method.RequestMappingInfo;import org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping;private RequestMappingHandlerMapping handlerMapping;@Autowired
public void EndpointDocController(RequestMappingHandlerMapping handlerMapping) { this.handlerMapping = handlerMapping;}**Map<RequestMappingInfo, HandlerMethod> handlerMethods = this.handlerMapping.getHandlerMethods();**
```

由于`handlerMapping.getHandlerMethods()`返回当前`RequestMappingInfo`的所有信息，我不得不根据我的需要定制它，我只想要 `File Name
Request Type
url
Method Name
Return Type
Parameters`——如果有的话

**第二步:** 我的设计是，客户端不会得到所有现有的端点，而只会得到他们正在寻找的那个端点。为此，我必须创建一个新的端点来接受要查找的字符串。我最终这样做了

现在，我已经完成了背面的工作，是时候开始正面的工作了。
我创建了一个简单的 UI 页面，包含一个搜索输入和一个显示搜索结果的 div 包装器。我为网格布局和 UI 组件使用了 bootstrap。

我使用 Ajax(和 jQuery)来创建 API 调用并显示服务器响应

**步骤 4:** 此时，我有了一个可用的 API 文档，我可以在其中输入一个端点字符串/端点的一部分，我将获得与该字符串匹配的所有端点，最重要的是，我将拥有正在处理这些端点的**文件**。

![](img/edabe320b99f48fb05c0092bd2c50f44.png)

我正在开发的产品有多个微服务，它们使用代理相互通信，使用我编写的代码，我只能获得主服务和当前服务的端点信息。为了能够看到**所有**所有**服务的端点，我需要对我的前端代码做一个小小的改动。**

由于每个服务都有自己的 restful API —
服务 A 将指向[http://localhost:8080/service-A](http://localhost:8080/service-a)
服务 B 将指向[http://localhost:8080/service-B](http://localhost:8080/service-a)
服务 C 将指向[http://localhost:8080/service-](http://localhost:8080/service-a)C
等等……我需要能够选择我想要搜索的服务下面是完成这项工作的最终代码..

最终。jsp 文件

还有决赛。js 文件

> 请记住，您需要在每个服务上添加`apidoc`端点。

![](img/bb67e5e0af4fef5abe1ba2ad17586af8.png)

## **最终想法**

我太激动了！使用这个新功能确实在开发过程中帮助了我，让我的生活更加轻松。一个漫长而令人疲惫的(如果我可以完全诚实，和愚蠢的)过程刚刚得到解决，不到一分钟的解决方案。

还能做什么？
1。此时，我只能搜索一个静态字符串。例如，使用 api 调用
**来查找
**的端点信息 http://localhost:8080/service-A/post/{ name:[A-Z0–9 _]+}** http://localhost:8080/service-A/post/my-post** 我们只能搜索静态部分
**service-A/post**
如果能够搜索

2.如果出于任何原因，我们想将它部署到服务器上，我们肯定不想公开这个页面或功能。为此，我们需要从服务器端处理它，这样只有有权限的人才能访问我们刚刚创建的端点。

3.我相信可以做得更多，如果你想到其他可以添加的东西，让我知道！我很想听听！

> 完整的代码你可以在我的 github 上看到