# Golang 中的文件驱动 API 测试，非常规方式

> 原文：<https://blog.devgenius.io/file-driven-api-testing-in-golang-the-unconventional-way-18577a586cdb?source=collection_archive---------11----------------------->

> 我如何在 Golang 中创建了一个函数来测试我所有的 API 端点。

我个人从来不喜欢 Golang 的测试惯例。不得不为每个测试用例定义新的功能很快就会变得不和谐。因此，当我被告知为一项任务编写 API 测试时，我决定采用一种非常规的方式。

让我们从一些基础开始，Golang 的运行时提供了一个`testing`包，即使对初学者来说也相当容易使用。我不会在这篇博客中详细讨论`testing`包。

这个包将所有以`_test.go`结尾的文件识别为包含测试的文件，并在`go test`命令运行时运行。

## 表格驱动测试

表驱动测试减轻了为每个新测试用例编写新函数的负担。我们没有为每个测试用例编写新的函数，而是定义了一个新的`struct`来保存所有的测试用例。

不打算过多讨论表驱动测试的细节，但是 [***这里有一个***](https://dave.cheney.net/2019/05/07/prefer-table-driven-tests) 的博客介绍它。

表驱动测试是可以的，但是为了添加一个新的测试用例，测试用例设计者必须具备 Golang 的知识。

## 文件驱动测试

进入文件驱动测试，它非常类似于表格驱动测试。然而，我们没有在`_test.go`测试文件中定义测试用例，而是为测试用例创建了一个单独的文件。在我的例子中，我使用了`json`来定义测试用例。

既然我们已经完成了基础部分。先说我做了什么奇怪的事。

对于我创建的测试 API，我有 3 个模型:`user`、`product`和`review`

`GetUser`、`CreateProduct`和`CreateReview`需要用户验证。

我在我的测试用例中使用了下面的`json`结构。

```
{
    "name": "",         // test case name
    "endpoint": "",     // endpoint to make the request at
    "method": "",       // the method to use to make the request
    "handler": "",      // the handler to use to make the request
    "inputHeaders": {}, // the headers to send with the request
    "inputBody": {},    // the body to send with the request
    "expected": {
        "status": 0,    // the expected status code
        "response": {}  // the expected response
    }
}
```

这是它在 Golang 的样子。

现在，为了我的测试目的，我想创建一个单独的函数来测试和运行一切。听起来很简单，对吧？不完全是，至少在 Golang 不是。

这是因为 Golang 中的 API 测试不向服务器发出模拟请求(不像其他一些框架，如`javascript`中的`chai`)，相反，它只是用给定的请求和响应记录器调用处理程序。

好的，但是我们如何从我们计划的单一方法中调用不同的处理函数呢？简单，用地图。(我希望这种行为在未来的 Golang 版本中会变得更好，我们可以改为向服务器发出实际的请求)。

总之，这是我创建的地图。

现在，为了初始化测试用例，我们需要数据库中一些现有的`users`、`products` 和`reviews`。在创建了模型的上述实例后，我将它们的`***id***`和`user`中的`***jwtToken***` 存储起来以备将来之用。

好了，现在我们已经设置了前提，让我们进入实际的测试函数。

测试函数采用三个参数:`t testing.T`、`casesFile string`和`func`来生成`newRequest`，为什么是 ***newRequest*** ？你以后会见到我的，我保证。

在这里，我们设置了`test`数据库，并将测试用例文件解析成我们将用来运行测试的`testCases`变量。

现在，我们已经解析了，我们需要做的就是运行测试，对吗？似乎很容易，好吧，是的，有点。

为了正确地测试 API，我们需要发出请求，并将预期的响应与给定的响应体进行比较。我们使用作为参数传递的`newRequest() func`生成请求，记录响应并比较输出。

我们预期的身体反应可能是`Object`或`Array`，因此我们需要考虑这两种数据类型。

下面是我为这些用例编写的自定义`Compare() func`。

除了`id`和`token`对象之外，`MatchMaps() func`比较给定的贴图是否相等，因为它们在每次运行中的值可能不同。

现在，关于那个`newRequest func()` …

对于我们当前的用例，我们需要用给定的`method`、`endpoint`和`inputBody`用`newRequest() func`来创建请求，并设置指定的`headers`。(除了那个`Authorization`头，我们将使用从我们的`setUp() func`得到的`jwtToken`作为令牌)

好吧，但是如果`request`代只有这么多，为什么要分解成另一个函数呢？实际上不只是这么多，它可以根据个人的使用情况而变化。

我们的`CreateReview`端点看起来像`POST /api/review/{productId}`。这是一个问题，因为仅仅生成请求本身并不会将`productId`设置为请求上下文中的一个变量，但是它只特定于我们的`review`模型端点，因此我将它分解为模块化的部分，并且`review`模型测试人员可以在上下文中提供一个带有`productId`的新请求。

这是它的样子…

我使用`chi`进行路由，因此我的`SetUrlParamInContext() func`看起来像这样。根据您使用的路由器，您的设置可能会有所不同。

好了，看起来我们差不多完成了。只剩下一些东西了。

一旦我们的测试运行，我们需要删除数据库`test`。我们把这个放在`t.Cleanup` ***下回调*** 功能

等等，最后一件事。如果我们也想用这些来测试中间件呢？那很简单

我们只需修改`handlersMap`来添加 ***中间件*** 即可。

这将把`AuthMiddleware`添加到给定的端点。

好了，现在我们终于完成了。这里有一个测试用例文件。

好了，就这样。我们最终可以通过只定义一个函数来运行所有的测试。

你可以在[https://github.com/ShauryaAg/ProductAPI](https://github.com/ShauryaAg/ProductAPI)看看最终的实现

我很确定有更好的方法来做这些事情，但这就是我所做的，最重要的是，它有效。我真的希望 Golang 的后代能改变这一点，让它变得更容易。