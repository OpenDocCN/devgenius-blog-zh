# NodeJS:什么和为什么？

> 原文：<https://blog.devgenius.io/nodejs-what-and-why-7068d834f616?source=collection_archive---------9----------------------->

NodeJS 是一个 javascript 运行时环境

![](img/2ab9f19aece74bf6c7d0ac0bbeb8a33e.png)

克莱门特·赫拉多特在 [Unsplash](https://unsplash.com/) 上拍摄的照片

根据官方 [NodeJs 文档](https://nodejs.dev/learn)，NodeJs 可以定义为一个开源的、跨平台的、后端的 Javascript 运行时环境，可以用来创建任何类型的项目。
由于定义本身很难理解，所以现在让我们将定义分解成单词并理解它的确切含义，因此 NodeJs 是开源的，跨平台意味着 nodejs 可以免费使用、共享，甚至可以由任何人修改，跨平台意味着我们可以编写能够在任何操作系统(如 Windows、Linux 或 Mac)中执行的 NodeJS 代码。

那么现在 **javascript 运行时**是什么意思呢？
Javascript runtime 简单来说就是我们运行代码时执行 Javascript 代码的地方。也就是说，如今 javascript 可以在任何装有“Javascript 引擎”程序的设备上运行。这个 javascript 引擎根据不同的平台有不同的名字，比如在谷歌 chrome 浏览器上叫 V8 引擎，在 Firefox 上叫 SpiderMonkey，在 Safari 上叫 JavascriptCore。

那么现在什么是 **javascript 引擎**？
Javascript engine 只是一个帮助将 Javascript 代码转换成机器可理解代码的程序。

总而言之，nodejs 是一个 javascript 运行时环境，用于创建服务器，并在 javascript 中执行后端操作，以开发实时可伸缩的应用程序。

**为什么要用 NodeJS？使用 nodejs 编程的优势在于使用一种编程语言(即 javascript)来开发前端和后端，因此 javascript 程序员在掌握一种编程语言方面具有优势，而不是为前端和后端学习不同的语言。
除此之外，Nodejs 运行在 chromes V8 引擎上，该引擎提高了 Nodejs 的性能，并允许进行异步操作。
异步意味着在同一时间同时执行操作，因此使用 nodejs 可以在不增加服务器负载的情况下执行异步操作。**

![](img/fc6e709ed9e01c4a428d78bed659b1c0.png)

NodeJs

Nodejs 附带了 **npm(节点包管理器)**，这是节点的默认包管理器，因此通过 npm，我们可以重用已经存在的代码，也可以开发自己的包供他人使用，从而节省开发时间。

**nodejs 与 javascript 有何不同？** 由于 nodejs 使用 javascript，所以很多新开发人员对这两者都感到困惑，这里有几个主要的区别，

*   Javascript 是一种编程语言，而 nodejs 是 javascript 运行时环境
*   Javascript 在浏览器上运行，但是在 nodejs 的帮助下，我们也可以在浏览器之外运行 javascript 代码。
*   Javascript 是一种客户端脚本语言，因此用于创建前端，而 nodejs 用作服务器端脚本语言，用于创建应用程序的后端。
*   使用 nodejs，我们可以执行文件和操作系统操作，而使用 javascript，我们可以执行特定于浏览器的操作和很少的操作系统操作，如上传文件等，以及某些权限。

**nodejs 的缺点** 到目前为止，我们已经看到了什么是 nodejs，为什么我们现在要使用它让我们看看什么时候不应该使用 nodejs。

*   由于 nodejs 使用异步编程模型，随着应用程序规模的增加，维护代码变得很困难。
*   在服务器上执行繁重的计算任务时，不应使用它。
*   由于有各种 npm 模块可供使用，所以应该明智地选择它们，因为其中一些可能包含质量较差的代码和文档。

**现实世界的应用** 现在我们已经看到了 nodejs 的所有优点和缺点，以及为什么我们应该使用它，所以现在让我们来看看世界各地的人们在日常生活中使用的一些最流行的现实世界的应用。

> LinkedIn——专业人士的网络平台
> 
> Paypal —支付应用程序
> 
> Trello —项目管理应用程序
> 
> 优步—随时随地的汽车服务

因此，nodejs 被大多数科技公司广泛使用，所以如果你想在下一个项目的后端使用它，学习 nodejs 绝对是最好的选择。

**总结:**

*   NodeJS 是一个运行在 chromes V8 引擎上的 javascript 运行时环境。
*   用于开发应用程序的后端。
*   允许执行异步操作。
*   可用于开发实时可伸缩的应用程序。

**有用资源:**

[NodeJs 简介](https://nodejs.dev/learn)

[节点历史](https://nodejs.dev/learn/a-brief-history-of-nodejs)

[节点的优点](https://relevant.software/blog/7-benefits-of-node-js-for-startups/)

[NodeJS:现实世界的应用](https://clockwise.software/blog/node-js-app-examples/)