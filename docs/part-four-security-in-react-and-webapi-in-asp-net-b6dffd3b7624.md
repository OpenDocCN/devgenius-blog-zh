# 第四部分:React 和 WebApi 在 ASP.NET Core C #中的安全性

> 原文：<https://blog.devgenius.io/part-four-security-in-react-and-webapi-in-asp-net-b6dffd3b7624?source=collection_archive---------0----------------------->

第四部分:使用访问 JWT 令牌持有者授权从 React SPA 调用受保护的 Web API

版本 1.0
日期 2022/06/15
作者 Nicolas Barlatier

![](img/a107c0d9b790382376d26d099081d55f.png)

如果你错过了第一部分:
[第一部分:用 Docker 和 Administration 安装 key cloak](https://medium.com/p/1d076777a979)

如果您错过了第二部分:
[第二部分:保护前端 React 应用程序](/security-in-react-and-webapi-in-asp-net-core-c-with-authentification-and-authorization-by-keycloak-89ba14be7e5a)

如果您错过了第三部分:
[第三部分:保护 ASP.NET 核心 C# REST Web API](/security-in-react-and-webapi-in-asp-net-core-c-with-authentification-and-authorization-by-keycloak-f890d340d093)

要在 Docker 中使用**前端 React 应用程序，您可以阅读:**

[第一部分将 TypeScript React App 与 NodeJS 和 NGINX 与 Windows 10 上的 WSL2 Alpine Linux 进行对比](https://medium.com/@barlatiernicolas/dockerizing-the-typescript-react-app-with-nodejs-vs-nginx-with-wsl2-alpine-linux-on-windows-10-8dddd447f43a)

包含 React 和 Web API 项目的 GitHub 存储库

*   **使用类型脚本对 18.1.0 版做出反应**
*   **采用 ASP.NET 核心 5.0 的 Web API**

[](https://github.com/nicoclau/reactwebapiaspnetcorekeycloak) [## GitHub-nicoclau/reactwebapiaspnetcorekeycloak:React 和 REST 受 keycloak 保护的 Web API 与…

### React 和 REST Web API 受 Keycloak 保护，带授权码流和 JWT 令牌- GitHub …

github.com](https://github.com/nicoclau/reactwebapiaspnetcorekeycloak) 

您有两个提交:

![](img/38a6e19aea67aef3f2d66e21b54cde1b.png)

第一次提交包含:

*   由 keycloak 服务器保护的 React SPA
*   Web API 由访问令牌保护，使用来自 keycloak 服务器的公钥进行验证

这两个应用程序还没有通信，它们只是受到保护。

第二个**提交**包含:

*   React SPA 使用 JWT 令牌与 Web API 通信，并处理 CORS 策略

# 介绍

在第四部分也是最后一部分，我们将在 **React** 中看到如何使用**访问 JWT 令牌**来调用我们的受保护资源:REST Web API 。

代码库将非常简单，不会使用中间件，redux 通常用于更多面向生产的应用程序。

在这里，我们只希望看到我们需要的东西，而不是通过我们的 JWT 访问令牌来获取我们的服务。

本博客将分为三个部分:

*   创建 HTTP 服务，这将有助于通过 HTTP 发送带有 JWT 令牌的请求
*   使用服务
*   更新 REST Web API 服务以处理 CORS 策略

**1-创建 HTTP 服务**

让我们用可视化代码 IDE 打开 React 解决方案。

请记住，我们有以下解决方案结构:

![](img/b6828f758dbb7cb5554be03692358fd0.png)

我们用 Keycloak 服务添加了一个名为 security 的目录。
现在我们将创建一个名为 services 的新目录。

![](img/77dd76a1d98f952ff378c7b50513355a.png)

我们得到了

![](img/9fea983e4871e7232742fab64cf4eb86.png)

现在让我们添加文件 **HttpService.ts** ，它将负责发送和接收我们对受保护的 Web Rest API 的调用。

你注意到我使用了扩展名**。ts 和 not。因为我们使用“纯”类型脚本，而不是像在 React 组件中那样混合使用类型脚本和 JSX。**

现在我们有了选择，事实上，反应过来并不是很固执己见。
在使用 HTTP 时，我们可以选择库/框架:

我们有 2022: 10 个不同的 HTTP 库，可以在 React 中轻松使用:

![](img/8995a02df67fa44a24649e9d9ca7d232.png)[](https://openbase.com/categories/js/best-react-http-request-libraries) [## 2022 年 10 大最佳 HTTP 请求库| Openbase

### 2022 年 10 个最好的 React HTTP 请求库对比:beccaccino，hermes-js，react-http-request…

openbase.com](https://openbase.com/categories/js/best-react-http-request-libraries) 

我们还有 **Fetch API** 是一个工具，内置于大多数现代浏览器的 window object ( `window.fetch`)上，使我们能够使用 JavaScript promises 非常容易地发出 HTTP 请求。

在库中，最常用的是 axios。此外，它还内置了**类型脚本定义**

[](https://openbase.com/js/axios) [## axios:文档、教程、评论| Openbase

### 基于 Promise 的浏览器和 node . js HTTP 客户端新的 axios docs 网站:单击此处从…

openbase.com](https://openbase.com/js/axios) 

所以我们可以在 axios 和 Fetch API 之间犹豫不决

我们可以在好文章中找到

[](https://www.smashingmagazine.com/2020/06/rest-api-react-fetch-axios/) [## 在 React With Fetch 和 Axios - Smashing 杂志中使用 REST APIs

### 如果你是一名 React 开发人员，想学习如何在 React 应用程序中使用 API，那么这个…

www.smashingmagazine.com](https://www.smashingmagazine.com/2020/06/rest-api-react-fetch-axios/) 

向我们展示了如何使用:

*   Axios
*   获取 API

要知道什么是最佳解决方案，另一篇文章可以提供帮助:

[](https://blog.logrocket.com/axios-vs-fetch-best-http-requests/#:~:text=To%20send%20data%2C%20fetch%28%29,stringify%20method) [## Axios 与 fetch():哪个最适合做 HTTP 请求？- LogRocket 博客

### Axios 并不总是理想的解决方案；根据您的需要，有时会有更好的选择来制作 HTTP…

blog.logrocket.com](https://blog.logrocket.com/axios-vs-fetch-best-http-requests/#:~:text=To%20send%20data%2C%20fetch%28%29,stringify%20method) 

*   Axios 比 Fetch API 更兼容很多浏览器。Axios 在幕后使用 **XMLHttpRequest** 。它包装它来帮助我们。
*   我们将使用的基本语法中的 Axios 更适合发布数据和接收已经在 Javascript 对象中解析的响应。使用 Fetch API，我们需要调用 Response.json()来解析响应并返回 JavaScript 对象。
*   使用 Fetch API，我们可以为相同的任务设置更多的东西。
*   Axios 更容易处理超时问题。
*   Axios 提供了拦截 HTTP 请求来添加我们的 JWT 令牌的可能性
*   Axios 可以提供下载进度
*   Axios 可以在//中提供几个请求

我们需要轻松地添加我们的 JWT 令牌，所以 axios 更好。

**所以让我们用我们常用的 npm 安装 axios 吧！**

在可视代码终端中

![](img/96cbe42c78cd904f3f454708efe8edf4.png)

使用命令:

```
npm i axios
```

![](img/be56abf5d450a7f01ea535741c9e8d3c.png)![](img/1014b91b0a40d7564b799d7dce878fb4.png)

现在我们有了 axios:

![](img/b18be4ba35b380a2a2f927666d4bc210.png)

但是在我们需要通过添加以下内容来更新我们的 KeyCloakService 之前:

*   IsLoggedIn
*   格托肯
*   更新令牌

GetToken 很明显，我们需要获得访问 JWT 令牌。然后，我们可以使用它将它添加到 HTTP 请求的报头:Bearer 中，这样我们就可以使用承载授权。

对于其他方法，我们将在 HttpService 的源代码中了解它们的用途。

HttpService 将配置 axios 在每个 HTTP 请求中添加访问 JWT 令牌

我们在 HttpService 中有三个主要的方法:

*   配置:当我们的 React 应用程序加载时，我们将使用它来设置我们的 axios 实例，以添加由另一个服务 KeyCloakService 返回的令牌。我们在**请求**级别使用了 Axios 的**拦截器的概念，它将为任何请求自动添加我们的 JWT 令牌，在我们的代码库中我们调用这个 axio 实例的任何地方**
*   getAxiosClient:返回代码库中任意位置的 axios 实例
*   HttpMethods:只是一个帮助器方法来发送 HTTP 方法

你可以看到更多关于 axio 拦截器的信息:

 [## 截击机

### 您可以在请求或响应被 then 或 catch 处理之前拦截它们。如果您需要移除拦截器…

axios-http.com](https://axios-http.com/docs/interceptors) 

现在我们来详细解释一下**配置**的方法

它分为两个主要部分:

*   我们创建了一个名为 cb (configure bearer)的函数，它检查用户是否通过了 keycloak 认证:如果是，我们设置 axios 发送报头“bearer”。
    ***这个函数还没调用呢！***
*   只有当我们从 **UpdateToken 调用**中成功返回时，才会调用它。在 **UpdateToken** 中，只有在令牌即将在 5 秒内到期时才更新令牌:参见另一个服务 KeyCloakService 的 UpdateToken 的代码，否则不更新令牌，返回成功。
    所以当令牌未更新时== >成功，当令牌过期且更新成功时== >成功，否则== > ko

**2-使用 HTTP 服务**

让我们在 Index.tsx 中使用我们的 HttpService

![](img/79851d2c447b2e22d872931314605a9d.png)

我们得到以下代码:

我们在 Keycloak 服务之后调用 HttpService，因为 HttpService 依赖于 KeyCloak 来检查用户是否经过身份验证，并获取访问 JWT 令牌。

让我们运行并检查我们的 SPA react 是否运行顺畅。

![](img/287a936f3c553f3ecb187eec438f62d4.png)![](img/f90e72c57d927dc66babfc536531b13b.png)

我们现在准备在 App.tsx 文件的应用程序组件中添加新功能“WeatherCast ”:

*   调用我们受保护的 Rest Web Api，通过发送我们的 keycloak 服务返回的令牌来获取天气信息。

让我们添加一个名为“WeatherCast”的简单按钮，它将调用我们的 Web Api。

我们只是包含了我们的 HttpService，然后调用方法 getAxiosClient()来获取带有请求拦截器的 axios 实例，当然，如果我们的用户被我们的 Keycloak 服务器正确地认证了，则添加 JWT 令牌。

我们将确保使用我们机器的 IP。

*   因此，我们将有我们的 r **eact 应用程序**，它运行在 **webpack 开发服务器**上，带有: [http://yourip:yourport](http://yourip:yourport) 这里我们有 [http://10.7.7.11:3000](http://10.7.7.11:5001) (对你来说，ip 可能不同，端口也可能不同)
*   我们的 **web api 运行在我们的 kestrel dev** 服务器上，带有[https://your IP:your port](https://yourip:yourport)这里我们有 [https://10.7.7.11:5001](http://10.7.7.11:5001)

因此，我们有两个应用程序运行在同一台机器上，就像我们在 dev 上一样，但是我们有不同的协议(http vs https)和不同的端口。

这样，我们就接近了在不同机器上提供服务的真实情况。我们会看到这足以得到 CORS 的保护，从而阻止我们！:-)

我们将看看如何解决这个问题。
顺便说一下，在 Keycloak 服务器上，我们已经通过设置 CORS 解决了这个问题

![](img/ad47781944fd4a328b08c3ca0344a8b9.png)![](img/3820d549b88ccfebedc4548698d3b75a.png)

在这里我们告诉我们的 Keycloak 服务器，我们的客户端 react 可以通过 XmlHttpRequests 到达它。

记住 CORS 检查的三个部分:

*   计划
*   域/子域
*   港口

请看看这篇文章，以便更好地了解这种保护。

[](https://medium.com/@baphemot/understanding-cors-18ad6b478e2b) [## 了解 CORS

### 如果您曾经使用过 AJAX 调用，您可能对浏览器控制台中显示的以下错误很熟悉:

medium.com](https://medium.com/@baphemot/understanding-cors-18ad6b478e2b) 

开发者 Mozilla 提供了一份非常详细的文档，非常精彩！

[](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#preflighted_requests) [## 跨来源资源共享(CORS) - HTTP | MDN

### 跨源资源共享(CORS)是一种基于 HTTP 头的机制，允许服务器指示任何源…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#preflighted_requests) 

顺便说一句，我用我的链接帮助媒体上的其他作者！:)

当我们试图点击按钮时，我们得到错误消息:

![](img/f1957f9bfb0dd0817c5006c4613bd5e9.png)

不酷，发生了什么事？

当我们检查 chrome 开发工具的控制台时，我们看到:

![](img/191a998d1db26812a15ed2206c3213c8.png)

> 对位于'[https://10 . 7 . 7 . 11:5001/weather forecast '](https://10.7.7.11:5001/WeatherForecast')from Origin '[http://10 . 7 . 7 . 11:3000 '](http://10.7.7.11:3000')的 XMLHttpRequest 的访问已被 CORS 策略阻止:对预检请求的响应未通过访问控制检查:请求的资源上不存在“Access-Control-Allow-Origin”标头。

基本上，react 应用程序对 webapi 的请求被 chrome 浏览器的 CORS 政策阻止了。

为什么这样因为 web api 服务器没有在响应中发回预期的标头“Allow-Control-Allow-Origin ”,其值为客户端的 URI:我们的 react SPA 应用程序。

好了，在我们解决 CORS 问题之前，让我们来看看 chrome、keycloak 服务器和 web api 应用程序之间的网络交换:

![](img/37aef61307a83b06ce8275ec8be06f31.png)

在承载授权中访问 JWT 令牌

我们可以通过发送用于[载体认证](https://swagger.io/docs/specification/authentication/bearer-authentication/#:~:text=Bearer%20authentication%20(also%20called%20token,the%20bearer%20of%20this%20token.%E2%80%9D)的访问 JWT 令牌来检查 react 中的服务 HttpService 是否正确地完成了工作😃👍

所以对于 CORS，我们又有了一个请求/回应之舞。让我们和 Fiddler 确认一下

*   首先，chrome 检测到我们需要将 ajax 请求发送到另一个 scheme 协议(https)和不同的端口，相同的 IP。因此，它向服务器发送 OPTIONS 方法 HTTP 请求，询问是否允许 SPA react 应用程序执行这个 ajax 请求。
    对于任何“不简单”的请求，这是**飞行前请求**(更多信息请见下面关于 CORS 的链接)

[](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) [## 预检请求- MDN Web 文档词汇表:Web 相关术语的定义| MDN

### CORS 预检请求是一种 CORS 请求，用于检查是否理解 CORS 协议以及服务器是否知道…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) ![](img/da45153f464a338d3673b47d11bd65ef.png)

```
OPTIONS https://10.7.7.11:5001/WeatherForecast HTTP/1.1
Host: 10.7.7.11:5001
Connection: keep-alive
Accept: */*
Access-Control-Request-Method: GET
Access-Control-Request-Headers: authorization
Origin: [http://10.7.7.11:3000](http://10.7.7.11:3000)
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
Sec-Fetch-Dest: empty
Referer: [http://10.7.7.11:3000/](http://10.7.7.11:3000/)
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,es;q=0.8,fr;q=0.7,zh-CN;q=0.6,zh;q=0.5,nl;q=0.4
```

基本上，chrome 会询问服务器是否允许应用程序使用请求方法:获取 URI。Chrome 通过使用请求方法:OPTIONS 来询问这个问题。

服务器是我们的 Web Api，现在它还不知道 CORS 的事情。所以它会回答:

```
HTTP/1.1 405 Method Not Allowed
Date: Thu, 16 Jun 2022 13:28:41 GMT
Server: Kestrel
Content-Length: 0
Allow: GET
```

它回答:“电脑说没有！”

![](img/317af945179d6d26f6698498122d6226.png)

非常著名的电视节目“小英国”😆

为什么不呢？

基本上，我们必须告诉我们的服务器 Web Api 应用程序，允许我们的 React SPA 使用 GET 方法或任何其他方法(如 POST)请求我们的资源。

所以我们必须回到我们的 Web Api 项目。

**3-更新 REST Web API 服务以处理 CORS 策略**

启用 CORS 中间件，这样我们就可以处理 HTTP 请求选项，这非常简单。

我们只需配置 CORS 服务，并告诉它我们允许 React SPA 应用程序(由它的 URI 标识)使用任何方法和头来到达 Web API 中的端点。

一旦建立了 CORS 服务，我们就可以在管道中启用 CORS 中间件。我们告诉中间件通过引用它们的名称来使用设置，这里是“MyPolicy”

我们可以在微软文档中找到 ASP.NET 核心 5.0 的所有细节:

[](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-5.0) [## 在 ASP.NET 核心中启用跨来源请求(CORS)

### 了解如何在 ASP.NET 核心应用程序中将 CORS 作为允许或拒绝跨来源请求的标准。

docs.microsoft.com](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-5.0) 

一旦我们这样做了，首先要确保 chrome 浏览器通过接受 HTTPS 自签名证书来允许访问我们的 Web Api。否则，如果 Chrome 浏览器不信任该证书，我们的 React 应用程序将会出错。

如果您看到:

![](img/7137de06bd632641299a364e6d757378.png)

点击“高级”,然后点击“继续……”

![](img/5ea8e380759618fa2e0f3fad87678298.png)

然后你会看到

![](img/75b0c16a2ae2e5dd3250522d3a26a729.png)

通过这种方式，你可以确定我们的 React 应用程序将能够通过 Ajax 使用我们的 Chrome 浏览器访问 Web Api。

![](img/2784b1bf2111ce88ce5700609b28284c.png)

我们可以看到我们关闭了安全警告，如果需要，我们可以随时回滚。

让我们再次尝试 React 应用程序:

![](img/4ff103630cf9cf750eece233c62c07fa.png)

我们成功了！

让我们来看看小提琴手:

同样，您需要使用 Chrome 访问 WebApi，并告诉它忽略警告，因为 Fiddler 使用了另一个证书。

让我们进入 React 应用程序，点击按钮“天气预报”

![](img/f61c184d3e24f0ef2a6482a142bd13a5.png)

我们接到两个电话:

第一次请求/响应

```
OPTIONS https://10.7.7.11:5001/WeatherForecast HTTP/1.1
Host: 10.7.7.11:5001
Connection: keep-alive
Accept: */*
Access-Control-Request-Method: GET
Access-Control-Request-Headers: authorization
Origin: [http://10.7.7.11:3000](http://10.7.7.11:3000)
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
Sec-Fetch-Dest: empty
Referer: [http://10.7.7.11:3000/](http://10.7.7.11:3000/)
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,es;q=0.8,fr;q=0.7,zh-CN;q=0.6,zh;q=0.5,nl;q=0.4HTTP/1.1 204 No Content
Date: Thu, 16 Jun 2022 13:53:23 GMT
Server: Kestrel
Access-Control-Allow-Headers: authorization
Access-Control-Allow-Methods: GET
Access-Control-Allow-Origin: http://10.7.7.11:3000
```

这一次，我们在 Kestrel 服务器上的 Web Api 应用程序返回了 expecter CORS 头，说:是的，允许使用 GET。

在第二次调用中，我们得到以下请求/响应

```
GET https://10.7.7.11:5001/WeatherForecast HTTP/1.1
Host: 10.7.7.11:5001
Connection: keep-alive
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="102", "Google Chrome";v="102"
Accept: application/json, text/plain, */*
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ1RXJacF9SUjJsRVRiN205a2tmMTJOel8wWjJiaVAyVVZBSDNiRTRfNG5VIn0.eyJleHAiOjE2NTUzODc4NDYsImlhdCI6MTY1NTM4NzU0NiwiYXV0aF90aW1lIjoxNjU1MzgzOTU4LCJqdGkiOiI2YmJhYjVjYy0wMzI5LTRmMWMtYTMxZS05Y2NmMjdkNjg3MDYiLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvcmVhbG1zL015UmVhbG0iLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMWVmM2UwMmItNTUwMy00YmIwLWI3NDgtNzM4NGZkM2ExOTc1IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiTXlBcHAiLCJub25jZSI6IjYwZDRhOWJmLWVjNTAtNDNkOS05MTI5LTk3YWRlZDFlMmQ5ZiIsInNlc3Npb25fc3RhdGUiOiI1YTEyMjRiYy0yNzk4LTRhMjEtOTAwNC1lNDFlODNiZGYyMjIiLCJhY3IiOiIwIiwiYWxsb3dlZC1vcmlnaW5zIjpbImh0dHA6Ly8xMC43LjcuMTE6MzAwMCJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1teXJlYWxtIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7Ik15QXBwIjp7InJvbGVzIjpbIkFkbWluIl19LCJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBlbWFpbCBwcm9maWxlIiwic2lkIjoiNWExMjI0YmMtMjc5OC00YTIxLTkwMDQtZTQxZTgzYmRmMjIyIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJuaWNvbGFzIn0.e_QiJXIdzQicLpn_wCG0c3idaXokfAu75vpcUd91dRep7YZyPX1el0W4BqC7pm_fKHdwtvYT71RkVs9WmQIoT-JrwmhLzkM_fbjZt0ISK5jan7tS5qJ6gBeKj7wscV1qtXT1BaZ0RWppzm1e-Pfd0QA3NYivdTnqEeSdAUM-hE1Z1_w-COtipVhPeH_K27TEOT-opsgbn9kjOpTeauJ1sX7egAXZzRQrposRO3YGF9cE_92cwuFMzY1qlkdaB8HIYqqXxuL3LhExnwXPyrEAMVBz2k9NUzniboefFnsL36k6uqtmWC_x6ZrcLCi6zz0no9oPqeaXJXA1cwaS72QIRg
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36
sec-ch-ua-platform: "Windows"
Origin: [http://10.7.7.11:3000](http://10.7.7.11:3000)
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: [http://10.7.7.11:3000/](http://10.7.7.11:3000/)
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,es;q=0.8,fr;q=0.7,zh-CN;q=0.6,zh;q=0.5,nl;q=0.4HTTP/1.1 200 OK
Date: Thu, 16 Jun 2022 13:53:23 GMT
Content-Type: application/json; charset=utf-8
Server: Kestrel
Transfer-Encoding: chunked
Access-Control-Allow-Origin: [http://10.7.7.11:3000](http://10.7.7.11:3000)1f4
[{"date":"2022-06-17T15:53:24.5701743+02:00","temperatureC":-8,"temperatureF":18,"summary":"Chilly"},{"date":"2022-06-18T15:53:24.5701806+02:00","temperatureC":49,"temperatureF":120,"summary":"Warm"},{"date":"2022-06-19T15:53:24.5701811+02:00","temperatureC":19,"temperatureF":66,"summary":"Scorching"},{"date":"2022-06-20T15:53:24.5701814+02:00","temperatureC":-1,"temperatureF":31,"summary":"Balmy"},{"date":"2022-06-21T15:53:24.5701818+02:00","temperatureC":17,"temperatureF":62,"summary":"Warm"}]
0
```

我们用 GET Http 方法，即 JWT 令牌发送请求。我们的 Web Api 验证了我们的 JWT 令牌，并返回了响应，因为 CORS 也被验证了！

**结论**

唷！最后，我们用 Keycloak、代码授权流程、React 应用程序和 ASP.NET 5.0 REST Web Api 应用程序实现了认证和授权。

我希望这 4 个部分能帮助你在你自己的生产项目中走得更快，更容易地保护你的服务。

谢谢你一直阅读我的文章！我希望它们是清晰的，易于理解和代码复制。

如果文章的任何部分不正确或令人困惑，请不要犹豫，在评论部分问我！😃

**参考文献**

[](https://openbase.com/categories/js/best-react-http-request-libraries) [## 2022 年 10 大最佳 HTTP 请求库| Openbase

### 2022 年 10 个最好的 React HTTP 请求库对比:beccaccino，hermes-js，react-http-request…

openbase.com](https://openbase.com/categories/js/best-react-http-request-libraries) [](https://openbase.com/js/axios) [## axios:文档、教程、评论| Openbase

### 基于 Promise 的浏览器和 node . js HTTP 客户端新的 axios docs 网站:单击此处从…

openbase.com](https://openbase.com/js/axios) [](https://www.smashingmagazine.com/2020/06/rest-api-react-fetch-axios/) [## 在 React With Fetch 和 Axios - Smashing 杂志中使用 REST APIs

### 如果你是一名 React 开发人员，想学习如何在 React 应用程序中使用 API，那么这个…

www.smashingmagazine.com](https://www.smashingmagazine.com/2020/06/rest-api-react-fetch-axios/) [](https://medium.com/@baphemot/understanding-cors-18ad6b478e2b) [## 了解 CORS

### 如果您曾经使用过 AJAX 调用，您可能对浏览器控制台中显示的以下错误很熟悉:

medium.com](https://medium.com/@baphemot/understanding-cors-18ad6b478e2b) [](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) [## 预检请求- MDN Web 文档词汇表:Web 相关术语的定义| MDN

### CORS 预检请求是一种 CORS 请求，用于检查是否理解 CORS 协议以及服务器是否知道…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) [](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#preflighted_requests) [## 跨来源资源共享(CORS) - HTTP | MDN

### 跨源资源共享(CORS)是一种基于 HTTP 头的机制，允许服务器指示任何源…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#preflighted_requests)