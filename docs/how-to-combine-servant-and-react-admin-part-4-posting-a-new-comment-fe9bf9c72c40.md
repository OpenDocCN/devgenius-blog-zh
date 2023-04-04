# 如何组合 Servant 和 React Admin，第 4 部分:发布新评论

> 原文：<https://blog.devgenius.io/how-to-combine-servant-and-react-admin-part-4-posting-a-new-comment-fe9bf9c72c40?source=collection_archive---------39----------------------->

![](img/101b8ac26b9335afd1b2a31775831635.png)

邮箱，信用:克里斯蒂娜·特里普科维奇

# 概观

[继续我们的上一篇博客](https://propellant.tech/blogs/servant-and-react-admin-3/)，我们将向 API 添加一个 POST 方法，并从 React 使用它。我们将通过使用`ReaderT`单子来跟踪添加的注释。

# API 类型

让我们用 POST 方法扩展 API 类型:

```
type WebApi
   = "comments" :> Get '[ JSON] (ListHeaders [Comment]) 
   :<|> "comments" :> Capture "id" Int :> Get '[ JSON] Comment
   :<|> "comments" :> ReqBody '[ JSON] NewComment :> Post '[JSON] NoContent
```

没什么特别的，也许除了`NewComment`型。此外，这里的新东西是我们通过使用`ReqBody`来表达`NewComment`应该作为 JSON 类型出现在请求的主体中。帖子不会返回任何内容。这可能看起来很奇怪，当没有内容被返回时，你必须以`JSON`的形式指定内容类型。背后的原因在这里[解释](https://github.com/haskell-servant/servant/issues/498)，但基本上归结为没有内容的特殊情况。

`NewComment`型与`Comment`型除 id 外基本相同。因为在我们的方法中，服务器是分发 id 的服务器，所以我们不能让 Web UI 指定它，所以它永远不会出现在发布的`comment`中。`NewComment`相当直白地这样指定:

```
{-# LANGUAGE DeriveGeneric #-}

module NewComment where

import           Data.Aeson.Types           (FromJSON, ToJSON)
import           GHC.Generics               (Generic)

newtype NewComment
  = NewComment {content :: String}
  deriving (Generic, Show)

instance ToJSON NewComment

instance FromJSON NewComment
```

注意，我们可以看到这里有重复。这样定义`Comment`会更好:

```
data Comment =
  Comment
    { id     :: Int
    , payload :: NewComment
    }
```

这将避免`Comment`和`NewComment`之间的重复。然而，这给了我们另一个问题。当我们只是派生出像这样定义的`Comment`的`JSON`实现时，它会创建一个嵌套的 JSON，结构如下:

```
{
 "id" : 42,
  "payload" : {
    "comment" : "this is my comment"
  }
}
```

React Admin 并不期望 JSON 以这种方式提供服务。我们可以通过自己实现`toJSON`和`fromJSON`来解决这个问题，而不是派生它，但是为了这个博客的目的，我们认为引入一个新的类型并吸收一点重复是很好的。

# 里德特

使用 ReaderT monad 的想法是我第一次在 Michael Snoyman 的博客[这里](https://tech.fpcomplete.com/blog/2017/06/readert-design-pattern)上遇到的。这是一本列举利弊的好书。

因此，让我们尝试向运行在 ReaderT monad 中的 API 类型添加一个 POST 方法。首先，我们需要添加变形金刚包

```
- transformers == 0.5.6.2
```

因为里德特住在那里。

接下来，我们将声明我们的状态并将其包装在 ReaderT monad 中，如 Snoyman 文章中所解释的:

```
newtype State =
  State
    { comments :: TVar [Comment]
    }

type AppM = ReaderT State Handler
```

我们基本上扩展了处理程序 monad 的功能，也可以访问`State`，State 包含一个可变的注释列表，受多线程保护。注意，在 Handler 上添加不同的 monad 对 web API 类型没有影响，这是应该的。

# 使用我们的 AppM 单子

目前，我们这样定义我们的服务器类型:

```
server :: Server WebApi
server = listComments :<|> getComment
```

服务器在这里是简写为:

```
type Server api = ServerT api Handler
```

因此，当我们使用服务器类型时，我们选择使用默认的处理程序类型，这当然是有意义的。如果我们想选择自己的 monad 来运行它，我们需要使用 ServerT 来指定它，如下所示:

```
server :: ServerT WebApi AppM
server = listComments :<|> getComment
```

当我们试图编译它时，编译器不会特别高兴，因为我们在这里使用了服务器函数:

```
app :: Application
app = corsConfig $ serve api server
```

它是这样定义的:

```
serve :: (HasServer api '[]) => Proxy api -> Server api -> Application
serve p = serveWithContext p EmptyContext
```

如你所见，这里的代码也引用了`Server api`，默认情况下，它让我们在`Handler`单子中运行。

# 抬起来

浏览文档时，我们注意到这个 [hoistServer](https://hackage.haskell.org/package/servant-server-0.17/docs/Servant-Server.html#v:hoistServer) 函数:

```
HasServer api '[] => Proxy api -> (forall x. m x -> n x) -> ServerT api m -> ServerT api n
```

类型签名看起来有点令人生畏，但是我们可以看到，如果我们可以定义一个从我们的`AppM`单子到`Handler`单子的转换(因此是一个函数`AppM a -> Handler a`，我们可以对它调用`hoistServer`，然后再次使用`serve`来创建一个`Application`，然后我们又回到了正轨。

所以，我们的下一个挑战是为每个 a 创建一个接受一个`AppM a`并从中生成一个`Handler a`的函数。

这可能不会立即显而易见，但我们可以为此使用 [runReaderT](https://hackage.haskell.org/package/transformers-0.5.6.2/docs/Control-Monad-Trans-Reader.html#t:ReaderT) 。回忆:

```
type AppM = ReaderT State Handler
```

以及`runReaderT`的定义，这是一个为实现 ReaderT 类型类的任何东西定义的函数(我们的 AppM 就是这样做的`AppM`):

```
runReaderT :: r -> m a
```

其中 r 是我们的状态，m 是处理程序单子，我们非常接近一个将我们的`AppM a`转换为`Handler a`的函数。

如果我们这样定义我们的运行状态:

```
runWithState :: State -> AppM a -> Handler a
runWithState state appM = runReaderT appM state
```

一旦我们提供了一个初始状态，我们就有了转换函数。

我们的服务器定义是这样的:

```
app :: Application
app = corsConfig $ serve api server
```

收件人:

```
app :: Application
app = corsConfig $ serve api $ hoistServer api (runWithState undefined) serve
```

我们的编译器又会开心了。当然，我们通过使用`undefined`函数做了一点手脚，但是我觉得分两步做更好，这样你可以更清楚地看到转换的发生，你马上就会明白为什么了。

因此，为了让它工作，我们需要在应用程序中传递一个初始状态。没什么大不了的，你只需要使用[事务变量](http://hackage.haskell.org/package/stm-2.5.0.0/docs/Control-Concurrent-STM-TVar.html)函数:

```
createInitialState :: IO State
createInitialState = do
  initalComments <- atomically $ newTVar fixedComments
  return $ State {comments = initalComments}
```

为此，您需要在 IO monad 中。幸运的是，应用程序的类型是应用程序，它是这样定义的:

```
type Application = Request -> (Response -> IO ResponseReceived) -> IO ResponseReceived
```

所以我们在木卫一单子上运行。

如果我们天真地像这样实现 app 函数:

```
app :: Application
app = do
  initialState <- createInitialState
  corsConfig $ serve api (hoistServer api (runWithState initialState) server)
```

我们会让编译器向我们抱怨:

```
* Couldn't match type 'IO b0' with 'Network.Wai.Internal.Request -> (Network.Wai.Internal.Response -> IO Network.Wai.Internal.ResponseReceived) -> IO Network.Wai.Internal.ResponseReceived' Expected type: Application Actual type: IO b0
```

这是因为我们认为我们在运行`IO`单子，但我们没有，因为`Application`是一个函数的简写。

如果我们这样定义 app 函数:

```
app :: Application app req res = do initialState <- createInitialState corsConfig (serve api (hoistServer api (runWithState initialState) server)) req res
```

编译器又会高兴了。

我们可以这样定义它，使它更加明确:

```
createApplicationWithoutMiddleware :: State -> Application
createApplicationWithoutMiddleware initialState = serve api (hoistServer api (runWithState initialState) server)

app :: Application
app req res = do
  initialState <- createInitialState
  corsConfig (createApplicationWithoutMiddleware initialState) req res
```

`corsConfig`函数的类型为`Middleware`，定义如下:

```
type Middleware = Application -> Application
```

因此，它接受一个应用程序，并返回给我们一个新的，与 CORS 的东西添加。然后，我们仍然必须应用我们匹配的请求元素和响应处理程序，并且我们的签名是正确的。

Pffw，我们还没到那一步。首先让我们的`listComments`和`getComment`函数在 AppM 单子中运行，因为它们仍然在`Handler`单子中运行。

# 将 listComments 和 getComment 函数转换为 AppM monad

回想一下我们对 getComment 的定义是这样的:

```
getComment :: Int -> AppM Comment
getComment requestedId = 
  let maybeComment = L.find (\comment -> Comment.id comment == requestedId) fixedComments
   in maybe (throwError err404 {errBody = BSLazy.pack $ "Could not retrieve comment with id " ++ show requestedId}) return maybeComment
```

为了使用 TVar 中的状态，我们需要将其重构为:

```
getComment :: Int -> AppM Comment
getComment requestedId = do
  commentsTransactionVar <- asks comments
  comments <- liftIO $ readTVarIO commentsTransactionVar
  let maybeComment = L.find (\comment -> Comment.id comment == requestedId) comments
   in maybe (throwError err404 {errBody = BSLazy.pack $ "Could not retrieve comment with id " ++ show requestedId}) return maybeComment
```

很简单，我们使用 [asks](https://hackage.haskell.org/package/transformers-0.5.6.2/docs/Control-Monad-Trans-Reader.html#v:asks) 函数返回 TVar。commentsTransactionVar 的类型现在是:

```
TVar [Comment]
```

现在我们利用`readTVarIO`函数返回`comments`，然后我们可以像以前一样继续。`readTVarIO`函数在`IO`中运行，所以我们需要提升它(因为我们的`AppM`单子比`IO`多)。

`listComments`功能非常相似:

```
listComments :: AppM (ListHeaders [Comment])
listComments = do
  commentsTransactionVar <- asks comments
  comments <- liftIO $ readTVarIO commentsTransactionVar
  return $ addHeader (List.length comments) comments
```

好了，现在我们终于准备好实现我们的`insertComment`函数了。

# 让我们最后实现 insertComment

你可以看到我们在`listComments`和`getComment`中使用 TVar 的方式，你可能会怀疑我们会以同样的方式使用它。问题是，如果我们首先原子地获取注释，更新它们，然后原子地写回它们，我们无法保证在我们的读和写之间，其他人没有执行写操作并使我们的操作无效。因此，我们将利用`STM`并像这样自动运行整个操作:

```
insertComment :: NewComment -> AppM NoContent
insertComment newComment = do
  commentsTransactionVar <- asks comments
  liftIO $ atomically $ do
    comments <- readTVar commentsTransactionVar
    let nextId = List.length comments + 1
    let comment = fromNewComment nextId newComment
    let updateComments = List.insert comment comments
    writeTVar commentsTransactionVar updateComments
  return NoContent
```

我们计算`nextId`的方法有点笨拙，一旦我们允许删除，就会给我们带来麻烦。但是让我们假装我们现在不会那样做。

让我们稍微关注一下我们在这里做的事情，我们读取注释，创建一个新的注释，插入它并更新事务性变量，所有这些都是自动完成的。原子性取决于 Haskell 中的 [STM 实现。你可以把它想象成某种乐观锁，但是在数据库之外，只是在内存中。原子函数将重试，直到成功。如果有人在我们读取和写入 TVar 之间写了 TVar，它可能会失败，但它会重试。因此，重要的是不要在原子块中产生副作用，因为它们可能会被执行多次，以防我们的代码片段需要重试。通常不可能产生副作用，因为 STM 没有实现 MonadIO。你当然可以用 unsafeIO still 搬起石头砸自己的脚，但最好不要这样做。](http://hackage.haskell.org/package/stm-2.5.0.0/docs/Control-Monad-STM.html)

请注意，如果我们像这样实现我们的代码片段:

```
insertCommentNotSoSafe :: NewComment -> AppM NoContent
insertCommentNotSoSafe newComment = do
  commentsTransactionVar <- asks comments
  comments <- liftIO $ readTVarIO commentsTransactionVar
  let nextId = List.length comments + 1
  let comment = fromNewComment nextId newComment
  let updateComments = List.insert comment comments
  liftIO $ atomically $ writeTVar commentsTransactionVar updateComments
  return NoContent
```

我们仍然不会安全。我们可以有另一个 insert，例如在 TVar 中看到两个元素，在我们之前添加一个 id 为 3 的新元素，没有什么可以阻止我们插入 id 为 3 的第二个注释。

注意，这不是处理 STM 的理想方式，我们将在下一篇博客中研究[state var](http://hackage.haskell.org/package/stm-2.5.0.0/docs/Control-Concurrent-STM-TVar.html#v:stateTVar)来更优雅地表达这一点。看到幕后发生的事情总是令人愉快的，这也是我们目前保留这个实现的原因。

好了，我们终于更新了服务器，可以接收新的评论了，让我们回到 React-Admin。

# 反应-管理和发布

我们将像这样创建我们的创建组件:

```
export const CommentsCreate = props => (
    <Create {...props}>
        <SimpleForm>
            <TextInput source="content" validate={required()}/>
        </SimpleForm>
    </Create>
)
```

我们只对`comment`的内容感兴趣，而`id`是由后端的 REST 服务器发出的。当然，`content`是必填字段，因此有了`validation-required`属性。我们可以像这样轻松地使用`create`组件:

```
const App = () => (
    <Admin dataProvider={dataProvider}>
        <Resource name="comments" list={CommentsList} show={CommentsShow} create={CommentsCreate}/>
    </Admin>
);
```

将该组件添加到`Admin`组件的`create`属性后，我们将在 UI 中看到一个创建链接:

![](img/f5df36f8977565ba5887dce062a2c5e7.png)

当我们点击它时，我们的`Create-component`被触发，我们可以填写新的`comments`，但是当我们试图保存时，我们将再次得到我们的网络错误。检查控制台证实了这一点，我们需要处理更多的 CORS 的东西。

# 重访 CORS

要知道哪个请求被发起和拒绝，我们需要在浏览器中打开开发者工具上的网络选项卡，并尝试再次添加一个`comment`。然后，我们将看到正在启动的请求是一个选项请求。这是 CORS 规范的一部分，对于每个帖子，我们首先验证是否允许我们发布，这是通过一个`options`请求来完成的。那么，让我们回到我们的服务器，展开 CORS 配置。

允许的默认方法是 GET、HEAD 和 POST。选项不在其中，所以我们应该添加它。当我们检查 CORS 记录时，我们看到这些方法的类型是:

```
corsMethods ∷ ![HTTP.Method]
```

`HTTP`是 http-types 包中的一个模块，我们必须先将它添加到我们的 package.yaml 中:

```
- http-types == 0.12.3
```

然后我们可以导入这个包，在这个例子中被限定为`HttpTypes`，并将正确的方法添加到我们的 CORS 记录中:

```
corsConfig :: Middleware
corsConfig = cors (const $ Just policy)
  where
    policy =
      simpleCorsResourcePolicy
        { corsExposedHeaders = Just ["X-Total-Count"],
          corsMethods = [HttpTypes.methodGet, HttpTypes.methodPost, HttpTypes.methodHead, HttpTypes.methodOptions]
        }
```

请记住，我们必须在这里添加默认的方法，并且在覆盖完整列表时覆盖`simpleCorsResourcePolicy`的内容。加完这些方法，我们再来检查一下！

嗯，还是老样子。也许我们现在应该检查错误响应。如果我们这样做，我们会看到这一点:

```
HTTP header requested in Access-Control-Request-Headers of CORS request is not supported; requested: content-type; supported are Accept, Accept-Language, Content-Language.
```

(注意，有时响应错误仅在您尝试在开发人员选项的网络选项卡中重新发送请求时显示。似乎是火狐的怪癖)

根据 [Mozilla cors 文章](https://developer.mozilla.org/nl/docs/Web/HTTP/CORS)中对内容类型的简单 CORS 描述(与我们之前分享的内容相同)，如果内容类型是以下内容之一，则应该被允许:

```
The only allowed values for the Content-Type header are: application/x-www-form-urlencoded multipart/form-data text/plain
```

当我们检查请求时，我们看到标题的值为 text/html。好了，我们现在可以选择是为内容类型添加一个总括接受还是对其进行提炼。为了简单起见，我们将对 content-type 头进行一揽子接受。我们的 CORS 配置变成了:

```
import qualified Network.HTTP.Types as HttpTypes
import Network.Wai (Middleware)
import Network.Wai.Middleware.Cors
  ( cors,
    corsExposedHeaders,
    corsMethods,
    corsRequestHeaders,
    simpleCorsResourcePolicy,
  )

corsConfig :: Middleware
corsConfig = cors (const $ Just policy)
  where
    policy =
      simpleCorsResourcePolicy
        { corsExposedHeaders = Just ["X-Total-Count"],
          corsMethods = [HttpTypes.methodGet, HttpTypes.methodPost, HttpTypes.methodHead, HttpTypes.methodOptions],
          corsRequestHeaders = [HttpTypes.hContentType]
        }
```

好吧，如果我们现在再试一次，它必须工作，不是吗，我们填写我们的内容，并按下保存按钮，看到…另一个错误。json 没有被定义。

# 返回新创建的评论

这有点难，但这是 React 告诉我们它希望我们的响应包含 JSON 类型的主体的方式。这与 React 如何处理我们的响应的内部机制有关，但是它告诉我们它想要一个 JSON 响应体，而不仅仅是我们指定的`NoContent`。哦，是的，记住，我们的 POST 方法不返回任何内容。在某种程度上，它不需要，但是 React-admin 希望新创建的`comment`作为 JSON 返回。回到我们的 REST 服务器，这意味着一些变化。我们的帖子资源类型需要更改以返回`comment`。

```
"comments" :> ReqBody '[JSON] NewComment :> Post '[JSON] Comment
```

我们的实现还需要返回`comment`:

```
insertComment :: NewComment -> AppM Comment
insertComment newComment = do
  commentsTransactionVar <- asks comments
  liftIO $ atomically $ do
    comments <- readTVar commentsTransactionVar
    let nextId = List.length comments + 1
    let comment = fromNewComment nextId newComment
    let updateComments = List.insert comment comments
    writeTVar commentsTransactionVar updateComments
    return comment
```

当我们现在尝试它时，我们将看到插入过程工作正常，但是当我们尝试检索 id 3 时，它似乎不存在。奇怪。事实上，无论我们做什么，唯一得到回报的是我们的初始状态。

哦该死的。回想一下，我们提到过初始化不应该在`Application`类型内部完成，因为`Application`是为每个请求运行的。确实如此。如果我们给 api 函数添加一个`print`语句:

```
app :: Application
app req res = do
  print "create initial state"
  initialState <- createInitialState
  corsConfig (createApplicationWithoutMiddleware initialState) req res
```

我们会注意到，这个 print 语句出现在每个请求中。愚蠢的我们。我们尽一切努力更新我们内部状态，然后每次都用基本状态覆盖它。好吧，让我们来解决这个问题:

```
createInitialState :: IO State
createInitialState = do
  initalComments <- atomically $ newTVar fixedComments
  return $ State {comments = initalComments}app :: ServerT WebApi Handler -> Application
app initedServer =
  corsConfig $ serve api initedServerstart :: Int -> IO ()
start port = do
  initialState <- createInitialState
  let initedServer = hoistServer api (runWithState initialState) server
  run port $ app initedServer
```

现在我们在调用 run 函数之前初始化服务器。为每个请求运行我们的`app`的是`run`函数。如果我们现在添加一个`comment`，我们将不再看到错误，我们将看到我们新创建的注释到达。

# 结论

好吧，我们成功了。这是一项重要的工作，经过了组成我们应用程序的所有层，必须学习关于 Servant、CORS 和 React-Admin 的新知识。我们将在下一篇博客中继续做一些更简单的事情，删除一些东西。这个博客的代码可以在[这里](https://gitlab.com/KasperJanssens/servant-blog/-/tags/v0.3)找到。

*最初发布于*[*https://propellant . tech*](https://propellant.tech/blogs/servant-and-react-admin-4/)*。*