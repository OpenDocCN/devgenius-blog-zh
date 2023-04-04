# 连接到 Go 中的数据库

> 原文：<https://blog.devgenius.io/connecting-to-database-in-go-f98c3a7a06a8?source=collection_archive---------2----------------------->

欢迎回到*Golang Web 开发简介*。上次，我们学习了如何使用`gorilla/mux`。这一次，我们将学习如何建立一个数据库并连接到它。我们开始吧！

# 从为什么开始

理解我们为什么需要数据库是很重要的。对于大多数 web 应用程序来说，数据库连接是必要的，因为需要一种可靠地持久存储数据的方法。

我们可以创建一个结构并在那里存储数据。大概是这样的:

```
type BookData struct {
    title string
    author string
    isbn string
} books := []BookData{
    {"Tools of Titans", "Tim Ferriss", "978-13-28683-78-6"},
    {"Siddhartha", "Herman Hesse", "978-1529024043"},
}
```

当然，这可能会存储书籍数据，但在我们的 RAM 中。这不是一个好主意，因为内存存储是不稳定的。当您关闭数据库时，存储在 RAM 中的数据将被销毁。

想象一下去法国旅行。你可以看到埃菲尔铁塔，吃最好的餐馆的高级菜肴，结交新朋友。将它存储到内存中就是试图在你的头脑中记住所有这些珍贵的记忆。使用数据库就像拍照并保存到你的电脑上。

对于我们的游戏玩家朋友(比如我！)，想象一下每次有服务器维护的时候，你的账号数据都在重置。呀。

使用适合存储数据的工具是个好主意。`database/sql`包提供对 SQL 数据库的支持。如果您想使用 NoSQL 数据库，您很可能需要第三方软件包。对于本教程，我们将使用 PostgreSQL。

# 创建数据库

创建数据库的简单方法是使用 Docker 映像。Docker 是一个让我们在容器内部运行服务的工具。容器化服务允许它们在隔离的环境中运行，使得运行、停止和删除变得容易。

首先，让我们安装 Docker。我强烈推荐你按照本教程使用简单的 Docker 命令。本教程将安装 Docker，并帮助您了解 Docker 是什么以及它是如何工作的。

一旦你完成了教程，我们就可以开始处理我们的容器了。创建一个名为`docker-compose.yml`的文件，编辑如下:

```
version: "3.7"services:
  database:
    container_name: database
    image: postgres
    ports:
      - "5432:5432"
    volumes:
      - ./database:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=bookstoreDB
      - POSTGRES_USER=jacob
      - POSTGRES_PASSWORD=password
```

上面的文件做了几件事:

*   它从 Docker Hub 中提取`postgres`图像。
*   容器打开 5432 端口让我们连接。
*   它设置连接到数据库时要使用的环境变量。
*   POSTGRES_USER 和 POSTGRES_PASSWORD 可以是任何值。对于本教程，我使用`jacob`和`password`。
*   由于容量的原因，数据在`/var/lib/postgresql/data`保存在容器中。

我们可以使用以下命令来运行它:

```
docker-compose up -d
```

我们可以使用以下命令来停止这种情况:

```
docker-compose down
```

一旦容器开始运行，我们就可以使用名为 pgAdmin 的工具连接到数据库。在这里为您的操作系统[安装 pgAdmin。安装后，打开应用程序。向左，右键点击`Servers`，并转到`Create > Server`。](https://www.pgadmin.org/download/)

*   在“常规”选项卡中，随意命名。转到连接选项卡。
*   主机名/地址是`localhost`。
*   端口是`5432`。
*   维护数据库是`bookstoreDB`。
*   用户名和密码是您在`docker-compose.yml`文件中设置的。

保存后，pgAdmin 将创建一个新的连接。一旦我们下拉到`bookstoreDB`，我们可以点击顶部工具栏的`Tools > Query Tool`。这将打开一个窗口，我们可以在其中键入和执行 SQL 查询。

好吧，设置过程很长，但我们以后不用经常碰它了。让我们继续进行代码。

# 休斯敦，你收到了吗？

我们知道为什么我们需要一个数据库，并且已经创建了一个 PostgreSQL 实例。现在我们需要从我们的围棋程序中建立一个连接。

```
// main.go
package mainimport (
    "database/sql"
    "fmt"
    "log" "github.com/gorilla/mux"
    _"github.com/lib/pq"
)var DB *sql.DBconst (
    HOST = "localhost"
    PORT = 5432
    USER = "jacob"
    PASSWORD = "password"
    DBNAME = "bookstoreDB"
)func main() {
    connString := fmt.Sprintf(
        "host=%s port=%d user=%s password=%s dbname=%s sslmode=disable",
        HOST, PORT, USER, PASSWORD, DBNAME,
    ) DB, err := sql.Open("postgres", connString)
    if err != nil {
        log.Fatal(err)
    }
    defer DB.Close() // mux definition and route registration (from last tutorial)
    r := mux.NewRouter() r.HandleFunc("/", homeHandler) booksSubR := r.PathPrefix("/books").Subrouter() booksSubR.HandleFunc("/all", AllHandler).Methods(http.MethodGet)
    booksSubR.HandleFunc("/{isbn}", IspnHandler).Methods(http.MethodGet)
    booksSubR.HandleFunc("/new", NewHandler).Methods(http.MethodPost)
    booksSubR.HandleFunc("/update", UpdateHandler).Methods(http.MethodPut)
    booksSubR.HandleFunc("/delete/{isbn}", DeleteIspnHandler).Methods(http.MethodDelete) log.Fatal(http.ListenAndServe(":8090", r))
}
```

这是一种非常简单、易于理解的建立联系的方式。

```
import (
    "database/sql"
    "fmt"
    "log" "github.com/gorilla/mux"
    _"github.com/lib/pq"   
)
```

最后一条进口声明看起来很可疑。为什么开头有下划线？还有，Go 已经提供了`database/sql`包，为什么还要下载这个包？这些都是很棒的问题！

*   `database/sql`旨在支持许多 SQL 和类似 SQL 的数据库。它不包括所有数据库的驱动程序。包开发者认为将驱动程序从`database/sql`包中分离出来会更有效。可以把它想象成一个模块化的旅行适配器，其中电源模块保持不变，但插脚可以互换。
*   `github.com/lib/pq`是 PostgreSQL 的驱动程序。这个包有点不同，因为代码中没有我们使用这个驱动程序的地方。因此，我们在前面加了一个下划线，表示用户正在使用这个驱动程序，但没有主动调用它。

确保运行`go mod tidy`来更新依赖列表。

```
var DB *sql.DB
```

*   我们使用`var DB *sql.DB`将连接数据存储为一个全局变量。
*   数据库被表示为指向结构`sql.DB`的指针，因为 DB 本身就是一个大结构。取它的指针更有效率。

如果你不知道指针是什么，那它就是一个存储对象内存地址的变量。

*   具有多个属性的大型结构等对象会占用大量空间。传来传去很繁琐，因为程序每次都需要复制很多数据。
*   如果我们使用一个指针，我们只是传递一个对对象的引用，这要简单得多。
*   当我们整理文件时，我们倾向于创建某些文件夹的快捷方式，对吗？我们不必将文件复制到桌面上。我们只需要创建一个快捷方式，这样可以节省大量宝贵的千兆字节。

总有一天，我会写一本关于指针的指南，这样更多的人可以得到帮助。

```
const (
    HOST = "localhost"
    PORT = 5432
    USER = "jacob"
    PASSWORD = "password"
    DBNAME = "bookstoreDB"
)func main() {
    connString := fmt.Sprintf(
        "host=%s port=%d user=%s password=%s dbname=%s sslmode=disable",
        HOST, PORT, USER, PASSWORD, DBNAME,
    ) DB, err := sql.Open("postgres", connString)
    if err != nil {
        log.Fatal(err)
    }
    defer DB.Close()
}
```

*   为了连接到我们的数据库，我们需要定义主机、端口、用户名、密码、数据库名和 SSL 模式。这些不会改变，所以把它们声明为常量是个好主意。
*   这些值遵循我们在`docker-compse.yml`文件中定义的环境变量。
*   `connString`是保存所有这些变量的字符串，作为选项传递给`sql.Open()`。
*   `sql.Open()`将我们的程序连接到数据库。它的返回值存储在我们的`DB`变量中。
*   我们需要在应用程序不运行时关闭我们的连接，所以我们使用`defer`关键字在应用程序停止前关闭连接。

使用全局变量适用于像这样的简单应用程序，但不建议在生产中使用，因为全局变量对于突变是不安全的。简单地说，我们无法控制全局变量会发生什么。随着您的进步，依赖注入将会被更频繁地使用。然而，这个概念超出了今天教程的范围。

# 结论

感谢您的阅读！我希望今天讨论查询，但是这会使教程太长。此外，引入 Docker 和 PostgreSQL 等新工具已经让人难以接受。下一个教程将是关于查询数据库的，所以请继续关注！

你也可以在 [Dev.to](https://dev.to/jpoly1219/connecting-to-database-in-go-4m95) 和[我的个人网站](https://jpoly1219.github.io)上阅读这个帖子。