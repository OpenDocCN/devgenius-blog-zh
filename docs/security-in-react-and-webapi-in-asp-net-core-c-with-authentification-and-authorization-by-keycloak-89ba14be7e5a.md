# 第二部分:React 中的安全性和 ASP.NET 核心 C#中的 WebApi，通过 KeyCloak 进行认证和授权

> 原文：<https://blog.devgenius.io/security-in-react-and-webapi-in-asp-net-core-c-with-authentification-and-authorization-by-keycloak-89ba14be7e5a?source=collection_archive---------0----------------------->

## **第二部分:保护前端反应**应用
**版本 1.0
日期 2022/05/29
作者 Nicolas Barlatier**

![](img/a3155cdd480ccdbb4046db674092c91a.png)

如果你错过了第一部分:
[第一部分:用 Docker 和 Administration 安装 key cloak](https://medium.com/p/1d076777a979)

下一部分可用:
[第三部分:保护 ASP.NET 核心 C# REST Web](https://medium.com/@barlatiernicolas/security-in-react-and-webapi-in-asp-net-core-c-with-authentification-and-authorization-by-keycloak-f890d340d093)

最后第四部分:
[第四部分:使用访问 JWT 令牌载体授权从 React SPA 调用受保护的 Web API](https://medium.com/@barlatiernicolas/part-four-security-in-react-and-webapi-in-asp-net-b6dffd3b7624)

要在 Docker 中使用**前端 React 应用程序，您可以阅读:**

[第一部分将 TypeScript React App 与 NodeJS 和 NGINX 与 Windows 10 上的 WSL2 Alpine Linux 进行对比](https://medium.com/@barlatiernicolas/dockerizing-the-typescript-react-app-with-nodejs-vs-nginx-with-wsl2-alpine-linux-on-windows-10-8dddd447f43a)

包含 React 和 Web API 项目的 GitHub 存储库

*   **React 18 . 1 . 0 版带打字稿**
*   **采用 ASP.NET 核心 5.0 的网络应用编程接口**

[](https://github.com/nicoclau/reactwebapiaspnetcorekeycloak) [## GitHub—nicoclau/reactwebapiaspnetcorekeycloak:React 和 REST 受 keycloak 保护的 Web API 与…

### React 和 REST Web API 由带有授权代码流和 JWT 令牌的 Keycloak 保护— GitHub …

github.com](https://github.com/nicoclau/reactwebapiaspnetcorekeycloak) 

您有两个提交:

![](img/38a6e19aea67aef3f2d66e21b54cde1b.png)

第一次提交包含:

*   由 keycloak 服务器保护的 React SPA
*   Web API 由访问令牌保护，使用来自 keycloak 服务器的公钥进行验证

这两个应用程序还没有通信，它们只是受到保护。

第二次提交包含:

*   React SPA 使用 JWT 令牌与 Web API 通信，并处理 CORS 策略

# 介绍

本文将向您展示如何通过认证和授权来保护一个前端 React 单页面应用程序。

它将被分成**三个主要部分:**

*   **了解 OAuth 2.0 和 OpenID 连接协议的流程**和**什么流程最适合我们的 SPA React 应用**
*   **通过**在 React 中添加 Keycloak Javascript 适配器**来保护我们的 SPA React 应用程序**
*   如何在带有自定义 KeyCloakService 的应用程序中正确使用 Keycloak Javascript 适配器

第一步是**在 keycloak** 中注册一个“客户端”,它代表我们要保护的应用程序(参见第一部分记住如何:[第一部分:用 Docker 和 Administration](https://medium.com/p/1d076777a979) 安装 keycloak)

我们可以在下面看到关于 keycloak 中“客户端”概念的详细部分:

[](https://www.keycloak.org/docs/latest/server_admin/index.html#assembly-managing-clients_server_administration_guide) [## 服务器管理指南

### 用户用户是能够登录到您的系统的实体。它们可以有与自身相关的属性…

www.keycloak.org](https://www.keycloak.org/docs/latest/server_admin/index.html#assembly-managing-clients_server_administration_guide) 

我们需要记住的是:

> **客户端是可以请求用户认证的实体。**

当我们创建我们的**客户端**时，我们必须意识到 **Keycloak** 自动将**客户端**与默认的**流**相关联，这将在稍后详述。

记得当我们创建客户端时，选择了协议 **OpendId Connect** 。

# OAuth 2.0 和 OpendId 连接流

因此，我们需要更好地理解不同的**流程**,包括:

*   **OAuth 2.0**
*   OpenId 连接。

我们需要了解他们的目的是什么，这样我们才能选择适当的流程来保护我们的前端 React 应用程序，以及为什么它是最佳选择！

# ***OAuth 流程***

OAuth 2.0 协议中的流实际上被称为“授权类型”

*   [授权码](https://oauth.net/2/grant-types/authorization-code/):授权码授予类型用于**机密客户端和公共客户端**将**授权码**兑换成**访问令牌**。在用户通过**重定向 URL** 返回客户端后，应用程序将从 URL 获取**授权码**，并使用**请求访问令牌**。
*   [PKCE](https://oauth.net/2/pkce/) :是[授权代码流](https://oauth.net/2/grant-types/authorization-code/)的扩展，防止 **CSRF** 和**授权代码注入攻击**。
*   [带表单 Post](https://auth0.com/docs/get-started/authentication-and-authorization-flow#implicit-flow-with-form-post) 的隐式流程:用于**公共客户端**，或**无法安全存储客户端机密的应用。**
*   [客户端凭证](https://oauth.net/2/grant-types/client-credentials/):被**客户端**用来获取用户上下文之外的**访问令牌**。这通常由客户端用来访问关于它们自己的资源，而不是访问用户的资源。== >我们稍后会更好地解释这是什么意思。
*   [设备代码](https://oauth.net/2/grant-types/device-code/):由[设备流程](https://oauth.net/2/device-flow/)中的无浏览器或输入受限设备使用，用于将之前获得的设备代码交换为访问令牌。
*   [刷新令牌](https://oauth.net/2/grant-types/refresh-token/):当访问令牌过期时，客户端使用刷新令牌交换访问令牌。这允许客户端继续拥有有效的访问令牌，而无需与用户进一步交互。

**如果我们没记错的话，我们实际上使用的是基于 OAuth 2.0 的 procotol OpenId Connect** 。

因此，我们需要查看可用的 OpenId 连接流。

# ***OpenId 连接流程***

***OpenId Connect***定义了可用于认证用户的四个主要流程:

*   **授权代码流**对于基于浏览器的应用，如 SPAs(单页应用)或服务器端应用== >我们将使用此选项
*   **隐式流**针对基于浏览器的应用，安全性不如上一个，OAuth 2.1 中不推荐和弃用；
*   对于像 web 服务这样的 REST 客户端，它涉及到存储一个秘密，所以客户端应该是可信的；
*   **资源所有者密码凭证授权**对于 REST 客户端，如与大型机的接口和其他不支持现代认证协议的遗留系统，它涉及到与另一个服务共享凭证，请注意。

还记得我们创建客户端时，我们

*   设置名称:MyApp
*   **客户端协议:openid-connect，因此我们必须根据协议流进行推理**
*   根目录 UR:http://locah host:3000

![](img/2b5a8cd9017e5af247018ae2347e7083.png)

创建新客户端

## 我们将看到下面的页面，**我们将突出显示需要了解的 6 个重要部分。**

# 客户端管理和流程

![](img/69250d5c08a167ccff057e98c6715a8b.png)

keycloak 自动设置的 6 个主要部分。我们必须掌握它们，这样我们才能知道幕后发生了什么。

默认情况下，这些零件会显示键盘锁:

*   第 1 部分) :将**客户端设置为 Enabled** :这样客户端就可以发起或不发起登录并取回**访问令牌**
*   第二部分):将**访问类型**设置为**“公共”**，用于**前端公共客户端无法安全存储秘密**发起登录。
    例如，我们的 SPA react 应用程序是一个公共应用程序，因为 javascript 代码直接交付给用户的浏览器。我们很难认为将秘密存储在 js 文件中是安全的！

> 我们在谈论什么秘密？该秘密用于授权代码流，其中客户端通过在请求中发送带有**秘密**的授权代码来用**授权代码**交换**访问令牌**。在我们的例子中，这个秘密不会被使用，但是我们将使用**valid Redirect URIs 来确保没有任何 SPA** 到达 Keycloak 服务器。

![](img/550f68def008a614215791c21b6d4054.png)

访问类型:公共

如果我们将访问类型更改为“**机密**”，我们将获得以下红色的新字段

![](img/591c3e7df81033fee2593e86236a7f70.png)

新字段:服务帐户，OIDC·CIBA 授权，授权已启用

只有**服务账户**是重要的:如果启用，我们实际上使用的是“**客户端凭证授权**”。
**服务账号对公共客户端不可用，因为没有可用的秘密。**

![](img/5e4939a07ad6f2660e3b2a7e9191c8c0.png)

它允许我们使用"**客户凭证 Gran** t "

为了理解“授权码授权类型流”和“客户端凭证流”之间的区别，请参见图表:

![](img/6949ad259978688df65cb11e08180bea.png)

授权码授予

资源所有者访问应用程序，我们有 2 个步骤:

*   步骤 1:身份验证和授权，将授权代码返回给应用程序
*   步骤 2:应用程序发送授权代码来获取访问令牌
*   当我们有一个资源所有者(用户)时使用它

![](img/44a471aa939149ec514ae71672661067.png)

客户端凭据流

这里我们没有资源所有者，它是机器之间的直接通信，所以我们只有一个步骤:**我们发送客户端 ID 和秘密**，我们直接从授权服务器(这里是 Keycloak)取回**访问令牌**

> 此客户端凭据流适用于可以请求访问令牌并自行访问资源的应用程序。这些应用通常使用在没有用户的情况下调用 API 的服务。

当我们使用“机密”客户端类型并单击“保存”时，我们将在我们的客户端 MyApp 页面中获得一个名为“凭证”的新选项卡

![](img/8fcdfb4860b03d34ada80357c2b43d1a.png)

我们在这里得到了秘密

![](img/c82b86cdab441fb70392af1160fdc3f8.png)

所以当我们有一个应用程序可以在服务器上安全地保存 **clientid** 和 **secret** 时，我们可以使用“**secret**客户端，并在 A **授权代码流**中使用“ **client id** 和“ **secret** ”。

> 这个秘密只有应用程序和授权服务器知道。这是应用程序自己的密码。

基本上，当我们的用户需要公共应用程序的认证和授权时:我们使用**授权代码**流和**登录/密码**。该流程由第 3 部分的**标准流程**启用。

*   Part 3):启用**“标准流程”**:基于重定向的标准 OpenID Connect 认证，授权码**。
    用于启用“授权代码流”,稍后将对其进行说明。
    **正是这个授权流将用于保护我们的前端 React SPA。我们稍后会解释原因。****

**![](img/cd1ffe62656f5a6cff05d84c8c17ba84.png)**

**标准流程:使用 **OpenID 连接协议**启用**授权流程****

*   **第 4 部分):启用了**“直接访问授权”**，其中客户端(我们的应用程序)可以获取并使用用户 l **ogin/pwd** 直接从 Keycloak 获取**访问令牌**。在 OAuth 2.0 中，它类似于启用"**资源所有者密码凭证授权**"**

**![](img/e5c2aad23dafe2f8681a0a49b0a7785d.png)**

**在我们完成这一部分后，我们将看到当我们禁用此流时会发生什么。**

*   **第 5 部分):添加了我们的带有通配符*
    [的 SPA URL http://localhost:3000/*](http://localhost:3000/*)为**valid Redirect URIs**
    我们将看到保护我们的公共客户端确实非常重要。
    链接越精确，越安全。因为公共客户端不能存储授权代码流中使用的秘密，所以我们需要这种保护来避免另一个网站访问访问令牌！如果 url 与 keycloak 不匹配，将返回以下错误:**

**![](img/70fa86a595502721fa5e4281e42742a1.png)**

**当未经授权的网站 url 试图连接到 keycloak 服务器时**

**![](img/8d5f16424fe7d7a4b1409c4ad4c6174e.png)**

**我们可以在下面的链接中看到补充解释:**

**[](https://www.thomasvitale.com/keycloak-authentication-flow-sso-client/) [## Keycloak 身份验证流程、SSO 协议和客户端配置

### 在本文中，我将介绍认证流的概念。然后，我将简单地提一下…

www.thomasvitale.com](https://www.thomasvitale.com/keycloak-authentication-flow-sso-client/) 

> 由于对客户端的访问将是公开的，出于[安全原因](http://www.keycloak.org/docs/latest/server_admin/index.html#_unspecific-redirect-uris)，我们必须通过正确设置重定向 URIs 来限制它。Keycloak 从*根 URL* 自动生成它们，但是我们可以添加更多。我们越具体越好。

*   第 6 部分):添加了应用程序的 URI。

> 该选项处理[跨产地资源共享(CORS)](https://fetch.spec.whatwg.org/) 。如果浏览器 JavaScript 试图向服务器发出 AJAX HTTP 请求，而该服务器的域与 JavaScript 代码来自的域不同，则该请求必须使用 CORS。服务器必须处理 CORS 请求，否则浏览器不会显示或允许处理请求。该协议可以抵御 XSS、CSRF 和其他基于 JavaScript 的攻击。

![](img/7ea2866e2e3489095a5ad7f359486220.png)

所以基本上 keycloak 为我们提供了以下两个流程:

*   **授权代码流**不使用 secret(公共客户端)
    我们将使用用户的登录名和密码。
*   **资源所有者密码凭证授予**

希望这一部分没有混淆，如果你需要更多的细节，请发邮件给我。

让我们看看如果禁用**资源所有者密码凭证授权会发生什么。**

我们可以看到，一旦 Keycloak 服务器验证了 login/pwd，就会直接颁发访问令牌。

![](img/7ef2f3c0ed512e935fa66a4c0cf37469.png)

**资源所有者密码凭证授予图**

让我们打开我们的 Insomia API 客户端应用程序并发送我们的请求:

![](img/140edc4f55d74c19674b58b93dbb37c0.png)

授予资源所有者口令身份证明

我们发出了请求

![](img/ee33252b9dd24fd1e70e35f0cc27ab7f.png)

我们发送:

*   grant_type:应用授权类型(或流)是应用可以通过其获得[访问令牌](https://auth0.com/docs/secure/tokens/access-tokens)的方法

以下是可能值的列表:

![](img/c24ff66ea24df1c0f9ce5678a2277b9f.png)

我们必须意识到，只有高度可信的应用程序才能使用资源所有者密码流。

让我们在服务器上禁用这个流:

![](img/34ca48497d0f52b4472c7de2a1323e1b.png)

关闭直接访问授权

![](img/8141957b356102a52c8fc9f1d066aed6.png)

禁用并保存

如果我们再次尝试，我们会得到以下错误:

![](img/8fbbc3b6b36851eb98da171bad95631b.png)

不允许客户端进行直接访问授权

不再允许客户端 MyApp 使用此授权对自己进行身份验证和授权。它将不得不使用另一个授权:授权码授权。

我们将使用授权码授权来保护我们的 React SPA 应用程序

# 使用 OpenID Connect procotol 保护 React SPA 应用程序

什么是 React SPA 应用程序？

它是一个前端应用程序，采用特殊调制的 HTML + CSS + Javascript，使其开发更快、更简单、更高效、更具可扩展性。

所以我们的代码是**客户端**上的 **Javascript** 。

让我们看看 Keycloak 网站上有什么可以用 **OpenID 连接协议**来保护它。

 [## 保护应用程序和服务指南

### 编辑描述

www.keycloak.org](https://www.keycloak.org/docs/latest/securing_apps/index.html) 

在这里，我们可以找到关于保护应用和服务的完整指南。

我们将查看客户端应用程序，因此我们单击

 [## 保护应用程序和服务指南

### 编辑描述

www.keycloak.org](https://www.keycloak.org/docs/latest/securing_apps/index.html#client-adapters) ![](img/02fe2a9a050648b325f483acf2853b78.png)

OpenID 连接客户端 Javascript 适配器

 [## 保护应用程序和服务指南

### 编辑描述

www.keycloak.org](https://www.keycloak.org/docs/latest/securing_apps/index.html#_javascript_adapter) 

> Keycloak 自带客户端 JavaScript 库，可用于保护 HTML5/JavaScript 应用。
> 
> 该库可以在`/js/keycloak.js`直接从 Keycloak 服务器中检索

让我们检查一下:

我们将 GET Http 方法请求发送到:

[http://localhost:8080/js/key cloak . js](http://localhost:8080/js/keycloak.js)

![](img/fc8b5877f9253d6c25fe5c82815e38d0.png)

Javascript Keycloak 适配器

这是一个 2405 行的大 js 文件，是我写这篇博客时最新版本 18.0.0 的服务器密匙锁:

![](img/a4c034b0a89f7afe0882f40873d83e21.png)

现在我们知道可以使用这个文件来帮助我们保护任何使用 javascript 的前端 web 应用程序。

但是我们如何在 React 应用程序中使用它呢？

首先我们需要一个文件:

*   按照说明下载 keycloak.json:

> 点击`Installation`选项卡选择`Keycloak OIDC JSON`为`Format Option`，然后点击`Download`。下载的`keycloak.json`文件应该放在你的 web 服务器上，和你的 HTML 页面放在同一个位置。

![](img/08aab0cc9e09d78658c3285cec2eacb6.png)![](img/36d9d50fb1cdb37a87f0aeef33c895f3.png)

“安装”选项卡帮助我们获取配置文件。

![](img/e4e465066ef8382f2dc669ccb3e779fa.png)

我们选择奇克劳 OIDC JSON

![](img/728185b72f902f7f669a922a6f92dcca.png)

我们下载我们的配置文件

我们将 keycloak.json 添加到 React 项目的 **public** 目录的根目录下。

最后，我们得到以下项目结构:

![](img/033f17828b24217dabdf919b3a7690e5.png)

Keycloak.json 在 **public** 目录中

现在我们准备编码了！

我们将使用**可视代码**，但是您可以自由使用任何 IDE:

让我们用可视代码打开终端

# 在 React 中添加 Keycloak Javascript 适配器

我们需要在 React 项目中添加 Javascript Keycloak 适配器。

为此，我们将使用国家预防机制。

在谷歌上简单搜索一下就会得到:

[https://www.npmjs.com/package/keycloak-js](https://www.npmjs.com/package/keycloak-js)

在这里我们可以找到它，好消息是它还包含了内置的 typescript 声明，非常方便！

[](https://www.npmjs.com/package/keycloak-js) [## keycloak-js

### 键盘锁适配器。最新版本:18.0.0，最后发布时间:一个月前。通过以下方式开始在您的项目中使用 key cloak-js…

www.npmjs.com](https://www.npmjs.com/package/keycloak-js) 

此外，我们可以检查它是否与 Keycloak 服务器版本相匹配。

![](img/0b7ecad94a42093aac37acf6cbe814e3.png)

让我们用下面的命令安装它:

```
C:\DevRoot\myapp>npm i keycloak-js@18.0.0 
npm WARN [@apideck/better-ajv-errors](http://twitter.com/apideck/better-ajv-errors)@0.3.3 requires a peer of ajv@>=8 but none is installed. You must install peer dependencies yourself.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@2.3.2 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.3.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})+ keycloak-js@18.0.0
added 3 packages from 3 contributors and audited 1434 packages in 13.281s182 packages are looking for funding
  run `npm fund` for detailsfound 1 high severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
```

![](img/c3e9c3e114609c1107d46ffa4f4bb188.png)

keycloak-js

现在我们可以在应用程序中导入 keycloak js 适配器特性。为此，我们将通过创建带有接口的服务来享受 Typescript。

它将使在任何地方使用它和改变它的实现变得更加容易。

首先，让我们为 keycloak 服务创建一个名为“ **security** 的文件夹，这个服务实例将使我们在 React 项目中的任何地方使用它变得更加容易。

让我们添加名为“KeycloakService.tsx”的 tsx 文件

![](img/9225aa2825cf3c76ea6855f3cc4c988c.png)

KeyCloakService

我们需要知道 Keycloak JS API，请去链接

[](https://github.com/keycloak/keycloak-documentation/blob/main/securing_apps/topics/oidc/javascript-adapter.adoc) [## keycloak-documentation/JavaScript-adapter . adoc 位于主 keycloak/keycloak-documentation

### 在 GitHub 上创建一个帐户，为 keycloak/keycloak 文档开发做贡献。

github.com](https://github.com/keycloak/keycloak-documentation/blob/main/securing_apps/topics/oidc/javascript-adapter.adoc) 

我们添加以下代码:

*   首先我们**从 keycloak js 导入**key cloak 类
*   我们创建一个**实例** keycloakInstance
*   我们创建一个**函数**登录，如果用户通过了身份验证，就调用这个函数。这里，该函数将显示 React 应用程序的主要组件。
    要理解这一部分，请查阅链接
    [https://www . key cloak . org/docs/latest/securing _ apps/index . html # JavaScript _ adapter](https://www.keycloak.org/docs/latest/securing_apps/index.html#_javascript_adapter)

非常重要的是要记住，文档说在[https://www . key cloak . org/docs/latest/securing _ apps/index . html # _ JavaScript _ implicit _ flow](https://www.keycloak.org/docs/latest/securing_apps/index.html#_javascript_implicit_flow)

> 默认情况下，JavaScript 适配器使用[授权代码](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)流。

**因此，我们确保客户端可以通过使用授权代码流与服务器端通信，默认情况下，Keycloak Javascript 适配器使用我们在 Keycloak 服务器配置上为客户端“MyApp”启用的授权代码流**

我们不必对代码流做任何事情。

现在我们已经准备好确保它能够工作:)在我们的 index.tsx 文件中

![](img/246b1157d2a048df7ddbacb46de13906.png)

使用 Keycloak 服务保护 React 应用程序

我们导入 KeyCloakService 并创建一个名为 renderApp 的函数。
**这个 renderApp 只有在用户通过认证的情况下才会被调用！**

因此，在 React 应用程序可以呈现之前，keycloak 服务将触发 CallLogin，我们将看到会发生什么。

让我们用 VS 代码运行 React 应用程序。

让我们打开我们的**包**

![](img/c2820ac532af3cdb43a1f0b594eee147.png)

调试

我们看到一个叫做“调试”的部分，里面有我们需要的所有脚本。

我们将使用脚本/启动

点击“调试”

![](img/2336d5bcf59faf307866b5678c6b9a4d.png)

点击“开始”运行反应-脚本开始

![](img/886c64a664cd3abe32ff926e5c20bdcc.png)

它将打开终端并运行

![](img/d55b2c69bd8950116c994693e421faf2.png)

最后，浏览器打开，终端返回:

![](img/1e6735f95a44c498c59d02824301fe61.png)

你在浏览器中看到了什么？:)

![](img/b5e4e088231b090aaa04d5ecbd1103e0.png)

从 React 应用程序重定向到 Keycloak 登录表单

我们的 react 应用程序受到 keycloak 的保护。

让我们看看网址

```
[http://localhost:8080/realms/**MyRealm**/**protocol**/**openid-connect**/**auth**?**client_id**=**MyApp**&**redirect_uri**=http%3A%2F%2Flocalhost%3A3000%2F&state=39ebab78-b871-43ce-a30a-b01cce35b83d&response_mode=fragment&**response_type=code**&**scope=openid**&nonce=e8f025e1-c1fc-4551-8bb6-2c1239b01a79](http://localhost:8080/realms/MyRealm/protocol/openid-connect/auth?client_id=MyApp&redirect_uri=http%3A%2F%2Flocalhost%3A3000%2F&state=39ebab78-b871-43ce-a30a-b01cce35b83d&response_mode=fragment&response_type=code&scope=openid&nonce=e8f025e1-c1fc-4551-8bb6-2c1239b01a79)
```

您可以看到领域“我的领域”，client_id 是我们在之前的博客中设置的 MyApp。

在 url 中，我们可以看到主要部分:

*   Keycloak js 适配器调用 auth 端点对用户进行身份验证[http://localhost:8080/realms/my realm/protocol/OpenID-connect/auth](http://localhost:8080/realms/MyRealm/protocol/openid-connect/auth):端点用表单对用户进行身份验证
*   协议是 openid-connect
*   客户端 id 是 MyApp
*   redirect_uri 是我们的 URI:[http://loca host:3000](http://locahost:3000)
*   response_type = code 我们告诉服务器我们将使用授权码授权
*   也就是说 keycloak 将返回 id 令牌和访问令牌。ID 令牌是 OAuth 2.0 协议上 OpenID Connect 协议的扩展。

在流程的最后，我们得到对授权代码流 POST 的最终请求的响应:[http://localhost:8080/realms/my realm/protocol/OpenID-connect/token](http://localhost:8080/realms/MyRealm/protocol/openid-connect/token)，在这里我们通过与授权代码交换来请求令牌。

![](img/33fe4f957277f45f09a9086fdd344f47.png)

ID 令牌和访问令牌

我们在邮件正文中发送了:

*   代码:授权代码的值
*   授权类型:授权代码流
*   client_id: MyApp
*   redirect_uri:在用户通过身份验证后重定向到我们的应用程序

![](img/c93005bb707577541f4d2ed9c0315120.png)

让我们看看小提琴手完整的舞蹈

步骤 1:用户未通过身份验证，重定向到 Keycloak 进行登录

![](img/0aa5a65bdb45c9a7946c66a517007d2c.png)

这里重要的请求/响应在第 13 行

下面是前面已经研究过的带有预期查询参数的完整请求

```
GET http://localhost:8080/realms/MyRealm/protocol/openid-connect/auth?client_id=MyApp&redirect_uri=http%3A%2F%2Flocalhost%3A3000%2F&state=be332dbe-e492-4133-b397-42fb385a0d9f&response_mode=fragment&response_type=code&scope=openid&nonce=c8fc1ae6-84eb-4332-91e1-9ebdf23447c9 HTTP/1.1Host: localhost:8080Connection: keep-alivesec-ch-ua: " Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"sec-ch-ua-mobile: ?0sec-ch-ua-platform: "Windows"Upgrade-Insecure-Requests: 1User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.67 Safari/537.36Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9Sec-Fetch-Site: same-siteSec-Fetch-Mode: navigateSec-Fetch-Dest: documentReferer: http://localhost:3000/Accept-Encoding: gzip, deflate, brAccept-Language: en-US,en;q=0.9,es;q=0.8,fr;q=0.7,zh-CN;q=0.6,zh;q=0.5,nl;q=0.4Cookie: AUTH_SESSION_ID=d1945748-e67b-448f-b204-a0b804da5740; AUTH_SESSION_ID_LEGACY=d1945748-e67b-448f-b204-a0b804da5740; KC_RESTART=eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyMTllYzkwNi1hYjNlLTQ4YWItOTM2OS0yZDdjYTZhM2QwOWQifQ.eyJjaWQiOiJNeUFwcCIsInB0eSI6Im9wZW5pZC1jb25uZWN0IiwicnVyaSI6Imh0dHA6Ly9sb2NhbGhvc3Q6MzAwMC8iLCJhY3QiOiJBVVRIRU5USUNBVEUiLCJub3RlcyI6eyJzY29wZSI6Im9wZW5pZCIsImlzcyI6Imh0dHA6Ly9sb2NhbGhvc3Q6ODA4MC9yZWFsbXMvTXlSZWFsbSIsInJlc3BvbnNlX3R5cGUiOiJjb2RlIiwicmVkaXJlY3RfdXJpIjoiaHR0cDovL2xvY2FsaG9zdDozMDAwLyIsInN0YXRlIjoiMWUxZGVhZjItMmI0NC00ZWY5LTk1YTktMzcxYWNlNmVkNGNjIiwibm9uY2UiOiIyYjc3YjVlYy05MTNkLTQwMjEtODZkZi0zYTRmNzc5Mjg5MjgiLCJyZXNwb25zZV9tb2RlIjoiZnJhZ21lbnQifX0.0VclmSPhERNprcUZ9oAaknvuvW5UNFM9clJkO9fOZsA
```

我们得到了表单的响应，这是小提琴手的主要预览

![](img/eb74113e93d7016bbe12ea2b417bc82f.png)

在我们的浏览器中，我们得到:

![](img/012980b5b87dd72e6841800589459277.png)

让我们登录网站，跟随《与提琴手共舞》:

步骤 2 和 3:授权码和令牌

![](img/815c1d69aede0c5d009e86f3a53b96d5.png)

步骤 2:让我们看一下第 26 行，我们在那里登录并调用端点身份验证。我们在 POST http 调用中发送了用户名和密码

```
POST http://localhost:8080/realms/MyRealm/login-actions/authenticate?session_code=XgIBUY1cPp8cxmlV90NMm_OvxpAzYqDgS1C5aEMFyZA&execution=bcb665d5-bd68-4918-9aa3-25eb3f06f834&client_id=MyApp&tab_id=tTrW_5TE5yM HTTP/1.1Host: localhost:8080Connection: keep-aliveContent-Length: 45Cache-Control: max-age=0sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"sec-ch-ua-mobile: ?0sec-ch-ua-platform: "Windows"Upgrade-Insecure-Requests: 1Origin: nullContent-Type: application/x-www-form-urlencodedUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.67 Safari/537.36Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9Sec-Fetch-Site: same-originSec-Fetch-Mode: navigateSec-Fetch-User: ?1Sec-Fetch-Dest: documentAccept-Encoding: gzip, deflate, brAccept-Language: en-US,en;q=0.9,es;q=0.8,fr;q=0.7,zh-CN;q=0.6,zh;q=0.5,nl;q=0.4Cookie: AUTH_SESSION_ID=d1945748-e67b-448f-b204-a0b804da5740; AUTH_SESSION_ID_LEGACY=d1945748-e67b-448f-b204-a0b804da5740; KC_RESTART=eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyMTllYzkwNi1hYjNlLTQ4YWItOTM2OS0yZDdjYTZhM2QwOWQifQ.eyJjaWQiOiJNeUFwcCIsInB0eSI6Im9wZW5pZC1jb25uZWN0IiwicnVyaSI6Imh0dHA6Ly9sb2NhbGhvc3Q6MzAwMC8iLCJhY3QiOiJBVVRIRU5USUNBVEUiLCJub3RlcyI6eyJzY29wZSI6Im9wZW5pZCIsImlzcyI6Imh0dHA6Ly9sb2NhbGhvc3Q6ODA4MC9yZWFsbXMvTXlSZWFsbSIsInJlc3BvbnNlX3R5cGUiOiJjb2RlIiwicmVkaXJlY3RfdXJpIjoiaHR0cDovL2xvY2FsaG9zdDozMDAwLyIsInN0YXRlIjoiYmUzMzJkYmUtZTQ5Mi00MTMzLWIzOTctNDJmYjM4NWEwZDlmIiwibm9uY2UiOiJjOGZjMWFlNi04NGViLTQzMzItOTFlMS05ZWJkZjIzNDQ3YzkiLCJyZXNwb25zZV9tb2RlIjoiZnJhZ21lbnQifX0.IwNPJ-OaVGtX-PgLxr1u849Q84LD4RPI87Bt4TzgcL0username=myuser&password=myuser&credentialId=
```

一旦 keycloak 验证了登录/密码，我们就会得到响应

```
HTTP/1.1 302 FoundReferrer-Policy: no-referrerX-Frame-Options: SAMEORIGINStrict-Transport-Security: max-age=31536000; includeSubDomainsX-Robots-Tag: noneCache-Control: no-store, must-revalidate, max-age=0X-Content-Type-Options: nosniffContent-Security-Policy: frame-src 'self'; frame-ancestors 'self'; object-src 'none';Set-Cookie: KEYCLOAK_LOCALE=; Version=1; Comment=Expiring cookie; Expires=Thu, 01-Jan-1970 00:00:10 GMT; Max-Age=0; Path=/realms/MyRealm/; HttpOnlySet-Cookie: KC_RESTART=; Version=1; Expires=Thu, 01-Jan-1970 00:00:10 GMT; Max-Age=0; Path=/realms/MyRealm/; HttpOnlySet-Cookie: KEYCLOAK_IDENTITY=eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyMTllYzkwNi1hYjNlLTQ4YWItOTM2OS0yZDdjYTZhM2QwOWQifQ.eyJleHAiOjE2NTM4NzkyODEsImlhdCI6MTY1Mzg0MzI4MSwianRpIjoiMTEyMjVmMjAtYWFjMC00NjJhLWFmODgtNjNiNDQ4ZWIxZTVjIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9NeVJlYWxtIiwic3ViIjoiOGI1YTc4NjYtZTk2OC00YjIwLTkwMDEtNWZmNWFmNzVmZGNlIiwidHlwIjoiU2VyaWFsaXplZC1JRCIsInNlc3Npb25fc3RhdGUiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJzaWQiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJzdGF0ZV9jaGVja2VyIjoiZWJmbWZGbzFldHRnVU5ybDhjVjhKZjNsVko2SnQ0Tk9TbC1JWlg2YjRrcyJ9.tqPAjEiivavLO5I8JPl-62Sn66Dvvpd-95Ymsp7vHo0; Version=1; Path=/realms/MyRealm/; SameSite=None; Secure; HttpOnlySet-Cookie: KEYCLOAK_IDENTITY_LEGACY=eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyMTllYzkwNi1hYjNlLTQ4YWItOTM2OS0yZDdjYTZhM2QwOWQifQ.eyJleHAiOjE2NTM4NzkyODEsImlhdCI6MTY1Mzg0MzI4MSwianRpIjoiMTEyMjVmMjAtYWFjMC00NjJhLWFmODgtNjNiNDQ4ZWIxZTVjIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9NeVJlYWxtIiwic3ViIjoiOGI1YTc4NjYtZTk2OC00YjIwLTkwMDEtNWZmNWFmNzVmZGNlIiwidHlwIjoiU2VyaWFsaXplZC1JRCIsInNlc3Npb25fc3RhdGUiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJzaWQiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJzdGF0ZV9jaGVja2VyIjoiZWJmbWZGbzFldHRnVU5ybDhjVjhKZjNsVko2SnQ0Tk9TbC1JWlg2YjRrcyJ9.tqPAjEiivavLO5I8JPl-62Sn66Dvvpd-95Ymsp7vHo0; Version=1; Path=/realms/MyRealm/; HttpOnlySet-Cookie: KEYCLOAK_SESSION=MyRealm/8b5a7866-e968-4b20-9001-5ff5af75fdce/d1945748-e67b-448f-b204-a0b804da5740; Version=1; Expires=Mon, 30-May-2022 02:54:41 GMT; Max-Age=36000; Path=/realms/MyRealm/; SameSite=None; SecureSet-Cookie: KEYCLOAK_SESSION_LEGACY=MyRealm/8b5a7866-e968-4b20-9001-5ff5af75fdce/d1945748-e67b-448f-b204-a0b804da5740; Version=1; Expires=Mon, 30-May-2022 02:54:41 GMT; Max-Age=36000; Path=/realms/MyRealm/Set-Cookie: KEYCLOAK_REMEMBER_ME=; Version=1; Comment=Expiring cookie; Expires=Thu, 01-Jan-1970 00:00:10 GMT; Max-Age=0; Path=/realms/MyRealm/; HttpOnlyP3P: CP="This is not a P3P policy!"X-XSS-Protection: 1; mode=blockLocation: http://localhost:3000/#state=be332dbe-e492-4133-b397-42fb385a0d9f&session_state=d1945748-e67b-448f-b204-a0b804da5740&code=618cefce-9582-4e5a-bd98-207b22e06a75.d1945748-e67b-448f-b204-a0b804da5740.c6015bba-a545-4114-841c-3a3425c001b2content-length: 0
```

它返回 HTTP 代码 302 重定向到[HTTP://localhost:3000/# state = be 332 DBE-e492-4133-b397-42fb 385 a0d 9 f&session _ state = d 1945748-e67b-448 f-b204-a0b 804 da 5740&code = 618 cef ce-9582-4e5a-bd98-207 B2](http://localhost:3000/#state=be332dbe-e492-4133-b397-42fb385a0d9f&session_state=d1945748-e67b-448f-b204-a0b804da5740&code=618cefce-9582-4e5a-bd98-207b22e06a75.d1945748-e67b-448f-b204-a0b804da5740.c6015bba-a545-4114-841c-3a3425c001b2)

我们可以在网址中看到**授权码**:
c[ode = 618 cef ce-9582–4e5a-bd98–207 b 22 e 06 a 75 . d 1945748-e67b-448 f-b204-a0b 804 da 5740 . c 6015 BBA-a545–4114–841 c-3a 3425 c 001 b 2](http://localhost:3000/#state=be332dbe-e492-4133-b397-42fb385a0d9f&session_state=d1945748-e67b-448f-b204-a0b804da5740&code=618cefce-9582-4e5a-bd98-207b22e06a75.d1945748-e67b-448f-b204-a0b804da5740.c6015bba-a545-4114-841c-3a3425c001b2)

步骤 3:一旦我们回到我们的应用程序，Keycloak JS 继续发送/接收其他请求/响应，直到第 45 行，在那里我们得到授权代码流的最后步骤 3:如果 Keycloak 可以验证，交换授权代码以取回令牌作为响应。

```
POST http://localhost:8080/realms/MyRealm/protocol/openid-connect/token HTTP/1.1Host: localhost:8080Connection: keep-aliveContent-Length: 207sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"sec-ch-ua-mobile: ?0User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.67 Safari/537.36sec-ch-ua-platform: "Windows"Content-type: application/x-www-form-urlencodedAccept: */*Origin: http://localhost:3000Sec-Fetch-Site: same-siteSec-Fetch-Mode: corsSec-Fetch-Dest: emptyReferer: http://localhost:3000/Accept-Encoding: gzip, deflate, brAccept-Language: en-US,en;q=0.9,es;q=0.8,fr;q=0.7,zh-CN;q=0.6,zh;q=0.5,nl;q=0.4Cookie: AUTH_SESSION_ID=d1945748-e67b-448f-b204-a0b804da5740; AUTH_SESSION_ID_LEGACY=d1945748-e67b-448f-b204-a0b804da5740; KEYCLOAK_IDENTITY=eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyMTllYzkwNi1hYjNlLTQ4YWItOTM2OS0yZDdjYTZhM2QwOWQifQ.eyJleHAiOjE2NTM4NzkyODEsImlhdCI6MTY1Mzg0MzI4MSwianRpIjoiMTEyMjVmMjAtYWFjMC00NjJhLWFmODgtNjNiNDQ4ZWIxZTVjIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9NeVJlYWxtIiwic3ViIjoiOGI1YTc4NjYtZTk2OC00YjIwLTkwMDEtNWZmNWFmNzVmZGNlIiwidHlwIjoiU2VyaWFsaXplZC1JRCIsInNlc3Npb25fc3RhdGUiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJzaWQiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJzdGF0ZV9jaGVja2VyIjoiZWJmbWZGbzFldHRnVU5ybDhjVjhKZjNsVko2SnQ0Tk9TbC1JWlg2YjRrcyJ9.tqPAjEiivavLO5I8JPl-62Sn66Dvvpd-95Ymsp7vHo0; KEYCLOAK_IDENTITY_LEGACY=eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyMTllYzkwNi1hYjNlLTQ4YWItOTM2OS0yZDdjYTZhM2QwOWQifQ.eyJleHAiOjE2NTM4NzkyODEsImlhdCI6MTY1Mzg0MzI4MSwianRpIjoiMTEyMjVmMjAtYWFjMC00NjJhLWFmODgtNjNiNDQ4ZWIxZTVjIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9NeVJlYWxtIiwic3ViIjoiOGI1YTc4NjYtZTk2OC00YjIwLTkwMDEtNWZmNWFmNzVmZGNlIiwidHlwIjoiU2VyaWFsaXplZC1JRCIsInNlc3Npb25fc3RhdGUiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJzaWQiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJzdGF0ZV9jaGVja2VyIjoiZWJmbWZGbzFldHRnVU5ybDhjVjhKZjNsVko2SnQ0Tk9TbC1JWlg2YjRrcyJ9.tqPAjEiivavLO5I8JPl-62Sn66Dvvpd-95Ymsp7vHo0; KEYCLOAK_SESSION=MyRealm/8b5a7866-e968-4b20-9001-5ff5af75fdce/d1945748-e67b-448f-b204-a0b804da5740; KEYCLOAK_SESSION_LEGACY=MyRealm/8b5a7866-e968-4b20-9001-5ff5af75fdce/d1945748-e67b-448f-b204-a0b804da5740code=618cefce-9582-4e5a-bd98-207b22e06a75.d1945748-e67b-448f-b204-a0b804da5740.c6015bba-a545-4114-841c-3a3425c001b2&grant_type=authorization_code&client_id=MyApp&redirect_uri=http%3A%2F%2Flocalhost%3A3000%2F
```

我们调用端点**令牌**来将**授权码交换给令牌**

我们发送 POST HTTP 请求

*   **授权码**code = 618 ce FCE-9582–4e5a-bd98–207 b 22 e 06 a 75 . d 1945748-e67b-448 f-b204-a0b 804 da 5740 . c 6015 BBA-a545–4114–841 c-3a 3425 c 001 b
*   授权类型=授权代码
*   client_id=MyApp
*   redirect_uri 我们的 React 应用程序 uri，在进入 keycloak 服务器之前，我们从这里被重定向

作为回应，我们得到:

```
HTTP/1.1 200 OKReferrer-Policy: no-referrerX-Frame-Options: SAMEORIGINAccess-Control-Expose-Headers: Access-Control-Allow-MethodsStrict-Transport-Security: max-age=31536000; includeSubDomainsCache-Control: no-storeAccess-Control-Allow-Origin: http://localhost:3000Access-Control-Allow-Credentials: trueX-Content-Type-Options: nosniffPragma: no-cacheX-XSS-Protection: 1; mode=blockContent-Type: application/jsoncontent-length: 3426{"access_token":"eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJFMUk0RHpMWHUzUTRqMm80ZHdSRFBSOVBGUzd6bEw2MjdOaGtiSUl5WkQ0In0.eyJleHAiOjE2NTM4NDM1ODYsImlhdCI6MTY1Mzg0MzI4NiwiYXV0aF90aW1lIjoxNjUzODQzMjgxLCJqdGkiOiI4MjI4NjM1OC1kZmVhLTQwMGMtOGVkNy05Yzg3NjFkMWU1YmQiLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvcmVhbG1zL015UmVhbG0iLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiOGI1YTc4NjYtZTk2OC00YjIwLTkwMDEtNWZmNWFmNzVmZGNlIiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiTXlBcHAiLCJub25jZSI6ImM4ZmMxYWU2LTg0ZWItNDMzMi05MWUxLTllYmRmMjM0NDdjOSIsInNlc3Npb25fc3RhdGUiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJhY3IiOiIxIiwiYWxsb3dlZC1vcmlnaW5zIjpbImh0dHA6Ly9sb2NhbGhvc3Q6MzAwMCJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1teXJlYWxtIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoib3BlbmlkIHByb2ZpbGUgZW1haWwiLCJzaWQiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsInByZWZlcnJlZF91c2VybmFtZSI6Im15dXNlciJ9.B2F_-pSTh6phS7TZJeS6-ZUgT31uEYQPbtV9oxjVdIKVeQWadWCyV-P2H0gY5oo7VWUhvtcqib_IK3fm41TxDxatlHkysFX5FVsvUoilkIeRp1vQDEhz745NddspVIZzH5u87TdFVuKH5Abv3Vr410sYAQeugzuCi28sbcgIsXzleX_LuUpOmaewz71j5zrkDGTz2qM9xo3D1g74ZuUegXsm_XKjDLqDEKHkJTa8YJvUBaf_iMfM-ZXI91Z41l9zP4rFuYqwFgLxvu-FdgN7jsW4rUGohsqyaNBJ_GCb0F8HYZRGCKuIkgZj-Zn6zkgbQetB7oMoB0ueEAtJd8aO2A","expires_in":300,"refresh_expires_in":1800,"refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyMTllYzkwNi1hYjNlLTQ4YWItOTM2OS0yZDdjYTZhM2QwOWQifQ.eyJleHAiOjE2NTM4NDUwODYsImlhdCI6MTY1Mzg0MzI4NiwianRpIjoiMDg1ZTJmZWQtZDgxZi00OTcxLWE0ZWEtODQxMmJiNWY4MDgzIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9NeVJlYWxtIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9NeVJlYWxtIiwic3ViIjoiOGI1YTc4NjYtZTk2OC00YjIwLTkwMDEtNWZmNWFmNzVmZGNlIiwidHlwIjoiUmVmcmVzaCIsImF6cCI6Ik15QXBwIiwibm9uY2UiOiJjOGZjMWFlNi04NGViLTQzMzItOTFlMS05ZWJkZjIzNDQ3YzkiLCJzZXNzaW9uX3N0YXRlIjoiZDE5NDU3NDgtZTY3Yi00NDhmLWIyMDQtYTBiODA0ZGE1NzQwIiwic2NvcGUiOiJvcGVuaWQgcHJvZmlsZSBlbWFpbCIsInNpZCI6ImQxOTQ1NzQ4LWU2N2ItNDQ4Zi1iMjA0LWEwYjgwNGRhNTc0MCJ9.i1WadD6KDRmAcJAPV2s8aYLd6VXR6YWO0l-a-TVGP9c","token_type":"Bearer","id_token":"eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJFMUk0RHpMWHUzUTRqMm80ZHdSRFBSOVBGUzd6bEw2MjdOaGtiSUl5WkQ0In0.eyJleHAiOjE2NTM4NDM1ODYsImlhdCI6MTY1Mzg0MzI4NiwiYXV0aF90aW1lIjoxNjUzODQzMjgxLCJqdGkiOiI0OTRkZDc4YS04YTY5LTQzYzItOWQ3OS05MWFkNzU1ZmU5NjkiLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvcmVhbG1zL015UmVhbG0iLCJhdWQiOiJNeUFwcCIsInN1YiI6IjhiNWE3ODY2LWU5NjgtNGIyMC05MDAxLTVmZjVhZjc1ZmRjZSIsInR5cCI6IklEIiwiYXpwIjoiTXlBcHAiLCJub25jZSI6ImM4ZmMxYWU2LTg0ZWItNDMzMi05MWUxLTllYmRmMjM0NDdjOSIsInNlc3Npb25fc3RhdGUiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJhdF9oYXNoIjoiemlUM0NqeDI4WEQtSjVjbWFVV1k1ZyIsImFjciI6IjEiLCJzaWQiOiJkMTk0NTc0OC1lNjdiLTQ0OGYtYjIwNC1hMGI4MDRkYTU3NDAiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsInByZWZlcnJlZF91c2VybmFtZSI6Im15dXNlciJ9.V6ZJKLECBOlKF0qisF19Y8xX9kTf7CZgHLz0efxmr63ZOSRsyZQwLBnoWH00E-pFIGHkdK5ugRGNU36N7xX1tzDvf6sOUIAAj7IUokaAxeYZOtjQL5ZTFgS1qZqEH7kUi27NaCc-z2mOrxF3nD6fSVqZcJ2jx-Wd5-nNvRCMpyEx0j5UtrB4aQP9XJddaJI4P19QjWicNTf1XBeProRKSoFsZ8W3wF0PCaCTV0hsF5rHyyvEP2csAAn1PZ7_Sv9TS0dI2_KjcxiiDCsMuBQnJI53vjAQkpm289dT20BUq6DXnJwLjeYsvO3EGAmpieqUIRTGx6JZ6WvZ-XVQp5_ctA","not-before-policy":0,"session_state":"d1945748-e67b-448f-b204-a0b804da5740","scope":"openid profile email"}
```

我们可以看到 ID 和访问令牌

视觉视图下方:

![](img/9f2c755f05e9a27911f84143e188417d.png)

我们还可以看到令牌的到期时间。

所以使用 fiddler，我们可以确保使用了**授权代码流**。

最后，我们可以进入 React 应用程序页面！

![](img/2b5f4c3ec16cac87d34af50a41a2df11.png)

使用 Keycloak 验证用户身份后，访问 React 应用程序

# 获取 React 应用程序的用户信息，注销和单点登录

在第 2 部分的最后一部分，我们将向您展示如何显示:

*   用户名
*   用户角色

在我们的 React 应用上。

我们将向您展示如何从我们的 React 应用程序注销。

最后，我们将通过在我们的 React 应用程序之外注销来向您证明 SSO 是有效的。

在我们的欢迎页面上，我们将:

*   添加用户名和角色
*   添加注销链接

记得在我们的 Keycloak 服务中，我们只添加了一个方法:CallLogin

我们将添加一个新方法 GetUserName 来获取我们的用户登录:

![](img/5c0fce9ba555864be709d38d42878f9f.png)

现在，让我们在应用程序组件的欢迎页面中使用它:

![](img/94a1b3ebb3825c4f9ea632f3a150ef72.png)

我们得到以下消息

![](img/29a0902f41c874f9d950b5f67111dd0b.png)

现在让我们看看角色

![](img/0ab66a269a22284aa9b82c1671dbad4e.png)

让我们使用它

![](img/4acde050f4317a616a154a18ce5de07b.png)

我们得到了结果

![](img/e2c01a1825cf7fb1cfa449fcc6f9521d.png)

所以我们可以看到获取所有用户信息是多么容易。
角色由访问令牌返回，访问令牌由 Keycloak js 适配器解析并返回。

最后，我们将添加一个按钮来注销用户，但首先我们需要在我们的 Keycloak 服务中添加方法:

![](img/34c296d052a109b73fd5d2cdbaf56712.png)

让我们添加调用注销的按钮:

![](img/1299cdd672484d1ab05ab34e0a657b90.png)

我们看到屏幕:

![](img/ffec244c8c2921b945dcb79163e6dc74.png)

当我们点击注销时，我们返回到

![](img/7041f9261bc7ef61937e54c91f8d8cf7.png)

最后，让我们证明我们使用了单点登录。

我们通过点击“登录”再次登录

我们回到

![](img/d3cfc7f2ec470efaca536b4325e2ca32.png)

让我们转到 Keycloak 服务器:

![](img/e65f5e73466a27fb9c10b09b2971fe6c.png)

对于我们的应用程序 MyApp，我们可以看到会话的数量，也就是登录的用户数量。

我们可以点击 MyApp

![](img/d0cc87256edc0549a393f3b42639e0a3.png)

我们可以通过点击“显示会话”来查看所有会话

如果我们单击会话中的“注销全部”，我们将使用户注销。

![](img/7658af222420aadebb68a4810dab7a14.png)

所有会话都将被删除。

当我们回到我们的应用程序时，用户不再登录。

![](img/dbcb2a95c867c96c8d6204de7b7e2768.png)

现在让我们做相反的事情，我们希望登录我们的用户，但是使用 KeyCloak:

我们去[http://localhost:8080/realms/my realm/account/](http://localhost:8080/realms/MyRealm/account/)

我们得到页面:

![](img/1bcf0c7be2437022a2d506b47cefb403.png)

让我们点击“登录”

![](img/61f5fe1fa83dbfe61c55c01bf66313b8.png)![](img/8da07926a8d47e62eb18ecb70f3d12bc.png)

我们登录后看到

![](img/3590da6d0bdab583211e8506905a6c95.png)

现在，让我们看看进入我们的应用程序后会发生什么:

![](img/98dec67d337ce33fb8c4dd9ccf5c58e8.png)

我们的用户已经登录，因为这两个应用程序:[帐户](http://localhost:8080/admin/master/console/#/realms/MyRealm/clients/412f01ab-3297-47bd-8731-d40fd7344b2e)([http://localhost:8080/realms/my Realm/account/](http://localhost:8080/realms/MyRealm/account/))和 [MyApp](http://localhost:8080/admin/master/console/#/realms/MyRealm/clients/c6015bba-a545-4114-841c-3a3425c001b2) **在同一个领域 MyRealm** ，用户可以一次登录使用这两个应用程序！

下一部分可用:
[第三部分:保护 ASP.NET 核心 C# REST Web](https://medium.com/@barlatiernicolas/security-in-react-and-webapi-in-asp-net-core-c-with-authentification-and-authorization-by-keycloak-f890d340d093)

# ***参考文献***

[](https://blogs.infinitesquare.com/posts/web/open-id-connect-et-oauth-les-differents-flow-de-connexion) [## OpenId Connect et OAuth:comment choisir son flow de connection？

### 在总统的文章中，我们用一种开放的方式进行评论。在 cet 文章中，我看到了…

blogs.infinitesquare.com](https://blogs.infinitesquare.com/posts/web/open-id-connect-et-oauth-les-differents-flow-de-connexion) [](https://www.oauth.com/oauth2-servers/single-page-apps) [## 单页应用程序- OAuth 2.0 简化版

### 加载 JavaScript 和 HTML 后，单页应用程序(也称为基于浏览器的应用程序)完全在浏览器中运行…

www.oauth.com](https://www.oauth.com/oauth2-servers/single-page-apps) [](https://www.keycloak.org/docs/latest/server_admin/index.html#_authentication-flows) [## 服务器管理指南

### 用户用户是能够登录到您的系统的实体。它们可以有与自身相关的属性…

www.keycloak.org](https://www.keycloak.org/docs/latest/server_admin/index.html#_authentication-flows) [](https://docs.vmware.com/en/Single-Sign-On-for-VMware-Tanzu-Application-Service/1.14/sso/GUID-grant-types.html) [## OAuth 2.0 授权类型

### 本主题说明 OAuth 2.0 授权类型如何与不同的应用程序类型配合使用。授权码授权类型是…

docs.vmware.com](https://docs.vmware.com/en/Single-Sign-On-for-VMware-Tanzu-Application-Service/1.14/sso/GUID-grant-types.html) [](https://auth0.com/docs/get-started/applications/application-grant-types#available-grant-types) [## 申请授权类型

### 应用程序授权类型(或流)是一些方法，应用程序可以通过这些方法获得访问令牌，并且您可以通过这些方法授予…

auth0.com](https://auth0.com/docs/get-started/applications/application-grant-types#available-grant-types) [](https://auth0.com/docs/get-started/authentication-and-authorization-flow/resource-owner-password-flow) [## 资源所有者密码流

### 虽然我们不建议这样做，但是高度可信的应用程序可以使用资源所有者密码流(在 OAuth 中定义…

auth0.com](https://auth0.com/docs/get-started/authentication-and-authorization-flow/resource-owner-password-flow) [](https://github.com/keycloak/keycloak-documentation/blob/main/securing_apps/topics/oidc/javascript-adapter.adoc) [## keycloak-documentation/JavaScript-adapter . adoc 位于主 keycloak/keycloak-documentation

### 在 GitHub 上创建一个帐户，为 keycloak/keycloak 文档开发做贡献。

github.com](https://github.com/keycloak/keycloak-documentation/blob/main/securing_apps/topics/oidc/javascript-adapter.adoc)**