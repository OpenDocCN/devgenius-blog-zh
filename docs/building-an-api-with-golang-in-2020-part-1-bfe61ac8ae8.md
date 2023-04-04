# 2020 年用 Golang 构建 API 第 1 部分

> 原文：<https://blog.devgenius.io/building-an-api-with-golang-in-2020-part-1-bfe61ac8ae8?source=collection_archive---------5----------------------->

![](img/36ef0b74f8057e324e497ca9b5147ca5.png)

照片由[埃米尔·佩龙](https://unsplash.com/@emilep?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在今年开始在 Mercado Livre 的新工作之前，我已经爱上了 Golang，在我以前的工作和一些辅助项目中制作了一个供内部使用的小 API，但没有真正准备好生产。

在过去的三个月里，我沉浸其中，从所有同事和队友那里学到了很多东西，我想分享一下我的经验和通过使用我学到的良好实践来构建项目所学到的东西会很好。

这篇文章和我打算写的其他一些文章的想法是在我读了 Diego Henrique Oliveira 的这篇伟大的文章后产生的，我觉得这篇文章很棒，它激励我使用他的项目组织的想法来做一些事情。

主要想法是建立一个**投票 API** ，在那里我们将有 CRUD 操作和 API 操作和验证，并应用一些概念，如*干净的架构、包管理、域隔离、依赖注入*等等。我们还将使用一点 Docker，并使用配置项部署我们的 API。也就是说，让我们开始吧！

# 开始一个项目

我们将从初始化支持 **Go 模块**的项目开始，这是自 Go 1.13 以来默认的包管理器。要做到这一点，创建您的源文件夹，在我的例子中是`go-voting-api`无论您喜欢在哪里保存您的项目，在该文件夹中打开一个终端并运行:

```
$ go mod init github.com/$yourGithubName/go-voting-api
$ git init
```

该命令将在您的根文件夹中创建两个文件，`go.mod`,这两个文件将跟踪您的依赖项及其版本。更多详情，[这篇博文](https://blog.golang.org/using-go-modules)很棒。

Go 已经提供了一个很棒的 HTTP 包，但是在这个例子中我们将使用 [Chi](https://github.com/go-chi/chi) ，它是一个不可思议的 HTTP 路由器，具有中间件支持。此时，我们将使用如下所示的`go get`手动安装 chi 包。我们不需要使用`go get`安装每一个包，因为`go mod`已经识别出在构建时没有满足的导入，并安装它们。关于`go mod`的另一个重要信息是，由于它是自动导入的，有时它会跟踪一些我们不再使用的包，你可以运行`go mod tidy`来删除未使用的包。

```
go get github.com/go-chi/chi
```

**建设第一条路线**

安装好我们的主要依赖项后，我们将通过创建一个简单的`/health`端点来启动我们的应用程序，只是为了查看我们的服务器运行良好。为此，创建一个`main.go`文件，它将创建一个`router`并启动一个服务器，处理我们的健康路径。

```
**package** main

**import** (
	"fmt"
	"net/http"

	"github.com/go-chi/chi"
)

**func** main() {
	router := chi.NewRouter()
	router.Get("/health", **func**(w http.ResponseWriter, r *http.Request) {
		fmt.Fprint(w, "healthy")
	})

	fmt.Println("Starting server...")
	err := http.ListenAndServe(":8080", router)
	if err != nil {
		fmt.Printf("Error starting server: %s", err)
	}
}
```

让我们分成几个部分，让它更容易理解。在第一行，我们键入`package main`告诉 go 我们正在构建一个 app，这是 app 的主包。之后，我们从 Go 标准库导入一些包，`fmt`来处理`logs`和字符串操作，比如将响应字符串写入响应流，`net/http`来处理我们的 HTTP 请求并启动服务器。最后一个导入是通过标准 HTTP 包导入 Chi，这将是我们的路由器。

之后我们创建一个`main`函数告诉 go，这是初始化我们的应用程序的函数，`main`包的`main`函数。在这个函数中，我们启动我们的 Chi 路由器，设置一个到`GET /health`的路由，并设置一个默认的 [HTTP 处理函数](https://golang.org/pkg/net/http/#HandlerFunc)，来处理请求。

之后，我们使用`http`包中的方法`ListenAndServe`启动一个服务器，在第一个参数中传递端口，在第二个参数中传递我们的路由器。

在`ListenAndServe`之后的`if err != nil {`中，我们可以看到 Golang 是如何处理错误的。每个可能引发错误的函数都会返回一个`error`作为结果之一，然后我们需要检查这个错误是否为空来处理这个错误。

现在你只需要运行你的终端`go run main.go`，你会看到我们的日志行，告诉你服务器正在启动，如果你访问`http://localhost:8080`，你会看到我们的`Healthy`响应。

# 结论

这篇文章的结尾比我预期的要大一点，但是我希望它能得到很好的解释，并能帮助你，欢迎任何反馈。您可以在[文章库](https://github.com/felipecaputo/go-voting-api)中看到我们构建的源代码，在下一篇文章中，我们将把数据库添加到我们的 API 中。

享受编码！