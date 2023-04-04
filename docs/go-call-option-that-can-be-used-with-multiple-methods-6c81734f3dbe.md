# Go:可用于多种方法的看涨期权

> 原文：<https://blog.devgenius.io/go-call-option-that-can-be-used-with-multiple-methods-6c81734f3dbe?source=collection_archive---------2----------------------->

允许在函数和方法调用中共享选项，现在有了类型安全！

![](img/9fe8c838c51ed2daf71030acc984cb6e.png)

照片由 [Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

无耻宣传片:想知道如何提高你的围棋开发能力，请查看我的新书: [**Go For DevOps**](https://www.amazon.com/Go-DevOps-language-Kubernetes-Terraform/dp/1801818894/ref=sr_1_1?crid=3UJVTH5WHIGI8&keywords=go+for+devops&qid=1656568378&sprefix=go+for+devops%2Caps%2C126&sr=8-1)

# 背景

几年前，Google 的 Go devs 提出了函数或方法调用的选项，这些选项使用函数调用来设置选项值。

Rob Pike 有一个版本，你可以在这里阅读:[https://command center . blogspot . com/2014/01/self-referential-functions-and-design . html](https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html)

举个例子，某个服务的客户端构造函数有一个设定的端点。当 99%的情况是“**https://myclientendpoint . com”**时，也许我们不想给用户设置那个端点增加负担。但是我们偶尔需要覆盖它。我们可能会这样做:

```
type SomeClient struct {
  http *http.Client
  endpoint string
}// Option is an optional argument to New().
type Option func(c *SomeClient)// WithEndpoint is an option that makes the client use the 
// endpoint passed.
func WithEndpoint(endpoint string) Option {
  return func(c *SomeClient) {
    c.endpoint = endpoint
  }
}func New(*httpClient *http.Client, options ...Options) *SomeClient { 
  c := &SomeClient{
    http: httpClient,
    endpoint: "https://myclientendpoint.com",
  }

  for _, o := range options {
    o(c)
  }
  return c
}
```

现在我们可以使用`WithEndpoint()`来设置一个超过默认值的自定义端点:

```
c := New(&http.Client{}, WithEndpoint("https://newendpoint.com"))
```

这是最常见的用例，构造函数选项。

`WithEndpoint()`返回函数闭包。也就是说，它返回一个可以访问其外部环境的函数，比如`endpoint`，即使它没有在内部定义。使用一个函数返回一个函数经常让人们感到困惑，因为不是所有的语言都有一级函数(可以是赋给变量的类型的函数)。

# 方法调用选项

另一个用例是为方法调用提供可选设置。大概是这样的:

```
// tokenOpts holds all our optional token values.
type tokenOpts struct {
  account string
}// TokenOption is an optional argument to ByToken().
type TokenOption func(t *tokenOpts)// WithAccount sets the account to name. For use with ByToken().
func WithAccount(name string) TokenOption {
  return func(t *tokenOpts) {
    t.account = name
  }
}// ByToken does something with tokens.
func (a *Auth) ByToken(token string, options ...TokenOption) (string, error) {
  opts := tokenOpts{account: "default"}
  for _, o := range options {
    o(&opts)
  }
  // Do whatever the method does below here
}
```

这给出了类似的语义，只是在方法级别。这非常有效，直到有多个方法可以使用同一个选项。

在上面的场景中，如果我们想要使用`WithAccount()`作为一个名为`ByUserPass()`的新方法的选项，而这个新方法没有所有相同的选项，会发生什么呢？

大多数解决这个问题的方法都不太好。例如，我们可以有一个共享的可选选项`struct`,里面有所有方法的选项。但是这样我们就失去了编译器类型的安全保护，因为你可以在方法`X()`上使用一个`OptionB()`，而这个方法只能在方法`K()`上使用。

我们可以有两个(或者更多)方法来设置相同的值，但是类型不同，比如`WithAccountToken()`和`WithAccountUserPass().`是的，这对我们来说是行不通的。

# 要解决的场景

我想出了一种方法来解决这个问题，并保持 Go 开发人员希望他们的调用选项的语义。这个方法**滥用了**一些不常使用的语言特性，而且冗长。但是对于最终用户来说，它仍然易于使用。

这不适合胆小的人。

让我们从一个示例场景开始:

*   方法 ByToken()
*   方法 ByUserPass()
*   一个带有 Account()的选项，这两种方法都可以使用
*   一个选项，带有只能由 Token()使用的 Verifier()

为此，我们将使用一些不常使用的功能:

*   `interface`嵌入在`struct`型中
*   未命名的`struct`类型
*   未命名的`interface`类型

在`struct`中嵌入一个`interface`允许`struct`满足那个`interface`，即使它没有实现这些方法。

所以我们可以让一些`struct`像这样实现`io.Reader`:

```
type myStruct struct {
  io.Reader
}
```

现在，你可能会问，如果有人在`myStruct`上调用`Read()`会发生什么？嗯，它会恐慌的。然而，我们这样做主要是为了加强类型检查，所以我们不必担心这个问题。

未命名的`struct`类型在表驱动测试中一直被使用，但是你很少在其他代码中看到它。使用这种方法，您可以设计`struct`，然后一次性实现所有功能，如下所示:

```
myVar := struct{
  name string
}{
  name: "John Doak",
}
```

在这里，我们创建一个具有`name`属性的`struct`，它是一个`string`类型，并用设置为“John Doak”的`name`实例化它。同样，这不是你经常在考试之外看到的。

未命名`interface`与未命名`struct`类型相似:

```
var myInter interface{
  someMethod() error
}
```

这创建了一个变量`myInter`，它只能由具有`someMethod() error`方法的类型来满足。同样，你很少在野外看到这种情况。

# 解决方案

我已经为您打包了一个助手解决方案，但是该解决方案的 99%将在您的代码中实现该方法。我们要进口的包裹在 https://github.com/johnsiilver/calloptions。

```
type tokenOptions struct {
  account string
  verifier string
}// tokenOption is an optional argument for ByToken().
// I've chosen to keep it private, but that is not required.
type tokenOption interface {
  token() // This must be kept private.
}func (c *Client) ByToken(token string, options ...tokenOption) (string, error) {
  opts := tokenOptions{account: "default"} 
  if err := calloptions.ApplyOptions(&opts, options); err != nil {
    return "", err
  }
  // Put the rest of your code here.
}type userPassOptions struct {
  account string
}type userPassOption interface {
  userPass()
}func (c *Client) ByUserPass(user, pass string, options ...userPassOption) (string, error) {
  opts := userPassOptions{account: "default"}
  if err := calloptions.ApplyOptions(&opts, options); err != nil {
    return "", err
  }
  // Put the rest of your code here.
}
```

现在我们已经设置了两个方法。每个方法都有一个名为`options`的变量参数，它有一个特定于该方法的`interface`类型。

`calloptions.ApplyOptions()`执行一个`for`循环，并应用传递给代表该方法选项的`struct`的每个选项。

为此，传递的选项还必须满足`calloptions.CallOption` `interface`。

`interface`定义为:

```
type CallOption interface {
  Do(a any) error 
  callOption()
}
```

通过向`calloptions.New()`构造函数传递一个`func(a any) error`来创建一个`CallOption`。我们马上就会看到。

使用您定义的函数，您将 assert `a`键入您期望的类型，然后设置值。因此，func 可能看起来像:

```
func(a any) error {
  t := a.(*tokenOptions)
  t.account = account
  return nil
}
```

这将包含在一个闭包里，我们马上会展示它。我们可以扩展它，使其涵盖两种情况:

```
func(a any) error {
  switch t := a.(type) {
  case *tokenOptions:
    t.account = account
  case *userPassOptions:
    t.account = account
  default:
    panic("not implemented bug")
  }
  return nil
}
```

让我们展示一下当我们想要创建一个共享选项时的情况:

```
func WithAccount(name string) interface{
  tokenOption
  userPassOption
  calloptions.CallOption
}{
  return struct{
    tokenOption
    userPassOption
    calloptions.CallOption
  }{
    CallOption: calloptions.New(
      func(a any) error {
        switch t := a.(type) {
        case *tokenOptions:
          t.account = name
        case *userPassOptions:
          t.account = name
        default:
          panic("not implemented bug")
        }
        return nil
      },
    ),
  }
}
```

哇…所以是时候打开这个了…

`WithAccount()`将要返回一个未命名的`interface`。那个`interface`嵌入了`tokenOption`、`userPassOption`和`CallOption`。这让我们可以在`ByToken()`和`ByUserPass()`中使用它。

它是一个闭包，所以它从外部方法获取“account”变量。我们定义的`CallOption`将被调用来设置我们的可选`struct`，你可以看到它将设置`*tokenOptions`和`*userPassOptions`。

让我们实现只能和`ByToken()`一起使用的`WithVerifier()`。

```
func WithVerifier(endpoint string) interface {
  tokenOption
  calloptions.CallOption
} {
  return struct {
    tokenOption
    calloptions.CallOption
  }{
    CallOption: calloptions.New(
      func(a any) error {
        t := a.(*tokenOptions)
        t.verifier = endpoint
        return nil
      },
    ),
  }
}
```

因为`WithVerifier`返回一个只实现`tokenOption`的`interface`，所以只能和`ByToken()`一起使用。

因此，当用户想要使用这些选项时，它看起来就像他们使用过的所有其他选项一样:

```
c := Client{}
c.ByToken("token", WithAccount("subID"), WithVerifier(" address"))
c.ByUserPass("user", "pass", WithAccount("subscriptionID"))// Uncommenting the next line will give a compiler error
// c.ByUserPass("user", "pass", WithVerifier("some address"))
```

这符合所有标准。坦率地说，如果您在没有实际扩展支持的情况下扩展该选项支持的方法，确实有可能导致混乱。但这是非常低的风险。我很想消除这种情况，但目前还没有找到办法。

你可以在这里看到这个例子:【https://go.dev/play/p/2Z3S6qv5xi7】T4

在多个方法调用中支持一个调用选项并不经常出现，但是一旦出现，解决起来会很痛苦。希望这能给你一个工具，当这个问题出现时，你可以用它来解决。

如果你想看这个东西的运行，使用 git 来检查[https://github.com/johnsiilver/calloptions](https://github.com/johnsiilver/calloptions)并运行 example/中的代码。

如果你已经做到了这一步，并且正在做 DevOps、SRE 或 NoOps，请查看我的书: [Go For DevOps](https://www.amazon.com/Go-DevOps-language-Kubernetes-Terraform/dp/1801818894/ref=sr_1_1?crid=3UJVTH5WHIGI8&keywords=go+for+devops&qid=1656568378&sprefix=go+for+devops%2Caps%2C126&sr=8-1) 。我们从一开始就教你如何构建平台提供者、GitHub 操作、扩展 Packer、使用开放式遥测技术实现可见性、设计 ChatOps 系统以及使用策略服务器构建自己的工作流引擎等等。

![](img/561c94d018534c8ee9df0ca580a1ca93.png)

Go For DevOps，在你最喜欢的书店有售

你也可以在 www.gophersre.com[找到更多我写的东西](http://www.gophersre.com)