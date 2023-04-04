# go:http outer 简介

> 原文：<https://blog.devgenius.io/go-httprouter-be9c27f569ba?source=collection_archive---------8----------------------->

Go 中最流行的 Http 路由器库之一

![](img/f6597fd32438d19c9f411c4ba2a3047e.png)

哈维尔·阿莱格·巴罗斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 什么是 HttpRouter？

HTTP Router 是 Golang 中最流行的开源 HTTP 路由器库之一。http outer 很快，也很简单，因为 http outer 只有路由功能。

如果你想看 HttpRouter 的文档，你可以访问下面的链接:

```
[https://github.com/julienschmidt/httprouter](https://github.com/julienschmidt/httprouter)
```

# HttpRouter 入门

在 go 中使用 http outer 之前，您需要将 http outer 包添加到您的项目中。您可以使用此命令将 HttpRouter 包添加到您的项目中。

```
**go get  github.com/julienschmidt/httprouter**
```

# 路由器

HttpRouter 库的核心是路由器的**结构。路由器是 http 的一个实现。Handler，所以将 Handler 添加到 **http 会更容易。服务器**。要创建一个新的路由器，你可以使用`**httprouter.New()**`，它将返回一个路由器**的**指针。**

# HTTP 方法

路由器类似于 ServeMux。您可以向路由器添加路由。与 ServeMux 相比，路由器具有优势。在 router 中，您可以轻松选择想要使用的 HTTP 方法。要将路由添加到路由器中，您可以使用相同的功能及其自己的 HTTP 方法，如`GET()`、`POST()`等。

请参见在 HttpRouter 中添加路由的示例

```
router := httprouter.New()
router.GET("/", GetHandlerFunctionName)
router.POST("/post", PostHandlerFunctionName)
```

# 处理

要在 HttpRouter 中添加路由，您可以使用`httprouter.Handle`。与只有两个参数的 ServeMux 相比，HttpRouter 有三个参数。有 ResponseWriter、Request 和 Params。不同的是参数。

请看使用 HttpRouter 添加处理函数的例子

```
func HandlerName(w http.ResponseWriter, r *http.Request, params httprouter.Params) {}
```

# 参数

之前我提到过 HttpRouter 有三个参数，其中一个是 Params。Param 是存储从客户端接收的参数的位置。有时我们需要使用一个动态的 URL 如`products/1`来访问 id 号为 1 的指定产品，其中 **1** 是产品的 id。URL 中的动态参数，将自动在 params 中收集。

请参见如何获取产品 id 的示例

```
router.GET("/products/:id", func(w http.ResponseWriter, r *http.Request, params httprouter.Params) {
    id := params.ByName("id")
}
```

在上面的代码中，您需要添加“:”来让 route 知道您的参数在哪里。要获得 params 的值，您可以使用`params.ByName(**ParamsName**)`，在这种情况下，ParamsName 是 **id。**如果您正在访问 URL `products/1`，id 变量的值将为“1”。

您也可以从 URL 获取多个参数，要获取多个参数，您可以像这样编辑路径`/products/:id/name/:name`。用前面的代码你可以得到参数 id 的值&名。

Params 也可以获取路径，要捕捉路径你可以在你的 route `iamges/*imagePath`中使用这个代码。如果您正在访问 URL `images/productId/photo.png`，您可以使用 **imagePath** params 访问文件路径，并将返回`/productId/photo.png`。

# 提供文件

http outer 还支持 ServeFiles，你可以使用`router.ServeFiles(path, fileSystem)`在 http outer 中提供文件。 **Path** 是一条路径，用于定位您的服务器文件可以访问的位置。**文件系统**是您想要服务的文件夹的目录。

请参见使用 Serve 文件的示例

```
router.ServeFiles("/files/*filepath", http.Dir("resource/"))
```

上面的代码是 serve 文件，在**资源文件夹**中，在你的项目中路由`/files/`。因此，当您需要从资源文件夹中访问 hello.txt 文件时，您可以使用`/files/hello.txt`进行访问。

# 紧急处理程序

HtppRouter 具有处理紧急情况的功能。通常，当你的逻辑出现混乱时，你的程序会出错并停止返回任何响应。有时当错误发生时，你需要做些什么。但是使用 HttpRouter，您可以使用下面的代码来处理它。

```
router.PanicHandler = func(w http.ResponseWriter, r *http.Request, error interface{}) {
**// you can add logic here when panic happens
// you can get an error message with "error"**
}
```

# 未找到处理程序

HttpRouter 也没有找到处理程序。未找到处理程序可以捕捉到您正在访问找不到的路由的情况。要使用 Not Found 处理程序，您可以使用下面的代码。

```
router.NotFound = http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
**// you can add logic here when Not Found happens**
})
```

# 方法不允许处理程序

使用 ServeMux，您不能选择将在处理程序中使用的 HTTP 方法。但是在 HttpRouter 中是可以的。您可以在创建处理程序时设置 HTTP 方法。因此，如果客户端使用我们之前已经设置的不同方法访问路由，不允许的处理程序将捕获它们。您可以使用下面的代码使用不允许的处理程序。

```
router.MethodNotAllowed = http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
**// you can add logic here when Method Not Allowed happens**
})
```

> 参考:
> [Golang Http 路由器 by PZN](https://www.youtube.com/watch?v=spXgBcjiuWM)
> [https://github.com/julienschmidt/httprouter](https://github.com/julienschmidt/httprouter)