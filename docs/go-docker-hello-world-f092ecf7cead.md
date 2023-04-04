# Go & Docker Hello World！

> 原文：<https://blog.devgenius.io/go-docker-hello-world-f092ecf7cead?source=collection_archive---------0----------------------->

![](img/26b740b6272b7648f773d4435089e3a5.png)

由 [Jerry Zhang](https://unsplash.com/@z734923105?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

一个简单的使用 Go 和 Docker 的 HTTP 服务器。

在本指南中，我将介绍如何从头开始创建您的第一个 Go 项目，并介绍如何使用 Docker 构建和运行一个 HTTP 服务器示例。

*注意:请确保您已经在系统上安装了* [*Go*](https://go.dev/) *和*[*Docker*](https://www.docker.com/)*后再继续。任何文本编辑器将工作，否则！(我用的是*[*vs code*](https://code.visualstudio.com/)*)。*

# 创建项目

让我们继续创建一个名为`hello_go_http`的新项目:

```
$ mkdir hello_go_http && cd hello_go_http
```

使用 go，我们可以创建一个`go.mod`文件(它将我们的项目表示为一个模块，并帮助我们定义任何未来的依赖关系)。这可以通过以下方式创建:

```
$ go mod init hellogohttp/m/v2
```

然后，让我们添加一个`main.go`文件，这将是我们的应用程序入口点，我们将在这里添加我们的服务器逻辑。

首先创建文件:

```
$ touch main.go
```

然后修改内容，如下所示:

```
package main;import (
    "fmt"
    "log"
    "net/http"
)func main() {
    http.HandleFunc("/helloworld", func(w http.ResponseWriter, r *http.Request){
        fmt.Fprintf(w, "Hello, World!")
    })
    fmt.Printf("Server running (port=8080), route: [http://localhost:8080/helloworld\n](http://localhost:8080/helloworld\n)")
    if err := http.ListenAndServe(":8080", nil); err != nil {
        log.Fatal(err)
    }
}
```

这将在端口`8080`启动一个 HTTP 服务器，并暴露单一路由`helloworld`。我们可以使用以下工具在本地构建和运行它:

```
$ go run main.go
```

并通过导航到[http://localhost:8080/hello world](http://localhost:8080/helloworld)，使用 curl 或浏览器测试它的工作情况。

```
$ curl [http://localhost:8080/helloworld](http://localhost:8080/helloworld)
Hello, World!
```

太好了！在 Go 中，您现在已经有了一个简单的本地运行的 HTTP 服务器。

# 用 Docker 构建

让我们通过在 Docker 中实现构建过程来更进一步，这将有助于[将应用部署到云](https://docs.aws.amazon.com/AmazonECS/latest/userguide/docker-basics.html)。

在项目的根目录下，创建一个名为`Dockerfile`的新文件:

```
$ touch Dockerfile
```

然后修改内容，使其看起来像:

```
# Create build stage based on buster image
FROM golang:1.16-buster AS builder# Create working directory under /app
WORKDIR /app# Copy over all go config (go.mod, go.sum etc.)
COPY go.* ./# Install any required modules
RUN go mod download# Copy over Go source code
COPY *.go ./# Run the Go build and output binary under hello_go_http
RUN go build -o /hello_go_http# Make sure to expose the port the HTTP server is using
EXPOSE 8080# Run the app binary when we run the container
ENTRYPOINT ["/hello_go_http"]
```

然后，您可以使用 Docker 构建并运行这个容器:

```
$ docker built -t hello_go_http .
$ docker run -p 8080:8080 -t hello_go_http
```

*注意:需要* `*-p*` *标志来让运行时知道要发布这个端口，它会将所有流量转发到 HTTP 服务器端口。*

并且可以使用 curl 测试一切是否正常。

```
$ curl [http://localhost:8080/helloworld](http://localhost:8080/helloworld)
Hello, World!
```

# 改进构建

虽然这是一个很大的进步，但我们可以更进一步，使用 Docker 的多阶段构建过程来改进容器的大小。

现在，我们已经将所有的 Go 源代码复制到了我们的容器中，并且将不必要的工具作为`buster`基础映像的一部分。实际上，我们只需要构建的二进制文件来运行应用程序。

为了去掉不必要的项目，让我们添加一个额外的构建阶段，这样`Dockerfile`现在看起来就像:

```
FROM golang:1.16-buster AS builderWORKDIR /appCOPY go.* ./
RUN go mod downloadCOPY *.go ./
RUN go build -o /hello_go_http# Create a new release build stage
FROM gcr.io/distroless/base-debian10# Set the working directory to the root directory path
WORKDIR /# Copy over the binary built from the previous stage
COPY --from=builder /hello_go_http /hello_go_httpEXPOSE 8080ENTRYPOINT ["/hello_go_http"]
```

现在我们有了一个额外的阶段，它使用一个不同的基础映像来运行二进制文件，从我们之前定义的`builder`阶段复制二进制文件。

同样，让我们使用 Docker 来构建和运行它:

```
$ docker build -t hello_go_http_multistage .
$ docker run -p 8080:8080 -t hello_go_http_multistage .
```

最后，检查我们的应用程序是否仍按预期工作:

```
$ curl [http://localhost:8080/helloworld](http://localhost:8080/helloworld)
Hello, World!
```

太好了！现在我们可以看到我们的应用程序成功运行了，有了我们的多级 Docker 构建。

现在你可能会问，我如何才能看到增加额外阶段的好处？好吧，让我们来看看我们现在构建的两个图像的大小(`hello_go_http` & `hello_go_http_multistage`):

```
$ docker images
REPOSITORY                        TAG           IMAGE ID       CREATED          SIZE
hello_go_http_multistage          latest        bd6daad47845   10 seconds ago   25.4MB
hello_go_http                     latest        a31792310b10   3 minutes ago    868MB
```

所以可以看到我们**把有效大小从 868MB 降到了 25.4MB** ！

*注意:这个灵感来自于* [*Docker 的官方文档，查看更多关于 Docker 和多级构建的信息*](https://docs.docker.com/language/golang/build-images/) *！*

现在，您已经完成了所有步骤，让我们来看看我们取得了什么成果:

*   创建了一个基本的 Go 应用程序，它通过 HTTP 提供内容。
*   配置 Docker 构建并向 Go HTTP 服务器公开流量。
*   使用多阶段方法改进了 Docker 构建图像的大小。

# 下一步是什么？

这是准备用 Go 开发应用程序的一个很好的开始，而且从本指南中可以找到许多很好的后续步骤和项目。首先，这里有一些建议:

*   [通过构建的容器映像将您的 HTTP 服务器部署到 AWS 上](https://docs.aws.amazon.com/AmazonECS/latest/userguide/docker-basics.html)。
*   扩展您的路由以构建一个后端服务来满足您的需求。
*   用 [Gorilla](https://github.com/gorilla/websocket) 添加 WebSocket 服务器支持。