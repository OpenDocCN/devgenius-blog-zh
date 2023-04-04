# 带 Symfony 保护的脸书日志 Id 按钮

> 原文：<https://blog.devgenius.io/facebook-log-id-button-with-symfony-guard-97afb1dd3142?source=collection_archive---------9----------------------->

![](img/42894fe6c2e6f122a5667ff6635fef3e.png)

我的目标是将**继续脸书**(登录)按钮添加到现有的 Symfony 应用程序中。我没有在这里提供完整的代码。只有创建您的脸书登录按钮的主要部分。以下是我所做的。

# 概观

工作流程概述是:

1.  用户点击脸书登录按钮
2.  脸书认证用户
3.  应用程序向 Symfony 后端发送 facebook 令牌(认证数据)
4.  后端使用 auth data 获取用户数据(姓名和电子邮件),并使用 Symfony 处理程序登录。

所以脸书只在唯一的第一部分是必要的——检查想要登录的人。
我用[守卫](https://symfony.com/doc/current/security/guard_authentication.html)在后台给用户授权。

# 脸书图书馆

在此创建新应用[。](https://developers.facebook.com/apps)

前端需要脸书 Javascript SDK，后端需要 PHP 库。
安装 PHP 脸书库:

```
composer require facebook/graph-sdk
```

将脸书 Javascript SDK 包含到您的模板中。应该是这样的:

```
<script async defer crossorigin="anonymous"
src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v7.0&appId=<your app id here>">
</script>
```

最简单的方法是从**脸书登录** — **快速入门**部分的[脸书开发者](https://developers.facebook.com/apps)页面生成这个片段。

# 前端

接下来，您需要添加一个按钮并处理登录结果。按钮示例:

```
<div class="fb-login-button"
     data-size="large"
     data-button-type="continue_with"
     data-layout="default"
     data-auto-logout-link="false"
     data-use-continue-as="false"
     data-scope="public_profile,email"
     onlogin="fbGetLoginStatus();"
     data-width=""></div>
```

[这里的](https://developers.facebook.com/docs/facebook-login/web/login-button/)是一个按钮配置器。

我用这段代码创建了一个 Javascript 模块:

```
window.fbGetLoginStatus = function () {
    FB.getLoginStatus(function (response) {
        if (isConnected(response)) {
            fbLogIn(response);
        }
    });
}
function isConnected(response) {
    return response.status === 'connected';
}

function fbLogIn(response) {
    let loginForm = document.querySelector('.login-form');
    let input = getHiddenInput("fbAuthResponse", JSON.stringify(response.authResponse));    
    loginForm.appendChild(input);
    loginForm.submit();
}

function getHiddenInput(name, value) {
    let input = document.createElement("input");
    input.setAttribute("type", "hidden");
    input.setAttribute("name", name);
    input.setAttribute("value", value);
}
```

当用户点击**继续脸书*按钮并成功登录时，我会提交一个登录(或注册)表单，并通过 POST 请求将数据发送到后端。

# 后端— Symfony Guard

我已经为通常的登录密码认证设置了一个防护。所以我加上第二个。以下是我的 security.yaml 警卫配置:

```
security:
    firewalls:
        main:
            guard:
                authenticators:
                    - App\Security\LoginFormAuthenticator
                    - App\Security\FacebookAuthenticator
                entry_point: App\Security\LoginFormAuthenticator
```

现在您需要创建保护文件，它扩展了 AbstractFormLoginAuthenticator:

```
class FacebookAuthenticator extends AbstractFormLoginAuthenticator {}
```

FacebookAuthenticator 的主要方法将是:
**支持** —它检查您是否应该使用该保护。我对登录和注册页面使用了相同的逻辑。看起来是这样的:

```
public function supports(Request $request)
{
    $route = $request->attributes->get('_route');
    $isLoginOrRegister = in_array($route, ['app_login', 'app_register']);
    return $isLoginOrRegister 
        && $request->isMethod('POST') 
        && $request->get('fbAuthResponse');
}
```

**getCredentials** —获取脸书认证响应并将其解码为一个数组:

```
public function getCredentials(Request $request)
{
    return json_decode($request->get('fbAuthResponse'), true);
}
```

**onAuthenticationSuccess**—我将用户重定向到登录页面:

```
public function onAuthenticationSuccess(
    Request $request, 
    TokenInterface $token, 
    $providerKey
)
{
    return new RedirectResponse(
        $this->urlGenerator->generate('app_landing')
    );
}
```

为了使用 url 生成器，将其添加到构造函数中(见下文)。

**getUser** —主要部分，从脸书图形 API 获取电子邮件和名称，并返回用户实体:

```
public function getUser($credentials, UserProviderInterface $userProvider)
{
    if (empty($credentials['accessToken'])) {
        throw new CustomUserMessageAuthenticationException('Your message here');
    }

    $fbUser = $this->fbService->getUser($credentials['accessToken']);

    if (empty($fbUser->getEmail())) {
        throw new CustomUserMessageAuthenticationException('Your message here');
    }

    return $userProvider->loadUserByUsername($fbUser->getEmail());
}
```

当用户不存在时，我使用电子邮件和名称进行注册。这里只是登录部分，为了简单起见没有注册。

以下是在 Guard 构造函数中自动连接服务的方法:

```
public function __construct(
    FacebookService $fbService, 
    UrlGeneratorInterface $urlGenerator
)
{
    $this->fbService = $fbService;
    $this->urlGenerator = $urlGenerator;
}
```

我的 fbService 是这样的:

```
<?php

namespace App\Service;

use App\Entity\FacebookUser;
use Facebook\Facebook;

class FacebookService
{
    private $client;

    public function __construct(
        string $fbAppId, 
        string $fbAppSecret, 
        string $fbGraphVersion
    )
    {
        $this->client = new Facebook([
            'app_id' => $fbAppId,
            'app_secret' => $fbAppSecret,
            'default_graph_version' => $fbGraphVersion,
        ]);

    }

    public function getUser(string $token): FacebookUser
    {
        $user = new FacebookUser();

        try {
            $fbUser = $this->client->get("/me?fields=name,email", $token);
            $data = $fbUser->getDecodedBody();
            $user
                ->setName($data['name'])
                ->setEmail($data['email']);
        } catch (\Throwable $exception) {
            // handle exception here
        }

        return $user;
    }
}
```

为了自动连接 FacebookService 构造函数参数，将变量添加到。env 文件(根据您的环境)并将参数添加到 services.yaml 配置:

```
services:
    App\Service\FacebookService:
        arguments:
            $fbAppId: '%env(FB_APP_ID)%'
            $fbAppSecret: '%env(FB_APP_SECRET)%'
            $fbGraphVersion: '%env(FB_GRAPH_VERSION)%'
```

这是一个 FacebookUser 用户实体。可以用数组代替实体，但是 OOP 对我来说更方便。

```
<?php

namespace App\Entity;

class FacebookUser
{
    private $name;
    private $email;

    public function getName(): ?string
    {
        return $this->name;
    }

    public function setName($name): self
    {
        $this->name = $name;
        return $this;
    }

    public function getEmail(): ?string
    {
        return $this->email;
    }

    public function setEmail(string $email): self
    {
        $this->email = $email;
        return $this;
    }
}
```

就是这样。

实际上，在 Guard 中还有更多的方法，但创建它们应该并不复杂。

*原载于*[*https://bogomolov . tech*](https://bogomolov.tech/Facebook-Log-Id-Symfony/)*。*