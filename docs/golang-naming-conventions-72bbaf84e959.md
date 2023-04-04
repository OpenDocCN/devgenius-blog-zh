# Golang 命名约定

> 原文：<https://blog.devgenius.io/golang-naming-conventions-72bbaf84e959?source=collection_archive---------0----------------------->

![](img/f7b888c8ae69c7a2abbfb5a582174aa4.png)

让我们来谈谈 Golang 的一个有趣的话题，因为并不是所有的项目都遵循标准或惯例来命名代码资源和代码属性。有时，我们会看到将它们全部应用于一些命名规则或约定，在某些情况下，我们会看到在不同项目中如何定义文件名称的差异，作为团队或公司的标准。

现在我想和你分享我在 Golang 上看到或定义命名约定的方式，这些不是我的定义，我从“官方命名约定”中得到它们，并适合我一直在工作的项目。

在本文中，我们将分析代码中重要部分的命名约定。

```
- Project Naming
--- Package Naming
------ Files Naming
--------- Functions
--------- Structures
--------- Variables
--------- Constants
```

## 项目名

![](img/ccd55226e6394bca8bd5bc0ef2052ffa.png)

*   使用小写字母命名您的项目，如果您在库中工作，这将非常有帮助。使用小写的 imports、repository 或 go.mo 会更好看。

*举例:*

```
github.com/gituser/project
```

*如你所见，它比*更好看

```
github.com/gituser/Project
```

*甚至:*

```
github.com/gituser/PROJECT
```

*   如果名字很长，并且是单词的组合，你应该使用破折号或下划线来分隔每个单词。此时最常见的是看到项目名称带有破折号。

*举例:*

```
my-repository
```

*或:*

```
my_repository
```

*比:*好看

```
MyRepository
```

```
myRepository
```

## 包名

![](img/73b1b665b354c3149e08a0b29cbe19f8.png)

*   包名尽量简短，最好用小写，千万不要用 camelCase，甚至不建议用 snake_case。

*例如，使用:*

```
controller
user
mypackage
```

*而不是:*

```
my_package
myPackage
```

*   对于包装，你应该使用单数名词而不是复数名词。

*例如:*

```
account
controller
user
repository
service
```

而不是:

```
accounts
controllers
users
repositories
services
```

对于这个建议，我们可以找到一个例外，您可以使用复数名词，因为某些原因使用单数名词会导致与现有 golang 核心标准库的冲突，这样做将有助于您不混淆实现。

例如，如果你的包用单数名词叫做“**错误”**你可以用**错误。**

*   如果您的软件包是 golang 标准库的扩展，您可以在名称后添加一个后缀，例如:

*为*

```
context
http
error
```

你可以用

```
context => contextext, contextx, etc
http => httpext, httpxt, etc
error => errorext, errorxt, etc
```

## 文件名

![](img/82dde25b40663459626f7ceb339c51fe.png)

*   像包一样，文件名应该尽可能的短，但是根据内部定义的代码或者文件的目的使用描述性的和有意义的。

*例如:*

```
account.go
configuration.go
dbconfig.go
```

*   如果文件名包含多个单词，应该使用 snake_case.go 而不是 camelCase.go。

*例如:*

```
configuration.go
database.go
db_config.go
account_service.go
account_service_notification.go
```

*而不是:*

```
accountController.go
dbConfiguration.go
accountService.go
```

## 功能名称

![](img/6b3617d5b34af0909b81b1960c576a14.png)

这些建议适用于函数和方法

*   使用 camelCase 定义名称，例如:

```
getSomthing()
DoSomething()
print()
```

*代替使用:*

```
get_somthing()
Do_something()
print_names()
```

*   函数/方法名应该描述它做什么，你不应该有一个名字和执行另一个动作。

*示例使用:*

```
func getSomeList() {
         .
         .
         .    
   return myList
}
```

*而不是:*

```
func getSomeList(id string) {       
     updateSomething(id)
}
```

*   如果函数/方法将上下文定义为一个参数，并接收其他参数，则上下文必须是第一个参数。

*示例使用:*

```
func doSomething(ctx context.Context, account userAccount) {
         .
         .
         .    
}
```

*而不是:*

```
func doSomething(account userAccount, ctx context.Context, id int32) {
         .
         .
         .    
}
```

*   如果你的函数/方法有一个上下文作为参数。你应该用“**CTX”**作为名字，而不是 **myContext，myCtx，currentCtx，cContext** 等。
*   定义方法时，接收方应该使用实现该方法的结构名中的一个、两个、最多三个字母。

*例如，使用:*

```
type Service struct {}
```

```
func (s Service)doSomething(ctx context.Context, account userAccount) {
         .
         .
         .    
}
```

*或使用:*

```
type Service struct {}
```

```
func (svc Service)doSomething(ctx context.Context, account userAccount) {
         .
         .
         .    
}
```

*对于组合:*

```
type AccountService struct {}
```

```
func (as Service)doSomething(ctx context.Context, account userAccount) {
         .
         .
         .    
}
```

*从不使用:*

```
type Service struct {}
```

```
func (myService Service)doSomething(ctx context.Context, account userAccount) {
         .
         .
         .    
}
```

*或:*

```
type Service struct {}
```

```
func (service Service)doSomething(ctx context.Context, account userAccount) {
         .
         .
         .    
}
```

## 变量和常量名称

![](img/97cedec10fe80341dff39bb6f9ad4481.png)

让我们从**常量开始:**

*   对于命名，常量使用大小写，而不是全部大写或用下划线分隔。

*例如，使用:*

```
tcpConnection
tcpUrl
tcpUser
tcpPass
port
```

*而不是:*

```
TCP_CONNECTION
TCP_USER
TCP_PASS
PORT
```

*   与相同主题/上下文相关的所有常量应该定义相同的前缀，以便更容易识别每个常量。

*例如，使用:*

```
For TCP logic by example:tcpConnection
tcpUrl
tcpUser
tcpPass
tcpPort
```

```
For DataBase logic by example:
dbConn
dbUrl
dbUser
dbPass
dbPort
```

*而不是:*

```
For TCP logic by example:tcpConnection
urlTcp
tcpUser
pass
portFor DataBase logic by example:
dbConn
urlForDb
user
pass
port
```

现在是**变量的时候了:**

*   名称应该简短，便于描述
*   该名称应使用骆驼案例
*   如果是两个单词的组合，你可以用每个单词的第一个字母。
    *例如:*

```
var wg sync.WaitGroup
```

*   对于在循环中用来保存索引的变量，可以只使用一个字母。

## 结构和接口名称

![](img/bc78eb48210908c29a78755404a673c0.png)

*   通常，您可以通过包含后缀“er”来定义您的接口名称。

*例如:*

```
type Doer interface{
     Do()
}
```

```
type Writer interface{
     Write()
}
```

```
type Builder interface{
     Build()
}
```

*   如果您的接口将用于定义服务或存储库操作，您可以只将其命名为“服务”，“存储库”(如果它是通用的)，或者在其中将包/结构的名称连接起来，如“AcountsService”，“QueryBuilder”，“FileWriter”，“ConsoleWriter”等。

## 结论

所有这些建议将有助于我们为其他开发人员编写易于阅读的代码，我们将开始在我们的应用程序中使用标准，目的是让我们的所有服务都具有相同的代码格式，如果我们需要使用一个我们从未见过的代码，那么它将变得更容易理解并使其更具可读性。

感谢阅读！
希望这篇文章对你有帮助！

![](img/173471e4521bb91ca20208a6cdddba5d.png)