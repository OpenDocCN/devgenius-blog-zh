# 如何在 PHP 中用 cURL 获得响应头

> 原文：<https://blog.devgenius.io/how-to-get-the-response-headers-with-curl-in-php-2173b10d4fc5?source=collection_archive---------1----------------------->

## 对此没有内置的解决方案，但是我们可以轻松地创建自己的解决方案

![](img/62a5477efff0eb416f0bd0f4802ddc20.png)

照片由[本](https://unsplash.com/@benofthenorth?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

许多公共 API 使用自定义头来向用户传递一些额外的数据，如速率限制或访问限制。当在 JavaScript 中使用`[XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)`时，你可以很容易地通过`[getAllResponseHeaders()](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/getAllResponseHeaders)`或`[getResponseHeader(headerName)](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/getResponseHeader)`函数得到它们。几天前，我在 PHP 中使用了一个带有自定义头文件的新 API，我想应该会有一个简单的内置函数。

然而，快速搜索后，我意识到没有内置的解决方案。幸运的是，只要多做一点工作，还是有可能得到响应头的。

# cURL 中的响应标头

如果您只对响应头感兴趣，而对正文内容不感兴趣，cURL 内置了一个简单的解决方案。通过将`[CURLOPT_HEADER](https://www.php.net/manual/en/function.curl-setopt.php)`和`[CURLOPT_NOBODY](https://www.php.net/manual/en/function.curl-setopt.php)`选项设置为真，`[curl_exec()](https://www.php.net/manual/en/function.curl-exec.php)`的结果将只包含标题。当您只需要消息头时，这很有用，但是大多数时候，我们也需要请求的内容。

这个问题的解决方案也存在于`[CURLOPT_HEADER](https://www.php.net/manual/en/function.curl-setopt.php)`选项中。将其设置为 true 将在`[curl_exec()](https://www.php.net/manual/en/function.curl-exec.php)`的输出中包含标题。然而，如果没有`[CURLOPT_NOBODY](https://www.php.net/manual/en/function.curl-setopt.php)`选项，我们现在有一个包含请求头和请求体的输出，我们需要将它们分开。这可以用`[curl_getinfo()](https://www.php.net/manual/en/function.curl-getinfo.php)`函数中的`[CURLINFO_HEADER_SIZE](https://www.php.net/manual/en/function.curl-getinfo.php)`参数来完成，它将告诉我们标题的长度，我们可以很容易地使用这个值来为标题和正文创建两个子字符串。

在这一点上，我们有一个很长的字符串，所有的标题都不是特别用户友好。因此，让我们实现一个函数来创建一个漂亮的关联数组，将标题名作为键，将它们的值作为值:

# 一个完整的例子

让我们首先创建一个快速示例 API，并设置一些自定义头:

现在，让我们对我们的测试 API 做一个 cURL 请求，并使用之前的函数同时获取自定义标题和正文内容:

如果我们查看脚本的输出，我们可以看到我们从自定义头中获取了值，并且仍然可以访问正文中的值:

```
CUSTOM_HEADER: Hello World
API_REQUEST_COUNTER: 42
API_REQUEST_LIMIT: 69
Body: {"result":true,"someValue":420}
```

虽然 PHP 中没有内置的解决方案来检索 cURL 中的响应头值，但很容易为它创建我们自己的自定义函数。通过设置一个特殊的选项，头部被包含在输出中，我们可以将它们从请求的内容中分离出来。然后，我们可以将它们转换成用户友好的关联数组，并轻松访问它们的值。如果您在 PHP 中使用 cURL 来使用任何带有自定义头文件的 API，这是一个非常有用的工具。