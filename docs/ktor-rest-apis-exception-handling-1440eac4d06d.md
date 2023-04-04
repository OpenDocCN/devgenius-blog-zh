# Ktor REST Apis 异常处理

> 原文：<https://blog.devgenius.io/ktor-rest-apis-exception-handling-1440eac4d06d?source=collection_archive---------5----------------------->

## 后端教程

## 如果可以的话，抓住它！

![](img/3e885b0255f43a79004c8dbdb1f9714a.png)

来源:[测试自动化博客](https://blog.testproject.io/2021/04/21/exception-handling-and-custom-exception-in-test-automation/)

在这个使用 Ktor 的 Rest Api 系列中，我们之前探索了如何创建 Rest APi。但是对于任何代码库来说，异常都是一个难以言表的现实。非常重要的是，我们的代码已经为任何这种意外的场景做好了准备。因此，在本文中，我们将探索如何在 rest apis 中处理异常。

如果你想从头开始，请阅读以下内容，你不会失望。

> [Ktor REST API-第 1 部分(项目设置)](https://proandroiddev.com/build-rest-apis-using-ktor-framework-i-dbbf36b332bb)
> [Ktor REST API-第 2 部分(创建路线)](https://proandroiddev.com/build-rest-apis-using-ktor-framework-ii-47948e89f1d6)
> Ktor REST API[-](https://proandroiddev.com/build-rest-apis-using-ktor-framework-ii-47948e89f1d6)[第 3 部分(测试路线)](https://proandroiddev.com/build-rest-apis-using-ktor-framework-iii-87e579a7258e)
> Ktor REST API-使用 Ktorm 集成 SQL 数据库

# 状态空间

在 Ktor 中，异常处理是使用 StatusPages 完成的。这是一个插件，它可以根据抛出的异常或状态代码优雅地处理任何故障状态。这意味着我们可以为

*   任何未捕获的异常或可抛出的异常，即在执行过程中从任何地方引发的异常。
*   任何数量的特定状态代码

## 建立

为了使用这个插件，我们需要添加如下依赖项。

```
// status pages for exception handling
implementation("io.ktor:ktor-server-status-pages:$ktor_version")
```

然后我们需要像安装其他插件一样安装这个插件，如下所示

```
fun Application.configureExceptions() {
    *install*(*StatusPages*)
}
```

然后我们可以从应用程序的主模块调用这个配置，我们就可以使用插件了。

## 状态使用基于异常的处理

一旦我们安装了插件，就在那里，我们可以提供基于引发的异常返回内容的实现。

现在，为了定义异常处理，我们简单地创建一个 ***异常*** 块，并传递一个 ***<可抛出>*** 类型。它现在表明，任何可抛出的，如果不处理，应该来到这里，并根据这个块内的定义返回响应。

我们创建了两个自定义异常，我们假设它们是在运行时从不同的 API 抛出的。这些是简单的可抛出类，如下所示。

```
class ValidationException(override val message: String): Throwable()
class ParsingException(override val message: String): Throwable()
```

我们覆盖了消息字段，这样我们就可以传递自定义消息，现在完整的处理如下所示。

基于引发的异常的内容

我们只是检查我们收到了哪种未处理的异常，并相应地返回一个响应。这里***exception response***是用于提供 json 响应格式的数据类。

```
@Serializable
data class ExceptionResponse(
    val message: String,
    val code: Int
)
```

我们将在本文的 ***动作*** 部分中看到 API 的运行。

## 使用基于状态代码的处理的状态页面

类似于 ***异常*** 块下基于异常的处理，我们可以如下定义 ***状态*** 块下基于状态码的处理。

基于状态代码的内容

在这种情况下，如果 api 将返回此 ***状态*** 块下定义的任何 http 状态，则控制进入 StatusPages。例如，我们提到了内部服务器和坏网关错误。因此，每当我们的 API 返回这些代码时，执行控制就会来到这里，我们就能够用定义好的响应进行响应。

好吧！设置够了！现在是时候来看看以上所有内容的实际应用了。

# 行动

我们将创建 4 个 api 端点，它们都将抛出 4 种不同类型的异常。其中两个将是未捕获的异常，其余两个将基于状态代码。

```
exception/validation      // exception based
exception/parsing         // exception based
exception/internal-error  // status based
exception/bad-gateway     // status based
```

对于这些 API，我们将如下设置一个路由。

```
fun Application.configureExceptionRoutes() {
    routing **{** route("/exception") **{** *exceptionRoutes*()
            *statusRoutes*()
        **}
    }** }
```

异常路由将包含基于异常的路由，即验证和解析。

状态路由将包含基于状态的路由，即内部错误和坏网关。

## API:例外/验证

我们简单地定义一个端点并抛出一个验证异常。

```
get("/validation") **{** throw ValidationException("this is a validation exception")
**}**
```

到达这个端点时，我们得到如下结果。

```
{
    "message": "this is a validation exception",
    "code": 400
}
```

喔！说到点子上。让我们也测试一下其他人。

## API:异常/解析

我们简单地定义一个端点并抛出一个解析异常。

```
get("/parsing") **{** throw ParsingException("this is a parsing exception")
**}**
```

到达这个端点时，我们得到如下结果。

```
{
    "message": "this is a parsing exception",
    "code": 417
}
```

## API:异常/内部错误

我们简单地定义一个端点并返回内部服务器错误状态。

```
get("/internal-error") **{** *call*.respond(
        HttpStatusCode.InternalServerError
    )
**}**
```

到达这个端点时，我们得到如下结果。

```
{
    "message": "Oops! internal server error at our end",
    "code": 500
}
```

## API:异常/错误-网关

我们简单地定义一个端点并返回内部服务器错误状态。

```
get("/bad-gateway") **{** *call*.respond(
        HttpStatusCode.BadGateway
    )
**}**
```

到达这个端点时，我们得到如下结果。

```
{
    "message": "Oops! We got a bad gateway. Fixing it. Hold on!",
    "code": 502
}
```

# 优势

使用 StatusPages 为我们提供了一些优势:

*   我们可以为未捕获的异常提供一致的实现。从代码维护的角度来看，这也是有益的。
*   我们可以为特定的异常提供特定的实现，同时仍然与其余的异常保持一致。
*   我们可以处理不同的状态代码，为用户提供一致的响应。

这足以让我们开始处理 API 中的异常。尝试不同场景并在现实中体验它们。作为参考，您可以在下面查看完整的代码。希望会有用。

[](https://github.com/aqua30/Apis-using-Ktor/tree/exception-handling) [## GitHub-aqua 30/Apis-在异常处理时使用-Ktor

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/aqua30/Apis-using-Ktor/tree/exception-handling) 

# 目前就这些了！敬请期待！

与我联系(如果内容对您有帮助),请访问

*   [中等](https://saurabhpant.medium.com/)
*   [GitHub](https://github.com/aqua30)
*   [推特](https://twitter.com/saurabh30pant)
*   [领英](https://www.linkedin.com/in/saurabh-pant-44619057/)

订阅电子邮件，同步了解更多关于 Android/IOS/Backend/Web 的有趣话题。

直到下一次…

干杯！