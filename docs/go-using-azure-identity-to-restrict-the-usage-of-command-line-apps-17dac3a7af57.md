# go-使用 Azure 身份来限制命令行应用的使用

> 原文：<https://blog.devgenius.io/go-using-azure-identity-to-restrict-the-usage-of-command-line-apps-17dac3a7af57?source=collection_archive---------6----------------------->

![](img/d1f71f335357180b696d68c29d0629d5.png)

Philipp Katzenberger 在 [Unsplash](https://unsplash.com/s/photos/security?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在这个故事中，让我们讨论一种限制使用 GO 开发的命令行应用程序的方法。我们为什么要这么做？一个原因是如果命令行实用程序与受保护的资源交互，就像我的例子一样。如果您想审计谁使用了命令行工具，它也可能会派上用场。

为此，我使用了微软的**叠氮实体**包。除了这个包，我还使用了 **azcore/policy** 包和 **golang-jwt** 包。

这个故事着重于处理这个端到端的问题。所以让我们从初始化模块的第一步开始。在运行这一步之前，创建一个名为**的目录，并将 *cd* 放入其中。**

```
go mod init github.com/thekfaktor/azidentityverification
```

下一步是如前所述安装必要的包。

```
go get -u github.com/Azure/azure-sdk-for-go/sdk/azidentity
go get -u github.com/Azure/azure-sdk-for-go/sdk/azcore/policy
go get -u github.com/golang-jwt/jwt/v4
```

现在我们可以创建所需的 go 文件了。下面的步骤在 azure 目录中创建一个 main.go 文件和一个 identity.go 文件。

```
type NUL > main.go
type NUL > azure\identity.go
```

> 如果您还没有注意到，上面的命令是 windows 的“触摸”等价物！

以下要点包含**身份**模块检查用户是否通过身份验证所需的代码。其目的不仅是验证用户是否通过了身份验证，而且是为了确保他们使用的是属于某个目录的帐户。顺便说一句，在文章的结尾有更多的观察。

为此，我首先验证用户是否使用默认 Azure 凭据进行了身份验证。之后，我获取登录用户的令牌，并使用 **golang-jwt** 包对其进行解码。最后，我得到了 **iss** 和 **unique_name** 声明的值。iss 声明标识了发行者，我使用一个简单的 if 条件来检查发行者是否与我的租户的期望值相匹配。

IsUserAuthenticated 函数返回一个 **UserInfo** 对象和一个错误对象。如果处理成功，返回的 **UserInfo** 对象包含发布者和登录用户的电子邮件地址，而 error 对象为 **nil** 。另一方面，如果认证/验证由于许多原因中的一个而失败，则返回一个空的 **UserInfo** 对象以及一个错误对象，该对象提供了关于错误的更多细节。

现在我们有了身份模块，我可以将它放入到 **main.go** 文件中使用，如下所示。在这种情况下，我通过将 LHS 设置为 _ 来丢弃没有错误时返回的 **UserInfo** 对象，但是如果需要，您可以选择显示登录用户的电子邮件地址。

尽管输出中没有什么有趣的东西，但我还是想添加它们。输出中的第二行很有趣。错误出现在尝试令牌检索的步骤中，而不是调用**NewDefaultAzureCredential**的步骤中。一旦有了更多的清晰，我会用这些细节更新这个故事。

![](img/9745d7d57e2daf288665b1448d73b5b7.png)

未经验证的用户的输出

一旦用户使用`az login`登录，输出将类似于下面这样。

![](img/fc3b0d2f135063ce02727b6fa0958f0b.png)

经过身份验证的用户的输出

补充一下，下面的 az cli 命令将显示当前登录用户的详细信息。

```
az ad signed-in-user show
```

在对用户进行身份验证时有多种选择，这个故事使用默认的 azure 凭据。如果你感兴趣，这篇文章也关注其他方法。

如果你对这个故事有任何想法，请随意添加评论。如果有更好的方法，我也想听听。感谢阅读！