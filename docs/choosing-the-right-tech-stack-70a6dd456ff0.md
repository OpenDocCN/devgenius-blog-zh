# 选择正确的技术组合

> 原文：<https://blog.devgenius.io/choosing-the-right-tech-stack-70a6dd456ff0?source=collection_archive---------1----------------------->

## 软件工程师的困境

![](img/2dd45a9c8c705f0511d2289ec381c747.png)

图片来源:谷歌图片(https://images.app.goo.gl/vkRBis5voicUcbyq8)

> 我是应该用 [Java](https://www.java.com/en/) 做我的应用的后端，还是应该用 [Go](https://golang.org/) ？
> 
> 我应该在 [Vue.js](https://vuejs.org/) 还是 [React](https://reactjs.org/) 中编写我的前端？
> 
> 我应该将我的数据存储在 [MongoDB](https://www.mongodb.com/) 还是 [MySQL](https://www.mysql.com/) 中？
> 
> 我的后端和前端应该使用 [REST](https://www.codecademy.com/articles/what-is-rest) 还是 [GraphQL](https://graphql.org/) 进行通信？
> 
> 我应该把我的应用程序放在 [AWS](https://aws.amazon.com/) 还是 [GCP](https://cloud.google.com/) 上？
> 
> *决断，决断！*

对于软件工程师来说，选择一种编程语言、一个应用程序框架、一个持久性存储、一个通信范例和一个托管服务是一项艰巨的任务，因为有许多选择可供选择。

见鬼。甚至在 [JavaScript](https://www.w3schools.com/js/DEFAULT.asp) 中寻找一个简单的 [AJAX](https://www.w3schools.com/xml/ajax_intro.asp) 库也能轻易让你偏头痛！

我没有夸张！

只需进入[npmjs.com](https://www.npmjs.com)，一个面向 JavaScript 平台的开源库公共集合( [Node.js](https://nodejs.org/en/) & [Web 浏览器](https://www.mozilla.org/en-US/firefox/browsers/what-is-a-browser/)，在搜索框中键入关键词[**【Ajax 请求】**](https://www.npmjs.com/search?q=ajax%20request) ，点击**回车/回车键**。

您将得到一个列表，每页大约有 [**640**](https://www.npmjs.com/search?q=ajax%20request) 个库， **20** 个库，总共有 **32** 个页，按它们的搜索分数(与搜索关键字匹配的百分比)排序，搜索分数最高的库显示在列表的最前面。

您可能不需要浏览所有的 **640** 结果来找到适合您使用的 ajax 库，但是仍然有很多可供选择！

当有这么多选择时，人们可能会想如何做出正确的选择？

理想情况下，您的选择应该基于以下问题的答案，这些问题是您在查看了该库(或者任何技术)的文档后得出的:

1.  有据可查吗？
2.  它是否涵盖了所有必需的用例？
3.  保养的好吗？
4.  将它集成到现有的代码库容易吗？
5.  它是否易于使用，也就是说，对于团队中的其他开发人员来说，学习曲线会不会太陡？
6.  它是开源的，还是需要付费许可才能用于商业用途？

一个缺乏文档记录的库(read technology)总是一个严格的禁忌，因为学习如何使用它的唯一方法是查看它的源代码，这非常耗时，因此不是大多数开发人员的首选。

根据库/技术的好坏，您可能会在上述几点上做出妥协。

例如，如果库/技术满足所有其他标准，但需要付费许可用于商业用途，您可能会同意，只要您的公司愿意买单！

我举一个具体的例子:

在我现在的公司，我正在做的项目要求我们使用 [iText](https://itextpdf.com/en) 库(需要付费许可用于商业用途)来生成 pdf。

除此之外，我们还使用免费的付费版 Adobe Acrobat Reader。它提供了许多附加功能，允许我们修改我们的应用程序要使用的 pdf，并使用 [iText](https://itextpdf.com/en) 进一步处理它们。

尽管我们为上述软件套件支付了一大笔钱，但投资回报(ROI)(我们能够有效地满足客户的需求)足以证明它是值得的！

您是否想知道所有这些与我们选择的技术堆栈有什么关系？

我想是的。所以现在开始:

你看， [iText](https://itextpdf.com/en) 库需要使用 [Java](https://www.java.com/en/) 或者 [C#](https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/) ，但是我们的核心后端是使用 [Go](https://golang.org/) 语言开发的。因此，要使用 iText，我们必须创建一个单独的基于 Java 的实用程序，它的唯一任务是生成 pdf。

基于 Java 的实用程序作为一个单独的应用程序在 [Apache Tomcat](http://tomcat.apache.org/) web 服务器上运行，这是一个针对基于 Java 的 web 应用程序的 [Java 企业版(Java EE)](https://www.oracle.com/in/java/technologies/java-ee-glance.html) 规范的开源实现。

我们的应用程序还需要使用[地理空间数据](https://www.sciencedirect.com/topics/computer-science/geospatial-data)，并在基于 Java 的实用程序生成的 PDF 上以图像的形式呈现它。为此，我们联系了我们的一位 GIS 专家，他编写了一个小型的 [Python](https://www.python.org/) 实用程序来完成这项工作。

想知道以上所有用不同编程语言编写的实用程序是如何一起工作的吗？

嗯，我们已经使用 [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) 协议将它们链接在一起。它是这样工作的:

1.  当 Go 服务器需要生成 PDF 时，它向基于 Java 的实用程序发送一个 [HTTP POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) 请求，该实用程序 pings 我们的核心 MongoDB 数据库以获得生成 PDF 所需的所有数据。
2.  如果基于 Java 的实用程序需要使用地理空间数据来生成请求的 PDF，它会向基于 Python 的实用程序发送一个 [HTTP GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) 请求，后者从我们的 MySQL 数据库中检索数据，并返回一个 [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics) 图像作为响应。
3.  然后，基于 Java 的实用程序将图像嵌入到 PDF 中，将 PDF 写入其本地磁盘，并返回一个 URL，最终用户可以使用该 URL 下载 PDF 文件。

从上面的描述来看，似乎所有的东西都各就各位，配合得天衣无缝！

但情况并非总是如此。

最初，当我们测试基于 Python 的实用程序时，我们发现它只做了最少的工作，也就是说，它只消耗地理空间数据并将生成的 PNG 图像写入磁盘。它无法使用 HTTP 协议与基于 Java 的实用程序通信！

为了让它能够做到这一点，我不得不使用 [Flask](https://flask.palletsprojects.com/en/1.1.x/) 将它包装在一个 web 应用程序中，这是一个用 Python 开发 web 应用程序的微型 web 应用程序框架。

基于 Flask 的应用程序本身需要使用 [Gunicorn](https://gunicorn.org/) ，一个用于 Unix 的 [WSGI](https://www.fullstackpython.com/wsgi-servers.html) HTTP 服务器！

因此，目前，我们的技术堆栈看起来像这样:

1.  应用前端的 [Vue.js](https://vuejs.org/) [JavaScript](https://www.w3schools.com/js/DEFAULT.asp) 框架
2.  用于应用程序核心后端的 [Go](https://golang.org/) 编程语言
3.  用于 PDF 生成工具的 [Java](https://www.java.com/en/) 编程语言
4.  用于运行 PDF 生成实用程序的 [Apache Tomcat](http://tomcat.apache.org/) web 服务器
5.  使用地理空间数据并以图像的形式呈现出来的编程语言
6.  用于将基于 Python 的实用程序包装到 web 应用程序中的 web 应用程序框架
7.  用于运行基于 Flask 的 web 应用程序的 guni corn(T21)web 服务器
8.  [MongoDB](https://www.mongodb.com/) 作为存储所有数据(地理空间数据除外)的核心持久性存储
9.  用于存储地理空间数据的 MySQL
10.  最后是 [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) 协议，用于在上述所有模块之间建立通信！

正如你所看到的，我们的技术堆栈非常多样化，它已经进化(最初，它只是 Vue.js & Go)以适应新的需求。随着我们项目规模的增长和我们向它添加新的特性，它将不断发展！

就选择这个技术堆栈的各种组件的标准而言，有些选择是为我们做出的(至少在最初)。相比之下，在过去的一年半时间里，我们自己选择了其他组件来满足新的需求。

关键是:你的技术永远不会是静态的。它随着时间不断发展。你无法预先预测它将如何发展，也无法当场做出所有相关的选择！

希望你对这篇文章感兴趣！

请在下面的评论区分享你的观点👇。

*最初发表于*[*【https://www.linkedin.com】*](https://www.linkedin.com/pulse/choosing-right-tech-stack-software-engineers-dilemma-pratik-chaudhari/)*。*