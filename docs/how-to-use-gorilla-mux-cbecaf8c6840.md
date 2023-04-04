# 如何使用 gorilla/mux

> 原文：<https://blog.devgenius.io/how-to-use-gorilla-mux-cbecaf8c6840?source=collection_archive---------8----------------------->

欢迎回到我的指南，*Golang Web 开发简介*。上一次，我们看了如何为我们的 web 应用程序编写处理程序。既然我们知道了多路复用器和处理程序是如何工作的，是时候使用一个更复杂的工具了。虽然 Go 确实有一个令人惊叹的`net/http`包，但是`gorilla/mux`提供的某些功能让我们的生活变得更加轻松。事不宜迟，让我们进入第 3 部分。

# 我为什么要使用这个包？

如果标准库这么好，我们为什么还要费心去使用`gorilla/mux`？有几个原因。

*   它可以从 URL 路径中提取变量。
*   它支持 subrouters，您可以使用它对相似的路由进行分组。
*   您可以通过域、前缀、方法等来匹配路由。
*   学习曲线并不陡峭，这要感谢实现了`http.Handler`接口的包，使它与标准库兼容。

还有更多功能我没有在这里列出，但这些是最常用的功能。`gorilla/mux`是一个成熟的软件包，拥有丰富的文档和广泛的用户基础，所以当你遇到困难时，更容易获得在线帮助。

# 导入包

要使用`gorilla/mux`，需要导入。

```
package mainimport (
    "github.com/gorilla/mux"
)
```

将它导入到您的`main.go`文件后，我们需要安装它。

```
go mod init example.com/mywebappgo mod tidy
```

我们首先用第一个命令初始化我们的`go.mod`文件。URL 还不重要。我们不会将它托管在远程存储库中。如果我们正在开发一个供其他人使用的库，这个 URL 将是到您的远程 repo 的路径，比如`github.com/username/mypackage`。

一旦创建了一个`go.mod`文件，我们使用第二行来检查和更新依赖关系。`go mod tidy`通常用于修剪不必要的依赖项，但它对于添加缺失的依赖项也很有用。

现在已经安装好了，我们可以开始使用这个包了！

# 让我们来看一个例子

我们将构建一个图书馆(如图书)管理应用程序，用户可以在其中搜索本系列的书籍和作者。它类似于 Kindle 或 Barnes & Noble 商店，尽管要简单得多。

我们的网络应用将支持这些操作:

*   用户可以获得所有书籍的数据。
*   用户可以通过他们的 ISBNs 获得每本书的数据。
*   用户可以添加新书。
*   用户可以更新现有的图书数据。
*   用户可以删除图书。

当然，如果您的普通客户可以自由使用，其中一些操作将是毁灭性的。想象一下，我去 Kindle 电子书商店，把他们一半的图书馆都抹掉。但是，由于身份验证和授权超出了本指南的范围，我们将假设此 web 应用程序的用户是经理和管理员。

```
package mainimport (
    "fmt"
    "log"
    "net/http" "github.com/gorilla/mux"
)func main() {
    r := mux.NewRouter() r.HandleFunc("/", homeHandler) booksSubR := r.PathPrefix("/books").Subrouter() booksSubR.HandleFunc("/all", AllHandler).Methods(http.MethodGet)
    booksSubR.HandleFunc("/{isbn}", IspnHandler).Methods(http.MethodGet)
    booksSubR.HandleFunc("/new", NewHandler).Methods(http.MethodPost)
    booksSubR.HandleFunc("/update", UpdateHandler).Methods(http.MethodPut)
    booksSubR.HandleFunc("/delete/{isbn}", DeleteIspnHandler).Methods(http.MethodDelete) log.Fatal(http.ListenAndServe(":8090", r))
}
```

如果你已经读过我以前的指南，这段代码看起来会很熟悉。

```
// using gorilla/mux
r := mux.NewRouter()
r.HandleFunc("/", homeHandler)
```

我们首先创建我们的 mux 并注册一个端点。在 vanilla Go 中，我们将通过这样做来完成:

```
// using net/http
mux := http.NewServeMux()
mux.HandleFunc("/", homeHandler)
```

注意到代码是多么的相似吗？这都归功于`gorilla/mux`实现了`http.Handler`接口，使其与`net/http`标准库兼容。

这是有趣的部分。

```
booksSubR := r.PathPrefix("/books").Subrouter()
```

*匹配路线*和*副路线*的概念在此发挥作用。第一种方法是`PathPrefix()`，匹配以`/books`开头的路线，比如`/books/all`或者`/books/new`。现在，这本身是可用的，但如果我们将这些路线分组会更好。我们通过调用`Subrouter()`方法进行分组。上面的代码将创建一个子路由器，它匹配只以`/books`开头的路由。它不会处理任何其他路线，例如`/home`或`/welcome`。

```
booksSubR.HandleFunc("/all", booksAllHandler).Methods(http.MethodGet)
booksSubR.HandleFunc("/{isbn}", booksIspnHandler).Methods(http.MethodGet)
```

这大体上和普通的`HandleFunc`一样，但是有额外的特性。

后面的`Methods()`方法将路由限制为只接受某些 HTTP 请求类型。在这种情况下，我们只接受`GET`请求。

现在，看一下第二行。`isbn`就是我们所说的 *URL 参数*。这些是路径中的变量，稍后可以在我们的处理函数中提取。因此，如果我向`/books/000-00-00000-00-0`发送一个请求，我将能够提取`000-00-00000-00-0`作为一个变量，然后用它做一些事情。

但是等等，如果我们向`/books/all`发送请求，子路由器如何判断`all`是否是 ISBN？当我们有如上所述的冲突路线时，就会出现这些情况。我们的 subrouter 将按照从上到下的顺序匹配请求，因此将首先匹配`/all`，然后由`/{isbn}`匹配其余的请求。

有很多冲突的路线不是一个好的习惯，你应该尽量避免这些。但是，如果有必要，我们可以在 URL 参数中添加一个模式。

```
booksSubR.HandleFunc("/{isbn:^[0-9]{3}[-]{1}[0-9]{2}[-]{1}[0-9]{5}[-]{1}[0-9]{2}[-]{1}[0-9]{1}$}", booksIspnHandler).Methods(http.MethodGet)
```

这可能看起来有点疯狂，但它所做的就是允许匹配 ISBN-13 格式的变量(任何看起来像`000-00-00000-00-0`的东西)。如果你知道如何写正则表达式，你可以用它来实现一个模式。

# 结论

我们学习了如何使用`gorilla/mux`，以及如何使用它的一些定义特性，比如模式匹配、URL 参数和子程序。还有其他的路由器包，像`httprouter`和`go-chi`。我从来没试过用这些，但是我听过很多好东西。请随意试用它们！

我希望你读这篇文章的时候觉得有趣。在下一篇文章中，我将把我们的 web 应用程序连接到数据库，并执行一些查询。对于你们中的一些人来说，这个指南的速度可能太慢了，但是我故意让它慢一些，以便这个指南更容易理解。请理解！

你也可以在 [Dev.to](https://dev.to/jpoly1219/how-to-use-gorillamux-359k) 和 [my personal site](https://jpoly1219.github.io) 上阅读这篇文章。