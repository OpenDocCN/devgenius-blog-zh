# 如何用 Nestjs 秒实现苹果登录

> 原文：<https://blog.devgenius.io/how-to-implement-apple-login-with-nestjs-in-seconds-b88f05abe847?source=collection_archive---------2----------------------->

![](img/cf8bd24dc5cb5cc6a940cf8af2ccb7fc.png)

Nestjs 是一个用于构建高效、可伸缩的 Node.js 服务器端应用程序的框架。它使用现代 JavaScript，用 TypeScript 构建(保持与纯 JavaScript 的兼容性)，结合了 OOP(面向对象编程)、FP(函数式编程)和 FRP(函数式反应编程)的元素。

Nestjs 的目标是提供一个开箱即用的应用程序架构，允许轻松创建高度可测试、可伸缩、松散耦合和易于维护的应用程序。该建筑深受 Angular 的启发，如果你熟悉 Angular，你会爱上 Nestjs。

# 用护照认证

关于认证，它使用最流行的 node.js 认证库 Passport 来完成认证工作。使用 [@nestjs/passport](http://twitter.com/nestjs/passport) 模块将这个库与 Nest 应用程序集成起来非常简单。如果是第一次见 Passport 和 Nestjs，可以在 [Passport 官网](https://passportjs.org)和 Nestjs 的[文档](https://nestjs.com)上了解更多信息。

# 苹果登录

我假设您已经熟悉了关于 Passport 和 Nestjs 认证的知识，让我们开始深入研究 Apple login API 的实现。

苹果公司正采取坚定的立场，通过这项新功能来保护用户的隐私。他们不会让应用程序看到用户的真实电子邮件地址，而是为应用程序提供每个应用程序唯一的代理电子邮件地址。应用程序开发人员仍然可以向代理地址发送电子邮件，这只是意味着开发人员将无法使用电子邮件地址在应用程序之间关联用户，这也意味着用户可以关闭每个应用程序的电子邮件转发。

令我惊讶的是，这个星球上几乎没有文章提到如何在 Nestjs 中实现苹果登录，我们可以很容易地找到一些关于脸书，谷歌和 Twitter 实现的帖子…，然而很难找到一个人告诉你苹果登录需要做什么，老实说，我完成了一些与脸书/谷歌相关的 API，这对于各种护照策略来说非常简单。难以置信的是，我花了几十个小时完成苹果登录。原因是市场上的 Passport 策略不适合 Nestjs 的方法，另一个原因是苹果为用户提供了一种登录应用程序的方式，而不必依赖脸书或 Twitter 等外部身份提供商。

苹果采用了现有的开放标准 OAuth 2.0 和 OpenID Connect 作为他们新 API 的基础。虽然他们在文档中没有明确地调用 OAuth 或 OIDC，但是他们使用了所有相同的术语和 API 调用。这意味着，如果你熟悉这些技术，你应该没有问题立即使用苹果登录！

让我们来构建一个简短的示例 Nestjs 应用程序，它可以利用苹果的新 API 让用户登录。

整个过程中令人头疼的部分是在苹果开发者门户注册一个应用程序。通常，OAuth 提供者有一个相对简单的过程来注册一个新的应用程序，它会给你一个客户端 ID 和密码。但由于苹果的 API 与他们的整个生态系统紧密相连，所以有点复杂。他们还选择使用公钥/私钥客户端身份验证方法，而不是传统的客户端密码。但是不要担心，感谢神奇的护照策略，你会很容易通过它。

在准备工作结束时，您将获得一个注册的`***client_id***`(他们称之为服务 ID)，一个作为文件下载的私钥，您将验证一个域并为应用程序设置一个重定向 URL。

注意:这篇文章是为那些有一个要求用户登录苹果的前台应用程序的人写的。登录工作流在前端处理，用户获得一个特定于其帐户的 id_token。我们将在后端使用这个令牌提取与他的苹果账户相关的 id 和电子邮件。

# 要求

下面是我们用 NestJS 设置 Apple 登录时需要遵循的软件列表:

*   苹果开发者账户
*   HTTPS
*   NodeJS + Nestjs

# 准备:在苹果开发者网站上配置

*   创建一个新的应用程序 ID ( *记下团队 ID*
*   创建一个服务 ID *(记下服务 ID)*
*   创建密钥并下载密钥文件(记下密钥 ID

请使用您的苹果开发者帐户登录[此处](https://developer.apple.com/)进行配置

## 一、创建应用 ID

![](img/33cd8de0b543df024d2155adf8955bc8.png)

从边栏中，选择标识符，然后单击蓝色加号图标。在这第一步中选择应用 id。

![](img/c18caccd01a7c1493ba39a816cdac740.png)

描述不是太重要，但是输入一些描述性的东西。当包 ID 是反向 dns 样式的字符串时，它是最好的。

你需要一个苹果登录的应用 ID，如果你已经有了，就跳过这个。请记下 ***团队 ID*** ，我们在几分钟后的策略设置中需要它。

![](img/d9e926ff3124c6b436b5b9ca141d81f2.png)

请注意，您必须向下滚动到“功能”部分，并像上面一样选中“登录 Apple”。

## 二。创建服务 ID

上一步中的应用程序 ID 是一种收集此应用程序信息的方式。当你只是像这个例子一样做一个简单的 web 应用程序登录体验时，这似乎是多余的，但是一旦你有一个本地应用程序和 web 应用程序，你想在一个单一的登录体验下捆绑在一起，这就更有意义了。

服务 ID 将标识应用程序的特定实例，并用作 OAuth `***client_id***`。

继续创建一个新的标识符并选择服务 id。

![](img/5ad64b38dbfff377fc9c51853f4c9358.png)![](img/cf147eadb7397fa3e21ab008bdecd4d0.png)

您需要点击“继续”和“注册”，然后再次点击您的服务 ID 来打开它。

在此步骤中，您还需要点击 ***旁边的配置按钮，使用 Apple*** 登录。在这里，您将定义应用程序运行的域，以及定义 OAuth 流程中使用的重定向 URL。

![](img/1135b4fcf642ac4e84ef7acb8dd91c0a.png)

请记下标识符，它将是我们的 ***clientID*** ，并勾选“ ***用苹果*** 签到”，您将看到如下弹出窗口:

![](img/9c6a8c75337a903808e9a3d86e31f575.png)![](img/13820d3fc76b99f7f0fcfe3bf7b60806.png)

确保您的关联应用 ID 被选为主要应用 ID。(如果这是您制作的第一个使用 Apple 登录的应用程序 ID，那么它可能已经被选中。)

输入应用程序最终将运行的域名，并输入应用程序的重定向 URL。如果您想跟进这篇博文，那么您可以使用占位符重定向 URL `**https://example-app.com/redirect**`，它将捕获重定向，这样您就可以看到返回的授权代码。

值得注意的是，苹果在这一步不允许使用`**localhost**`URL，如果你输入一个类似`**127.0.0.1**`的 IP 地址，它将在流程的后面失败。这里必须使用一个真实的域。

继续并点击保存，然后继续并注册，直到该步骤全部确认。

如果您不知道如何为本地环境创建 SSL 证书，请遵循以下子任务:

## 子任务:为本地开发创建 SSL 认证

## 1.通过 OpenSSL 创建认证

创建一个目录`**certificates**`，并添加证书文件`**localhost.key**`和`**localhost.crt**`，它们是使用 OpenSSL 生成的:

*Linux/macOS:*

*视窗:*

OpenSSL 可执行文件随用于 Windows 的 [Git](https://git-scm.com/download/win) 一起发布。安装完成后，你会在`**C:\Program Files\Git\mingw64\bin**`中找到 openssl.exe 文件，如果还没有添加的话，你可以把它添加到系统路径环境变量中。

添加环境变量`***OPENSSL_CONF=C:\Program Files\Git\mingw64\ssl\openssl.cnf***`

现在您已经获得了 SSL 认证，接下来我们需要创建一个主机名解析。

## 2.主机名解析

编辑你的主机文件，将你的站点指向`**127.0.0.1**`。

```
***sudo echo ‘127.0.0.1 dev.example.com’ >> /etc/hosts***
```

*Windows* (以管理员身份运行 PowerShell)

```
**Add-Content -Path C:\Windows\System32\drivers\etc\hosts -Value "127.0.0.1 dev.example.com" -Force**
```

更多信息:[如何编辑我的主机文件？](https://phoenixnap.com/kb/how-to-edit-hosts-file-in-windows-mac-or-linux)

## 3.部署到服务器

您可以在项目的根目录下创建一个`**server.js**`,并使用`**node server.js**`运行它，以便在本地测试与苹果集成的登录:

## 三。创建一个密钥

Apple 决定使用公钥/私钥对，而不是使用简单的字符串作为 OAuth 客户端机密，其中客户端机密是签名的 JWT。下一步包括向 Apple 注册一个新的私钥。

返回证书、标识符和配置文件主屏幕，从侧面导航栏中选择密钥。

![](img/e12eabcb1528f870faad28c86e5cd73f.png)

单击蓝色加号图标注册一个新密钥。为您的密钥命名，并选中“使用 Apple 登录”复选框。

![](img/b3ac904d9141d380d441817a618387f4.png)

单击编辑按钮，并选择您之前创建的主要应用 ID。

然后单击继续并注册。现在，点击下载。

![](img/3b6d1c9c5fd832421cb381c0446f8fc8.png)

Apple 将为您生成一个新的私钥，并且只允许您下载一次。请务必保存此文件，因为您以后将无法再恢复它！您下载的文件将在`**.p8**`结束。

请同时记下密钥 ID，它将作为我们的 ***密钥 ID。***

哇，我们完成了整个准备，很“简单”不是吗？:)

你可能注意到我的标题是“如何在几秒钟内用 Nestjs 实现苹果登录？”，但你花了 20 多分钟完成以上工作，我怎么敢说“几秒钟”？我不是诱饵，你做的那十几件事都不是我的工作。

现在，让我们开始在几秒钟内实现苹果登录 API。真正的故事从这里开始:

# 项目设置

本文不会讨论如何从头开始创建一个 Nestjs 项目，如果你没有这方面的经验，你可以按照[和](https://docs.nestjs.com/first-steps)开始。

# 认证要求

首先，您需要安装以下 Passport 包:

```
npm install --save @nestjs/passport passport @nestjs/jwt
```

其次，你需要安装一个非常独特的软件包:

```
npm install --save @arendajaelu/nestjs-passport-apple
```

只有[@ arendajaelu/nestjs-passport-apple](https://www.npmjs.com/package/@arendajaelu/nestjs-passport-apple)可以和 Nestjs 一起工作而不会有任何恐慌。

现在，让我们生成我们需要的组件:

```
$ nest g module apple
$ nest g controller apple
$ touch apple/apple.strategy.ts
```

另外，请将您从苹果开发者网站下载的密钥文件(. p8 文件)复制到您的应用程序文件夹中(在我的示例中，我将它放在应用程序根目录下名为“secret_key”的文件夹中。

您将得到一个类似的文件夹结构:

![](img/4e9cd003758a23198a365eb5a8d627da.png)

然后我创造了。env 放在应用程序文件夹的根目录中，并将前一节中所有标记的 id 放在相应的键中。请注意，ClientID 指的是服务 ID，而不是应用 ID。

```
APPLE_CLIENTID=xx.xxx.xx
APPLE_TEAMID=xxxxxxxx
APPLE_KEYID=xxxxxxxx
APPLE_CALLBACK=https://dev.example.com:3000/auth/apple/redirect
APPLE_KEYFILE_PATH =./secret_key/xxxxx.p8
```

让我们回去重新润色我们的`apple.strategy.ts`

我建议您使用[配置](https://docs.nestjs.com/techniques/configuration)服务从获取您的设置。env 而不是直接通过`process.env`读取。

让我们给我们的控制器`apple.controller.ts`添加两个方法，第一个方法将触发 Apple 登录，第二个方法将处理从 Apple 端发回的数据，供我们注册或登录。

有了`@UseGuards(AuthGuard('apple'))`，我们使用的是`@nestjs/passport`在扩展苹果护照策略时自动为我们提供的`AuthGuard`。

我们来分析一下。我们的 Apple passport 策略有一个默认名称`'apple'`。我们在`@UseGuards()`装饰器中引用这个名字，将它与`**@arendajaelu/nestjs-passport-apple**`包提供的代码关联起来。这用于在我们的应用程序中有多个 Passport 策略的情况下，消除调用哪个策略的歧义(每个策略可能提供一个特定于策略的`AuthGuard`)。虽然到目前为止我们只有一个这样的策略，但我们很快会添加第二个，所以这是消除歧义所需要的。

创建一个服务来处理注册和登录的逻辑。

```
$ nest g service apple
```

然后我们需要从`apple.module.ts`导入 JWT 护照模块，因为我将在`apple.service.ts`中用 JwtService 进行解密。

当我访问[https://dev.example.com:3000/apple/](https://dev.example.com:3000/apple/,)时，你将被重定向到苹果登录，在你输入你的电子邮件和密码后，苹果将回调我们的 URL，我在苹果开发者网站中设置了该 URL。比如说[https://dev.example.com:3000/apple/redirect](https://dev.example.com:3000/apple/redirect)。

我会得到这样的帖子正文:

![](img/576c0fd1f466aef9251ac017fff93561.png)

我在`apple.service.ts`中增加了一个方法，从苹果端解密 id_token，通过 JwtService.decode()获取其数据非常简单，也可以通过一些 https://jwt.io/等 JWT 解码网站查看数据

全部完成！如果你已经在苹果开发者网站上准备好了“登录苹果”的所有 id 和令牌，你只需要几秒钟就可以用苹果 Passport 策略包([@ arendajaelu/nestjs-Passport-Apple](https://www.npmjs.com/package/@arendajaelu/nestjs-passport-apple))实现 API 逻辑。

PS:示例代码旨在描述 Apple Passport 策略如何工作，您需要实现安全保险部分以防止某些类型的攻击。

# **结论**

实现苹果登录的 API 有点棘手，你需要在苹果开发者网站上做很多工作。另一个棘手的问题是，从苹果公司返回的电子邮件可能是苹果公司提供的转发电子邮件地址。我的两点意见是，你需要使用苹果账户 ID 作为关联密钥，而不是电子邮件。

因为 Apple 正在设置从编码邮件到“真实”邮件的重定向，幸运的是，这个编码邮件是固定的。对于发送电子邮件，你可以使用那个看起来很奇怪的电子邮件地址，因为苹果会负责将你发送的任何电子邮件转发到“真实”的电子邮件地址。只要他们关联的 Apple 帐户 ID(真实的电子邮件)仍然有效，该电子邮件就会保持打开状态。

请提醒自己，苹果允许用户通过简单地删除该电子邮件来关闭某个帐户，这对你来说与任何其他电子邮件一样具有相同的风险。你没有办法找到原始的苹果电子邮件，最好的方法是始终使用苹果帐户 id 来关联到你的用户帐户。

你也可以在我的[medium.com](https://medium.com/@joosepwong)上看到这篇文章，在那里我分享了我在软件工程师职业生涯中遇到的问题的解决方案。

如果您还有其他问题，请随时通过 [LinkedIn](https://www.linkedin.com/in/joosepwong/) 联系我。

科图米塞尼！