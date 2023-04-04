# 在 Go Web 应用程序中查询数据库

> 原文：<https://blog.devgenius.io/querying-the-database-in-a-go-web-app-21883d38fb82?source=collection_archive---------8----------------------->

欢迎回到我的教程！上次，我们设置了 PostgreSQL 数据库的一个实例，并将其连接到我们的后端。这一次，我们将使用数据库并学习如何查询它。

# SQL 数据库快速介绍

尽管我们已经建立了数据库连接，但是您可能仍然不明白数据库是如何工作的。它就像我电脑上的文件浏览器吗？它是如何存储数据的？*查询*数据库意味着什么？我认为如果我们理解了数据库是如何工作的，SQL 语法会变得容易得多。

PostgreSQL 就是所谓的 *SQL 数据库*的一个例子。SQL 数据库是一种以已定义的*模式*的格式存储数据的数据库。简单地说，这是一个美化了的电子表格，其中每个数据都存储为表中的一行。一个 SQL 数据库通常有几个定义数据格式的模式(表)。

在我们的例子中，我们需要一个存储书籍数据的表。我们在 Go 中的结构定义如下:

```
type BookData struct {
    title string
    author string
    isbn string
}
```

每本书都有标题、作者和 ISBN。我们数据库中的表看起来会像这样:

标题作者 ISBN 泰坦之工具 Tim Ferriss 9781328683786 席特哈尔塔赫尔曼黑塞 9781529024043 原子习性 James Clear 9780735211292

我们将在本指南中重新定义模式和结构，以更好地满足我们的需求。

# 简单的 SQL 语法

现在我们知道了数据库是如何存储数据的，是时候学习如何使用它了。除非你使用 GUI 工具来简化操作，否则你将需要使用一种特殊的语言来操作数据库。即使你有一个 GUI 工具，知道如何使用 SQL 也是很好的。

SQL 是*结构化查询语言*的缩写，是操作数据库的主要方式。语法读起来像英语，所以很容易理解每行的意思。请求和编辑数据的行为被称为*查询*。

让我们从学习几个最重要的命令开始。此外，是的，习惯上使用全大写字母键入 SQL 命令，以将它们与数据区分开来。为了便于组织和阅读，您可以选择将查询语句分成多行。

```
CREATE DATABASE myDatabase;
```

该命令创建一个数据库。我们已经在上一个教程中创建了数据库，所以我们可以跳过这一步。

```
CREATE TABLE myTable (
    column1 dataType columnConstraints,
    column2 dataType columnConstraints,
    column3 dataType columnConstraints,
    tableContrainsts
);
```

这就是如何创建像本教程第一部分中那样的表。

*   您选择表的名称。
*   然后，您必须指定列名、数据类型和约束。

约束是分配给每列的选项。例如，`NOT NULL`约束将强制该列具有非空值。一个`PRIMARY KEY`约束将把该列指定为可以标识每一行的唯一值。

```
SELECT column1 FROM myTable WHERE condition;
```

这是从表中选择某些数据的方法。该命令是不言自明的:从满足条件的表中选择一列或一组列。

```
INSERT INTO myTable (column1, column2, ...)
VALUES (value1, value2, ...);
```

这是将数据插入特定列的方式。`value1`被插入`column1`，反之亦然。

```
UPDATE myTable
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

这就是如何更新给定列中的现有值。

```
DELETE myTable WHERE condition;
```

这就是删除符合条件的现有值的方法。

# 备餐到位

我们的数据库目前什么都没有。我们应该在后端做任何事情之前做好准备。让我们首先创建我们的表。

```
CREATE TABLE books (
    isbn VARCHAR(13) PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    author VARCHAR(50) NOT NULL
);
```

这个脚本创建了一个名为`books`的表，该表包含列`isbn`、`title`和`author`。

*   `VARCHAR(length)`是最大长度的字符串使用的类型。`VARCHAR(13)`将只接受不超过 13 个字符的字符串。我们将使用中间没有连字符的 ISBN 13，因此只分配 13 个字符听起来不错。
*   `PRIMARY KEY`是应用于列的约束，用于在表中唯一标识一行。`PRIMARY KEY`列中的所有项目必须是唯一的，不能为空。
*   `NOT NULL`是一个约束，用于确保列中没有数据为空。

好了，我们创建了表。我们现在可以通过插入新行来播种数据库，但是我们将在后端这样做。如果您现在想添加一些书籍，可以继续使用以下脚本:

```
INSERT INTO books (isbn, title, author)
VALUES ("9781328683786", "Teen Titans", "Tim Ferriss");INSERT INTO books (isbn, title, author)
VALUES ("9781529024043", "Siddhartha", "Herman Hesse");INSERT INTO books (isbn, title, author)
VALUES ("9780735211292", "Atomic Habits", "James Clear");
```

哎呀，我们在第一本书里打错了一个字。标题应该是**泰坦们的工具**，而不是**少年泰坦**！要解决这个问题，我们可以使用以下查询语句:

```
UPDATE books
SET title = "Tools of Titans"
WHERE isbn = "9781328683786";
```

很简单，对吧？

# 在 Go 中执行查询

到目前为止，按照指南做得很好！现在我们将学习如何从我们的后端执行这些查询。让我们回到我们的代码。

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

我们应该定义到达相应端点时触发的处理程序。为了减少混乱，让我们创建一个名为`controllers.go`的新文件，并在那里编写我们的控制器。如果你想知道，我会互换使用术语*控制器*和*操作者*。

在创建`controllers.go`之前，我们需要创建一个反映数据库模式的已定义模型。这些模型应该放在`models.go`里面。

```
// models.go
package maintype Book struct {
    Isbn string `json:"isbn"`
    Title string `json:"title"`
    Author string `json:"author"`
}
```

现在我们已经定义了`Book`，我们可以在我们的`controllers.go`文件中使用它。

```
// controllers.go
package mainimport (
    "database/sql"
    "net/http"
)func AllHandler(w http.ResponseWriter, r *http.Request) {
    var books = make([]Book, 0) results, err := DB.Query("SELECT * FROM books;")
    if err != nil {
        log.Println("failed to execute query", err)
        w.WriteHeader(500)
        return
    } for results.Next() {
        var b Book err = results.Scan(&b.isbn, &b.title, &b.author)
        if err != nil {
            log.Println("failed to scan", err)
            w.WriteHeader(500)
            return
        } books = append(books, b)
    } json.NewEncoder(w).Encode(books)
}// other handler definitions go here
```

这是一个控制器函数如何查询数据库、将结果解析为 JSON，然后将它们发送回用户的示例。

```
var books = make([]Book, 0)
```

我们创建一个`Book`对象的切片。这用于存储查询结果。

```
results, err := DB.Query("SELECT * FROM books;")
if err != nil {
    log.Println("failed to execute query", err)
    w.WriteHeader(500)
    return
}
```

我们以前没见过这个代码。这是执行查询的代码片段。

*   我们在`main`函数中创建了`DB`。我们在`DB`上使用`Query`方法来执行查询。
*   查询语句是一个字符串:`SELECT * FROM books;`。星号表示我们想要从表`books`中选择所有记录。
*   该方法返回类型为`sql.Rows`的`results`。这将逐行存储查询的结果。光有这些是不够的；我们需要将内容扫描到另一个变量中。
*   如果有错误，我们用 HTTP 错误代码 500 来响应，这表示服务器端有问题。

```
for results.Next() {
    var b Book err = results.Scan(&b.isbn, &b.title, &b.author)
    if err != nil {
        log.Println("failed to scan", err)
        w.WriteHeader(500)
        return
    } books = append(books, b)
}
```

这是我们保存查询结果的地方。

*   `results.Next`遍历`results`的行。因为我们查询了所有记录，所以有许多行需要迭代。
*   `results.Scan`将行数据扫描到目标目的地。这里，我们扫描到一个名为`b`的`Book`实例。
*   一旦扫描完成，我们将`b`附加到`books`切片上。
*   如果有错误，我们用 HTTP 错误代码 500 来响应，这表示服务器端有问题。

```
json.NewEncoder(w).Encode(books)
```

这很简单。它将`books`片编码成 JSON，然后将其发送回用户。

# 处理发布请求

我们需要处理向数据库添加新书的请求。基本操作与上一个示例相同，但是查询方法会略有不同。

```
// controllers.go
package mainimport (
    "database/sql"
    "net/http"
)func NewHandler(w http.ResponseWriter, r *http.Request) {
    var b Book err := json.NewDecoder(r.Body).Decode(&b)
    if err != nil {
        log.Println("error while decoding r.Body", err)
        w.WriteHeader(400)
        return
    } queryStmt := `
        INSERT INTO books (isbn, title, author)
        VALUES ($1, $2, $3)
        RETURNING isbn;
    `
    var isbn string
    err := DB.QueryRow(queryStmt, &b.isbn, &b.title, &b.author).Scan(&isbn)
    if err != nil {
        log.Println("failed to execute query", err)
        w.WriteHeader(500)
        return
    } log.Println("%s has been added to the database", isbn)
}
```

代码看起来类似于我们的第一个例子，但略有不同。

```
var b Bookerr := json.NewDecoder(r.Body).Decode(&b)
if err != nil {
    log.Println("error while decoding r.Body", err)
    w.WriteHeader(400)
    return
}
```

*   因为用户将发送一个 JSON 对象作为请求体，所以我们需要以一种 Go-friendly 格式对它进行封送。
*   我们创建了一个名为`b`的`Book`实例，并将请求体解码给它。
*   如果出现错误，这意味着请求体无效，因此我们返回错误代码 400。

```
queryStmt := `
    INSERT INTO books (isbn, title, author)
    VALUES ($1, $2, $3)
    RETURNING isbn;
`
var isbn string
err := DB.QueryRow(queryStmt, &b.isbn, &b.title, &b.author).Scan(&isbn)
if err != nil {
    log.Println("failed to execute query", err)
    w.WriteHeader(500)
    return
}log.Println("%s has been added to the database", isbn)
```

这就是我们如何处理`INSERT`语句或任何需要我们动态创建新查询语句的语句，比如`UPDATE`。

*   `queryStmt`是查询语句。关注第二行——它有`$1, $2, $3`。这些是我们稍后将插入的数据的占位符。因为一个人的`INSERT`语句会和其他人的不一样，所以我们不能只有一个静态的查询语句。在用户告诉我们之前，我们怎么知道他们想要添加什么书呢？
*   末尾的`RETURNING isbn;`行将返回添加的图书的`isbn`。这是需要保存的有用数据，我们可以用它来做任何我们想做的事情。
*   根据查询语句，我们可以肯定地知道在查询之后只会返回一行。这就是为什么我们使用`sql.QueryRow`而不是`sql.Query`。
*   在`DB.QueryRow`中，我们传入我们的查询语句，并用数据替换`$1, $2, $3`。顺序很重要，所以在我们的例子中，`&b.isbn`将替换`$1`，反之亦然。`&b.isbn`、`&b.title`和`&b.author`是我们之前解码的`b`的字段。
*   如果有错误，我们用 HTTP 错误代码 500 来响应，这表示服务器端有问题。

您将能够从这里了解如何编写其他控制器函数。祝你好运！查看这些资源来帮助您:

*   [sql 包—数据库/sql — pkg.go.dev](https://pkg.go.dev/database/sql)
*   [PostgreSQL 教程](https://www.postgresqltutorial.com)

# 结论

祝贺你完成这部分教程。我们现在已经有了一个非常基本的 REST API，并且正在运行。运行代码，启动你最喜欢的客户端(我的是 cURL 和 Postman)，然后开始测试！随意添加更多的控制器和扩展。在构建 API 方面有更高级的主题，但是对于学习目的来说，这已经足够了。我将很快进入更高级的话题，敬请关注！

你也可以在 [Dev.to](https://dev.to/jpoly1219/querying-the-database-in-a-go-web-app-dm) 和[我的个人网站](https://jpoly1219.github.io)上阅读这个帖子。