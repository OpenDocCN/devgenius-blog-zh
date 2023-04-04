# 用户注册服务

> 原文：<https://blog.devgenius.io/user-registration-service-5c1739321eb7?source=collection_archive---------1----------------------->

## 用户注册一个应用会怎么样？

![](img/eaaa6d530f785b9741e5e48edc0a0db9.png)

照片由[迈卡·威廉姆斯](https://unsplash.com/@mr_williams_photography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这些天我们有如此多的应用程序需要我们注册才能访问它们的应用程序。如果注册成功，我们就可以登录应用程序。让我们了解一下当我们注册任何应用程序时，后台发生了什么。

## 用户注册

注册请求必须提供一个用户对象作为键/值属性的集合。集合必须包含标记为 identity 的属性以及`password`属性。此外，如果您的应用程序被配置为确认注册用户的电子邮件地址，那么`email`属性是必需的。

*方法:*发布

*端点 URL :*

```
http://localhost:8080/api/users/register
```

*请求头:*内容类型:应用程序/json

其中`Content-Type`必须设置为`application/json`。此标题是强制性的。

*请求正文:*

```
{  
  "first_name" : value,  
  "last_name" : value,  
  "email" : value,
  "password" : value 
}
```

*响应正文:*

```
{
  "objectId" : value,
  "first_name" : value,  
  "last_name" : value,  
  "email" : value,
  "password" : value 
}
```

*错误代码:*

当服务器端报告错误时，它以下列格式返回一个 JSON 对象:

```
{  
  "message" : error-message,  
  "code" : error-code  
}
```

## 创建电子邮件确认 URL

确认电子邮件包含一个特殊链接，用户单击该链接可以确认他们的电子邮件地址。当点击链接且链接未过期时，用户状态从`EMAIL_CONFIRMATION_PENDING`变为`ENABLED`。发送给用户的电子邮件可以使用 Amazon Simple Email Service 创建。有些场景需要在应用程序逻辑中生成链接，例如，如果您需要通过文本消息发送出去，或者如果您需要动态生成确认电子邮件。下面的 API 用于获取用户的确认 URL。

*方法:*贴

*端点 URL :*

```
http://localhost:8080/api/users/resendconfirmation/<identity>
```

其中，Users 表中标记为 identity 的列中的`<identity>`值。默认情况下，应用程序将`email`作为标识列。在这种情况下，API 的参数应该是用户的电子邮件地址。

*请求头:*内容类型:应用程序/json

其中`Content-Type`必须设置为`application/json`。此标题是强制性的。

*请求正文:*无

*响应体:*一个 JSON 文档，结构如下。

```
{  
  "confirmationURL": "https://xxxx.backendless.app/api/............"  
}
```

*错误代码:*

当服务器端报告错误时，它以下列格式返回一个 JSON 对象:

```
{  
  "message" : error-message,  
  "code" : error-code  
}
```

## 电子邮件验证

该 API 向用户重新发送电子邮件确认消息。它仅适用于启用了“要求电子邮件确认”选项的应用程序。如果用户状态是“电子邮件确认待定”，则重新发送带有确认链接的电子邮件，否则会出现错误。

*方法:*发布

*端点 URL :*

```
http://localhost:8080/api/users/resendconfirmation/<identity>
```

其中，Users 表中标记为 identity 的列中的`<identity>`值。默认情况下，应用程序将`email`作为标识列。在这种情况下，API 的参数应该是用户的电子邮件地址。

*请求头:*内容类型:应用程序/json

其中`Content-Type`必须设置为`application/json`。此标题是强制性的。

*请求正文:*无

*响应正文:*无。如果 API 请求成功完成，这意味着已经向用户发送了电子邮件确认消息。否则，将返回一个错误。

*错误代码:*

当服务器端报告错误时，它以下列格式返回一个 JSON 对象:

```
{  
  "message" : error-message,  
  "code" : error-code  
}
```

## 用户登录

登录操作需要两个属性:一个标记为用户身份，另一个是密码。它自动将`"AuthenticatedUser"`角色分配给所有成功登录的用户。该角色可用于区分经过身份验证的用户和来宾对各种资源(数据库中的数据、文件、消息传递通道)的访问。

*方法:*贴

*端点 URL :*

```
http://localhost:8080/api/users/login
```

*请求头:*内容类型:应用程序/json

其中`Content-Type`必须设置为`application/json`。此标题是强制性的。

*请求正文:*

```
{  
  "login" : value,
  "password" : value 
}
```

`"login"`键必须包含标记为 identity 的属性值。身份用于登录和恢复密码操作。当用户注册时，服务器将确保身份属性的值是唯一的。

*响应正文:*

```
{  
  "objectId" : value,  
  "user-token": value,   
  "prop-name1":value,  
  "prop-name2":value,  
  "prop-name3":value,  
  ...  
}
```

`objectId`属性是服务器分配给用户帐户的唯一标识符。`user-token`值标识由登录操作发起的用户会话。这两个值`objectId`和`user-token`都是更新数据库中的用户所必需的。

*错误代码:*

当服务器端报告错误时，它以下列格式返回一个 JSON 对象:

```
{  
  "message" : error-message,  
  "code" : error-code  
}
```

## 维护用户会话

登录 API 中返回的`user-token`值必须在后续请求中使用，以便维护用户会话。该值唯一标识服务器上的用户和会话，并用于实施安全策略、应用用户和角色权限以及跟踪使用率分析。对于登录后发出的所有请求，必须在 HTTP 头中发送`user-token`值:

```
"user-token" : value
```

## 验证用户登录

`user-token`值可以保存在客户端应用程序中，以便在应用程序重启时使用。这有助于简化用户体验，因为应用程序的用户不需要再次登录。但是，当应用程序重新启动时，它需要检查底层用户令牌以及用户会话是否仍然有效。这可以通过下面的 API 来实现:

*方法:*获取

*端点 URL :*

```
http://localhost:8080/api/users/isvalidusertoken/<userToken>
```

其中`<userToken>`用于验证。用户令牌的值作为登录 API 请求的结果返回。

*返回值:*

如果令牌有效，服务器返回布尔值`true`，否则返回`false`。

## 注销

注销操作终止用户会话，并解除`AuthenticatedUser`角色与客户端应用程序发出的后续请求的关联。

*方法:*获取

*端点 URL :*

```
http://localhost:8080/api/users/logout
```

*请求标题:*

```
user-token: value-of-the-user-token-header-from-login
```

其中`user-token`是前面登录操作的响应中返回的值。该值标识要注销的用户。此标题是强制性的。

*错误代码:*

当服务器端报告错误时，它以下列格式返回一个 JSON 对象:

```
{  
  "message" : error-message,  
  "code" : error-code  
}
```

在本文中，我们讨论了用户注册的细节。许多应用程序也在使用 OAuth。OAuth 用于授权网站或应用程序访问它们在其他网站上的信息，但不提供密码。我们将在以后的文章中讨论 OAuth 工作流。敬请关注。快乐学习！🎃