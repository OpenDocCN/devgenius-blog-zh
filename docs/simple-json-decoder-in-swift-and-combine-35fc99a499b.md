# Swift 和 Combine 中的简单 JSON 解码器

> 原文：<https://blog.devgenius.io/simple-json-decoder-in-swift-and-combine-35fc99a499b?source=collection_archive---------3----------------------->

![](img/32a6740994dd41ff3b287d9a87960b0c.png)

克里斯蒂安·威迪格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

现在几乎每个应用程序都需要你连接到互联网才能访问一些内容。这些应用中的大部分使用 [JSON](https://www.w3schools.com/whatis/whatis_json.asp) 来交流来自后端系统的数据。

您的应用程序中很有可能会有一些代码来下载、解析和返回对象，供您的应用程序从端点使用(除非您使用的是网络库，如 [Alamofire](https://github.com/Alamofire/Alamofire) )

在这篇文章中，我们将展示如何使用[泛型](https://docs.swift.org/swift-book/LanguageGuide/Generics.html)和 [Codable](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) 来帮助我们构建一个简单的可重用 JSON 解码器来下载和解析来自端点的响应。

# 构建我们的可编码对象

我们需要做的第一件事是构建我们的可编码对象。实现可编码协议的对象允许[编码器](https://developer.apple.com/documentation/swift/encoder)和[解码器](https://developer.apple.com/documentation/swift/decoder)将它们编码或解码到外部表示，如 JSON。

让我们以下面的示例端点的响应为例:

[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)

您可以自己手动创建可编码的类。在简单的例子中，这可能相当简单，但是如果您的响应具有更复杂的结构，那么这样做可能会很耗时并且容易出错。

为了创建我们的可编码对象，我们可以使用生成器，我选择的武器是 [QuickType](https://app.quicktype.io) 。我们只需粘贴从 posts 端点返回的 JSON，它就会自动为我们生成可编码的结构。轻松点。

如果我们粘贴我们的 post 响应，我们应该得到如下所示的代码:

有多简单？！显然，我们仍然需要检查结构，在上面的例子中，没有一个字段是可选的，这意味着必须传入数据，否则我们的解码将会失败。在这里我们不需要担心这个问题，但是在检查示例中生成的代码时值得记住。

# URLSession 扩展和泛型

为了解决我们的问题，我们将包装现有的 URLSession [dataTask](https://developer.apple.com/documentation/foundation/urlsession/1410330-datatask) 方法。我敢肯定，如果你在 pure swift 中做过任何类型的请求工作，你会以某种形式使用这种方法，所以我们不打算详细讨论它是如何工作的。

因此，让我们一步一步地浏览这个代码示例:

1.  首先，我们为这个扩展定义了一个自定义错误，当请求没有返回任何数据时，就会返回这个错误，这在第 6 点中讨论过。如果我们得到一个状态代码不正确的 HTTPURLResponse，我们也会遇到一个错误情况，这在第 5 点中有所涉及。
2.  这里我们利用[泛型](https://docs.swift.org/swift-book/LanguageGuide/Generics.html)来允许任何类型的 T 从这个函数返回，只要类型 T 实现了[可解码](https://developer.apple.com/documentation/swift/decodable)协议(我们需要它来使用 [JSONDecoder](https://developer.apple.com/documentation/foundation/jsondecoder)
3.  如前所述，这里我们调用现有的 [dataTask](https://developer.apple.com/documentation/foundation/urlsession/1410330-datatask) 方法来运行我们的请求。
4.  一旦请求返回，我们要做的第一件事是检查是否有请求错误，如果有，我们调用带有响应和错误的完成处理程序。
5.  我们执行的第二个检查是检查我们是否收到了 HTTPURLResponse 的状态代码。注意，如果我们没有得到 HTTPURLResponse，我们不会在这里停止代码，因为您可以使用这个函数加载本地 JSON 文件，而不仅仅是远程 URL。任何在 200–299 范围内的状态码都被认为是成功的请求，如果我们收到一个在这个范围之外的状态码，我们将返回一个错误和响应，以便由通过完成处理程序的人进一步处理。
6.  我们执行的第三个检查是打开准备解码的数据。如果这失败了(因为它是 nil ),那么我们调用 completionHandler 并给出响应和我们在步骤 1 中定义的自定义错误。
7.  难题的最后一部分是尝试将数据解码为我们在方法签名中定义的 T 类型，这是步骤 2 的一部分。如果成功，我们可以用解码的类型和响应调用完成处理程序。如果它抛出一个错误，我们使用下面的 catch 块捕获错误并返回它。

# 看看它的实际效果

既然我们已经把功能组装好了，就让我们试一试吧。

这看起来不应该太吓人，事实上，如果您以前在代码中使用过标准的 dataTask 函数，这应该看起来非常熟悉。这里唯一的不同是，我们的完成处理程序现在返回我们的可编码用户对象，而不像以前那样只是一个数据块。

希望这个例子有意义，给你一个简单的方法来执行一个请求，让它把一些 JSON 解码成一个 struct / class。现在让我们看看一些使用 Combine 的反应式编程。

# 结合

希望你至少听说过 [Combine](https://developer.apple.com/documentation/combine) ，即使你还没有机会在生产应用中使用它。这是苹果自己版本的反应式框架。那些已经在使用 [RxSwift](https://github.com/ReactiveX/RxSwift) 的人将会感觉如鱼得水。我们不会对什么是组合做太多的详细说明，但是这里有一个关于什么是反应式编程的定义:

> *在计算领域，反应式编程是一种声明式编程范式，关注数据流和变化的传播*

更简单地说，反应式编程使用观察者模式来允许类监控不同的数据流或状态。当这个状态改变时，它发出一个具有新值的事件，该事件可以触发其他流执行工作或更新，例如 UI 代码。如果你熟悉 KVO，你会明白它的基本概念。然而，反应式编程远没有 KVO 那么痛苦，也更强大。

现在让我们以之前的 pure Swift 为例，看看如何在 Combine 中使用它。Combine 框架以 [dataTaskPublisher](https://developer.apple.com/documentation/foundation/urlsession/3329708-datataskpublisher) 函数的形式向 URLSession 添加了新的反应功能。

与前面的例子类似，我们扩展了 URLSession 来提供这个功能。让我们一步一步来:

1.  与纯 Swift 示例一样，我们在这里定义了一个自定义错误，以便在收到不成功的状态代码时进行处理。不同之处在于，我们将响应附加到错误上，因为我们在 Combine 中没有 completionHandler。这样，无论是谁处理错误，都可以检查响应并了解失败的原因。
2.  这里我们定义了方法，再次使用泛型只接受实现了可解码协议的类型 T。该函数返回一个发布者，该发布者返回我们解码的对象。
3.  如前所述，我们只是包装了现有的 dataTaskPublisher 方法。
4.  现在事情开始变得被动。 [tryMap](https://developer.apple.com/documentation/combine/publishers/merge/3209016-trymap) 函数类似于标准的 Map 函数，它试图将元素从一种类型转换成另一种类型。然而，这里的区别是，它几乎是包裹在一个尝试。在这种情况下，您可以在抛出错误的闭包中包含代码，这些错误将被推送到下游并在以后处理，而不需要 do 块。与我们的纯 Swift 示例类似，我们正在检查是否有有效的状态代码，如果没有，我们将抛出自定义错误。如果没有，我们映射我们的数据准备解码。
5.  这里我们使用内置的 [decode](https://developer.apple.com/documentation/combine/publishers/merge/3208943-decode) 方法来尝试使用 JSONDecoder 解码我们的自定义类型。类似于上面的 tryMap 函数，任何错误都被推送到下游，以便稍后处理。
6.  拼图的最后一块是使用[式擦除](https://pyartez.github.io/swift/apdvanced/protocols/generics/what-is-type-erasure.html)。这将删除 publisher 类类型，并使其成为 AnyPublisher。关于类型擦除的更多信息，请参见我的[上一篇文章](https://pyartez.github.io/swift/apdvanced/protocols/generics/what-is-type-erasure.html)

# 联合行动

现在我们已经构建了我们的包装类，让我们来看看实际情况:

1.  这里我们调用了新创建的 dataTaskPublisher 方法，该方法返回了我们的 Publisher。这就是反应式编程的用武之地。dataTaskPublisher 中的所有代码尚未执行。我们只是返回了一个出版商，他正在等待一个订阅者的到来和收听。除非订阅尚未完成，否则发布服务器不会执行。为了订阅一个流，我们使用 sink 方法。如果您认为反应式方法链流入底部的水槽，这是这里最好的类比。
2.  sink 方法有两个部分。第一个闭包定义了流完成后会发生什么。现在，这可以以完成状态的形式出现，这意味着流已经完成了正在做的事情，将不再发出任何事件。或者 failure，这意味着上游的某个项目引发了一个错误，该错误会向下流到这个接收器中，在那里可以对其进行处理。
3.  第二个闭包定义了每次事件流发出变更时我们想要做的事情。在这种情况下，发布者将在完成加载后发送一个用户数组，这里我们只是打印出用户名。

# 最后

我们学到了什么:

*   我们已经使用了 [QuickType](https://app.quicktype.io) 将 JSON 转换成可编码的结构用于解码。
*   使用[泛型](https://docs.swift.org/swift-book/LanguageGuide/Generics.html)将现有的 URLSession [dataTask](https://developer.apple.com/documentation/foundation/urlsession/1410330-datatask) 方法与我们自己的方法包装在一起，这样我们就可以使用任何[可编码](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)类型来解码响应。
*   同样，使用反应式编程和苹果新的[组合](https://developer.apple.com/documentation/combine)框架为现有的 [dataTaskPublisher](https://developer.apple.com/documentation/foundation/urlsession/3329708-datataskpublisher) 函数创建了我们自己的通用包装器。

请随意[下载游乐场](https://github.com/pyartez/blog-samples)并自己玩玩这些例子

*原载于 2020 年 6 月 11 日*[*https://pyartez . github . io*](https://pyartez.github.io/networking/simple-json-decoder-in-swift-and-combine.html)*。*