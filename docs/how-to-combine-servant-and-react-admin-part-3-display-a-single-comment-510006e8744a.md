# 如何组合 Servant 和 React Admin，第 3 部分:显示单个注释

> 原文：<https://blog.devgenius.io/how-to-combine-servant-and-react-admin-part-3-display-a-single-comment-510006e8744a?source=collection_archive---------39----------------------->

![](img/7bcd7534937338173411a6c6a38511e1.png)

# 概观

我们的[上一篇博客](https://propellant.tech/blogs/servant-and-react-admin-2/)以一个非常简单的 REST 服务器和一个可以列出评论的 UI 结束，就是这样。让我们扩展一下，用 GET 请求获取单个注释！

# 在仆人中获取资源

GET 请求本身相当接近我们已经拥有的内容。让我们回忆一下我们目前的类型:

```
type WebApi = "comments" :> Get '[ JSON] (ListHeaders [Comment])
```

我们将首先增加第二条休息路线。其次，GET 请求必须指定一个它想要检索的单个`comment` ,因此它需要某种惟一的 id。同样，如果找不到那个`comment` ，我们应该能够指示失败。

在仆人端，它看起来像这样:

```
type WebApi
   = "comments" :> Get '[ JSON] (ListHeaders [Comment]) 
   :<|> "comments" :> Capture "id" Int :> Get '[ JSON] Comment
```

这里已经有一些东西要打开了。尽管我们的意图可能相当清楚，但还是有一些新的因素。

最引人注目的是工会运营商`:<|>`。它表示我们定义的两个路由应该集中在一个 API 类型中。您可以将其视为类型级别的`( ,)`-操作符。事实上，当你忘记实现 API 的一部分时，你会看到一个类似嵌套元组结构的编译错误，试图告诉你哪里做错了。

下一个新事物是`Capture` 型。`Capture` 尝试将 url 的一个子路由解析成您指定的类型，在本例中是一个`Int`。这里的“id”字符串只是提供信息，可以在为 Servant Api 类型生成文档时使用。我们稍后会谈到这一点。

# 实现服务器

如果我们现在尝试编译，我们会看到以下错误:

```
* Couldn't match type 'Handler (ListHeaders [Comment])' with 'Handler (Headers ' [TotalCountHeader] [Comment]) :<|> (Int -> Handler Comment)' Expected type: Server WebApi Actual type: Handler (ListHeaders [Comment])
```

您已经可以看到 union 运算符的效果。当我们增加第三条路线时，它将变得更加直言不讳。这个错误有效地告诉我们，请通过在`Handler Monad`中提供从`Int` 到`Comment`的函数来实现服务器。

请注意，您不再需要解析捕获的子路由。

所以，我们开始吧:

```
module Server where

import Servant
import ApiType
import Network.Wai.Handler.Warp (run)
import Comment
import Cors
import qualified Data.List as L
import Data.Maybe (fromJust)

fixedComments :: [Comment]
fixedComments = [Comment 1 "A comment", Comment 2 "Another comment"]

listComments :: Handler (ListHeaders [Comment])
listComments = return $ addHeader 2 fixedComments

getComment :: Int -> Handler Comment
getComment requestedId = 
  let maybeComment = L.find (\comment -> Comment.id comment == requestedId) fixedComments in
  return $ fromJust maybeComment

server ::  Server WebApi
server = listComments :<|> getComment
```

我们改变了很多东西。

首先，我们提取了变量`fixedComments`中的所有注释，因此它既可以在`listComments`中使用，也可以在`getComment`中使用。

其次，有一个实现 get 功能的`getComment`函数。它获取一个 id，查找它，如果没有找到它，`fromJust`函数将抛出一个异常。

最有趣的部分是，您看到我们的 union 操作符在`server`方法中返回。

请注意，如果您没有传递一个可以被解析为`Int`的“key ”, Servant 将已经用一个 400 进行了回答，并且您不再需要在`getComment` 函数中处理那个错误，让我们来演示一下:

```
curl -i http://localhost:8082/comments/asdf HTTP/1.1 400 Bad Request Transfer-Encoding: chunked Date: Sat, 30 May 2020 09:46:36 GMT Server: Warp/3.3.10
```

## union 运算符

上面的服务器片段归结为实现我们用 union 操作符定义的服务器 api，方法是对具有正确签名(对应于我们在定义中使用的签名)的函数应用相同的 union 操作符。

我认为值得注意的是，我们用来创建 API 类型的同一个操作符，在类型级别上，也可以用在函数上，但是我们不是定义服务器类型，而是提供函数来实现它。

# 错误处理

目前，我们将返回 500 内部服务器错误，以防 GET 请求无法检索我们的`comment`。这是因为我们应用的`fromJust`函数将抛出一个异常，这个异常将被服务器循环转换成一个 500。

这当然远非最佳，但是让我们回忆一下本系列的第一篇博客[中的处理程序 monad 可以用来执行 IO 以及返回 ServantError。](https://propellant.tech/blogs/servant-and-react-admin-1/)

最简单的方法是使用 servant-server 库中预制的错误类型，并通过使用 MonadError 中的 [throwError](http://hackage.haskell.org/package/mtl-2.2.2/docs/Control-Monad-Error.html#v:throwError) 从 getComment 函数中返回这些错误类型。我们这样做的唯一原因是，处理程序 monad 实现了这个类型类。

我们像这样改变我们的`getComment` 函数:

```
getComment :: Int -> Handler Comment
getComment requestedId = 
  let maybeComment = L.find (\comment -> Comment.id comment == requestedId) fixedComments in
  maybe (throwError err404) return maybeComment
```

[也许](http://hackage.haskell.org/package/base-4.14.0.0/docs/Data-Maybe.html#v:maybe)函数不需要太多解释，如果存在它将返回`comment` 或者如果不存在返回错误类型。

现在，这些`errXXX` 函数都返回非常标准的响应，例如`err404`实现是这样的:

```
err404 :: ServerError
err404 = ServerError { errHTTPCode = 404
                    , errReasonPhrase = "Not Found"
                    , errBody = ""
                    , errHeaders = []
                    }
```

这就达到了目的，但是更好的做法是具体说明还没有发现什么。对于更具描述性的错误，我们将把我们的`getComment`函数改为:

```
getComment :: Int -> Handler Comment
getComment requestedId = 
  let maybeComment = L.find (\comment -> Comment.id comment == requestedId) fixedComments in
  maybe (throwError err404 {errBody = BSLazy.pack $ "Could not retrieve comment with id " ++ show requestedId}) return maybeCommen
```

当然，我们需要`import Data.ByteString.Lazy.Char8 as BSLazy`来实现这一点。如果我们愿意，我们也可以改变原因短语，就像我们编辑 errBody 一样。

注意，`errHeaders`不像 Api 类型中的头那样改变 ServerError 的类型。在这里，它们归结为一组字符串。

让我们再次用旋度来演示:

```
curl -i http://localhost:8082/comments/2 HTTP/1.1 200 OK Transfer-Encoding: chunked Date: Sat, 30 May 2020 09:46:25 GMT Server: Warp/3.3.10 Content-Type: application/json;charset=utf-8 { "content":"Another comment","id":2}
```

并且在不存在 id 的情况下:

```
curl -i http://localhost:8082/comments/3 HTTP/1.1 404 Not Found Transfer-Encoding: chunked Date: Sat, 30 May 2020 09:46:26 GMT Server: Warp/3.3.10 Could not retrieve comment with id 3
```

# 从反应管理员获取

现在让我们回到 React Admin。我们想要显示我们的`comment`，React Admin 有一个方便的特性，它可以根据返回的 JSON 猜测显示组件的布局。

我们将应用组件更改为:

```
const App = () => (
    <Admin dataProvider={dataProvider}>
        <Resource name="comments" list={CommentsList} show={ShowGuesser}/>
    </Admin>
);
```

使用`ShowGuesser` 作为处理单个`comment`的“显示”功能的组件。

该组件不仅会猜测布局，还会在浏览器的控制台中打印出来，因此打开开发人员工具并见证:

```
Guessed Show:

export const CommentShow = props => (
    <Show {...props}>
        <SimpleShowLayout>
            <TextField source="content" />
            <TextField source="id" />
        </SimpleShowLayout>
    </Show>
);
```

这是一个非常实用的方法，可以毫不费力地从一个像样的组件开始。我们可以在事后改变它。目前，这很适合我们，我们现在唯一要改变的是切换文本字段的顺序，首先显示 id。

因此，我们的 React 组件显示我们的`comment` 将如下所示:

```
export const CommentsShow = props => (
    <Show {...props}>
        <SimpleShowLayout>
            <TextField source="id"/>
            <TextField source="content"/>
        </SimpleShowLayout>
    </Show>
);
```

`SimpleShowLayout` 是最常用的节目布局之一。另一个选项是`TabbedShowLayout`，更多信息见 react 管理教程中的[。我们需要使用一个现有的显示布局，以便我们的组件可以在`Resource` 组件的“show”属性中使用。](https://marmelab.com/react-admin/Show.html)

如果我们现在将该属性更新为:

```
<Resource name="comments" list={CommentsList} show={CommentsShow}/>
```

我们将能够点击列表概览中的`comment` 并放大单个`comment`。

这个博客的源代码可以在[这里](https://gitlab.com/KasperJanssens/servant-blog/-/tags/v0.2)找到。

# 结论

好了，这就结束了一个 GET 请求。在下一篇博客中发表。

*最初发布于*[*https://propellant . tech*](https://propellant.tech/blogs/servant-and-react-admin-3/)*。*