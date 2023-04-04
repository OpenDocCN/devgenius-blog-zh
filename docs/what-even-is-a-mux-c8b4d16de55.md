# Mux 到底是什么？

> 原文：<https://blog.devgenius.io/what-even-is-a-mux-c8b4d16de55?source=collection_archive---------18----------------------->

"工程师不擅长给事物命名。"

这适用于很多事情。奇怪的术语，时髦的词语，完全相同的东西有十个不同的名字…名单还在继续。承认这一点有点尴尬，但我认为学习围棋最讨厌的部分之一是我必须习惯的行话和术语。当你一开始什么都不知道，决定一头扎进一个新的话题时，你必须经历这些。

什么是 mux？什么是处理程序？什么是中间件？手柄类型和手柄功能有什么区别？HandleFunc 和 HandlerFunc？苹果是橘子吗？生存还是毁灭？我喜欢游泳，有七个小矮人，怎么计算到月亮的距离？我到底在说什么？

没关系。我知道迷失在文档丛林中的感觉。这将是我试图帮助初学者 Go web 开发人员的系列文章的第 1 部分。在这篇文章中，我将解释什么是 mux，它是如何工作的，以及如何编写自己的 mux。

# 什么是 mux？

多路复用器。这不是一个很常见的词，这是肯定的。Mux 是*复用器*的简称。什么是多路复用器？

> *复用器。一种能够在一个通信信道上同时传输多个信息或信号的设备。—Dictionary.com*

在 Go 中，当我们说 mux 时，通常指的是 HTTP 请求多路复用器。它的主要工作是将传入的请求 URL 与一组预定义的路由进行匹配，并在匹配时做一些事情。简而言之，mux 充当进入应用程序的网关。

# 创建自定义多路复用器

这在您的代码中会是什么样子？让我们一步一步来。

```
package mainimport (
    "log"
    "net/http"
)func homeHandler(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("This is the home page."))
}func aboutHandler(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("This is the about page."))
}func main() {
    mux := http.NewServeMux() mux.HandleFunc("/", homeHandler)
    mux.HandleFunc("/about", aboutHandler) log.Fatal(http.ListenAndServe(":8080", mux))
}
```

这是一个非常简单的后端应用程序的例子。它通过运行相应的`homeHandler`和`aboutHandler`处理函数来处理对`/`和`/about`端点的请求。先看主函数。

```
mux := http.NewServeMux()
```

这将创建一个新的`ServeMux`实例。`ServeMux`是 mux 在`net/http`代码中的定义方式。

```
mux.HandleFunc("/", homeHandler)
mux.HandleFunc("/about", aboutHandler)
```

一个`ServeMux`有一个方法叫做`HandleFunc`。这需要两个参数:目标端点和一个处理程序。`HandleFunc`将向目标端点注册这个处理程序。每当请求到达端点时，我们的`ServeMux`将检查该端点是否有注册的处理程序。

```
func homeHandler(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("This is the home page."))
}func aboutHandler(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("This is the about page."))
}
```

这些是负责人。当到达一个端点时，处理程序实际上做了一些事情。他们有两个可以使用的参数:响应和请求。它可以从请求中读取任何数据，比如请求头和请求体。它还可以向响应中写入任何数据，无论是字节文本、简单的 HTML 还是 JSON 数据。

```
log.Fatal(http.ListenAndServe(":8080"), mux)
```

这一行是启动 web 服务器的代码。`ListenAndServe`将在指定的端口监听，在本例中是 8080。您还可以选择使用哪个 mux，传入一个`nil`值将使它使用`DefaultServeMux`。请注意，不推荐使用默认的 mux，因为它是作为全局变量存储的，其他包可以修改它。

# 多路复用器是如何工作的？

因此，我们对多路复用器的工作原理有了较高的理解。让我们更深入一点，因为了解一些东西在引擎盖下是如何工作的是一件有趣的事情。

```
type ServeMux struct {
    mu    sync.RWMutex
    m     map[string]muxEntry
    es    []muxEntry
    hosts bool
}type muxEntry struct {
    h       Handler
    pattern string
}
```

我们现在可以忽略`sync.RWMutex`，因为这是一种防止并发请求处理期间出现任何问题的措施。这超出了本教程的范围。

正如我上面提到的，mux 在 Go 中被定义为一种`ServeMux`类型，这是一种保存两种不同数据的结构。

*   `ServeMux.m`保存与`muxEntry`配对的 URL 模式的映射，T6 是保存处理程序和 URL 模式的结构。
*   `ServeMux.es`是用于模式匹配的`muxEntry`对象的有序列表。
*   `ServeMux.hosts`用于匹配基于主机的 URL，在 URL 中包含完整的主机名。

# 结论

希望这篇文章对想要从事后端 web 开发的 Go web 初学者来说是一个有用的起点。同样，这只是后端 web 开发系列的第 1 部分，所以请关注下一组帖子！我们还没有涵盖 Go 为我们提供的所有内容。在下一篇文章中，我们将更详细地看看处理程序。

感谢您的阅读！你也可以在 [Dev.to](https://dev.to/jpoly1219/what-even-is-a-mux-4fng) 和 [my personal site](https://jpoly1219.github.io) 上查看这个帖子。