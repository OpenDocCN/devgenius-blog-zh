# 你所需要的就是用灵药启动 Youtube 上传

> 原文：<https://blog.devgenius.io/all-you-need-to-kickstart-youtube-upload-with-elixir-25dd62127033?source=collection_archive---------0----------------------->

## 找出帮助你上传**仙丹**的秘密和偷偷摸摸的 bug。

![](img/9352a73e515ee1707a56b1b1c376c008.png)

照片由[克里斯蒂安·威迪格](https://unsplash.com/@christianw?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 多次上传

您是否遇到过视频上传问题？我有。于是想到了这个项目来解决。我选择了长生不老药来解决这个问题，并开始了。

# 先获得授权！

你需要授权才能让它工作。使用控制器重定向到所需的验证端点。

```
# auth.ex
**defmodule** **GoogleServices**.**Auth** **do**
 **use** **OAuth2**.**Strategy**
  **require** **Logger**

  @redirect_url  "http://localhost:4000/auth/callback"
  @auth_site "https://accounts.google.com"
  @auth_url "/o/oauth2/auth"
  @token_url "https://oauth2.googleapis.com/token"

  # Public API
  **def** client **do**
 **OAuth2**.**Client**.new([
      strategy: __MODULE__,
      client_id: **Application**.get_env(:videoupload, :google_auth_client_id),
      client_secret: **Application**.get_env(:videoupload, :google_auth_client_secret),
      redirect_uri: @redirect_url,
      site: @auth_site,
      authorize_url: @auth_url,
      token_url:  @token_url
    ])
  **end**...
```

重定向到 Google Auth URL，然后用回调来处理它。

```
# auth_controller.ex...**def** youtube(conn, _params) **do**
redirect conn, external:
     **GoogleServices**.**Auth**.authorize_url!(scope: @yt_scope_url)
**end
...**
```

一个函数应该解析回调代码。TokenStore 是存储令牌的 GenServer，并在请求时提供令牌。

```
# auth_controller.ex**def** callback(conn, %{"code" => code, "scope"=> scope}) **do**
# Exchange an auth code for an access token
      # Extract token to separate worker and call to get it

      api_scope = **String**.split(scope, "/")
      |> **List**.last

      **Logger**.info("###Api Scope #{api_scope} ###")

      oauth_client = **GoogleServices**.**Auth**.get_token!(code: code)
      **GenServer**.cast(**TokenStore**, {:set, api_scope, oauth_client.token.access_token})

      conn |> redirect(to: "/")
    **end**
```

# 以后再联系！

对于 YouTube 来说，这种联系是至关重要的，为此，我们将有一个单独的模块。它将使用**特斯拉**和扩展它的基本网址和标题，所以它可以轻松地联系 API。创建连接将需要令牌，这将在后面讨论。

```
**defmodule** **YouTubeData**.**Connection** **do**
@moduledoc """
  Handle Tesla connections for YouTubeData.
  """ **use** **Tesla**# Add any middleware here (authentication)
  plug **Tesla**.**Middleware**.**BaseUrl**,  "https://www.googleapis.com/upload/youtube/v3"
  plug **Tesla**.**Middleware**.**Headers**, %{"User-Agent" => "Elixir"}
  plug **Tesla**.**Middleware**.**EncodeJson**
```

# 立即开始上传！

下面列出了 YouTube 上传鲜为人知的秘密。您将拥有来自`Connection`模块的`new/1`方法，该方法将创建正确的连接。同样重要的是关键字列表中的`uploadType`，它将作为查询参数被附加。所有的逻辑都可以放在 facade 或服务中，并且可以通过前端访问。

```
 # Create your client with new token
        ytClient = new(token) # Add some metadata
        snippet = %**YouTubeData**.**Model**.**VideoSnippet**{
          title: "Dummy",
          categoryId: **22**,
          description: "Dummy"
        }

        status = %**YouTubeData**.**Model**.**VideoStatus**{
          privacyStatus: "private"
        }

        video = %**YouTubeData**.**Model**.**Video**{
          snippet: snippet,
          status: status
        }

        # Important step to follow!
        keyword_list = [{:uploadType, "multipart"}]

        **case** youtube_videos_insert(ytClient, filename, video, "snippet,status", keyword_list) **do ...**
```

# 注意这个重要的步骤！

下面列出了创建请求的秘密顺序，注意请求参数的名称。`resource`将包含元数据，`media`将包含`multipart/form-data`。`part`很重要，因为它保存了发送给 Google 服务的内容和返回的内容。

```
**def** youtube_videos_insert(connection,file, video, part, opts \\ []) **do**
optional_params = %{
      :"alt" => :query,
      :"fields" => :query,
      :"key" => :query,
      :"oauth_token" => :query,
      :"prettyPrint" => :query,
      :"quotaUser" => :query,
      :"userIp" => :query,
      :"autoLevels" => :query,
      :"body" => :body,
      :"notifySubscribers" => :query,
      :"onBehalfOfContentOwner" => :query,
      :"onBehalfOfContentOwnerChannel" => :query,
      :"stabilize" => :query,
      :"uploadType" => :query
    }
    %{}
    |> method(:post)
    |> url("/videos")
    |> add_param(:body, :"resource", video)
    |> add_param(:file, :"media", file)
    |> add_param(:query, :"part", part)
    |> add_optional_params(optional_params, opts)
    |> **Enum**.into([])
    |> (&**Connection**.request(connection, &1)).()
    |> decode(**YouTubeData**.**Model**.**Video**)
  **end**
```

# 我发现了 4 个错误，这样你就不用再去找了

当你试图独自上传视频时，你会遇到很多错误。这里列出了一些。

`mediaBodyRequired`如果在请求的开头没有包含带有`:file` atom 的媒体部分，就会出现错误。

`unknownPart`错误信息告诉您，您没有像上面添加的那样将零件参数添加到查询中。

`unexpectedPart`如果发`snippet,status`以外的东西。

`authorizationRequired`您的令牌已过期，请手动刷新或使用刷新令牌刷新。

![](img/5226f2c2284d62add0493075054c595c.png)

&clap_if_you_want/0