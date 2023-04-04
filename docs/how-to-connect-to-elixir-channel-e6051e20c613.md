# 如何连接到仙丹频道

> 原文：<https://blog.devgenius.io/how-to-connect-to-elixir-channel-e6051e20c613?source=collection_archive---------1----------------------->

## 良好的沟通和黑咖啡一样刺激，也一样辛苦。

![](img/0c4e4a4039fe85a205695b8b2cf63923.png)

由[打磨机 Mathlener](https://unsplash.com/@sandermathlener?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的一些通道照片

## 建立前端连接

`socket.js`被生成来创建一个到套接字的连接，它还导出要使用的连接。

```
// channel creationlet channel = socket.channel("upload_state:all", {})// channel joiningchannel.join()
  .receive("ok", data => {
    console.log("Joined topic", "upload_state:all" )
  })
  .receive("error", resp => {
    console.log("Unable to join topic", "upload_state:all")
  })// on channel event do something
channel.on("change", payload => {
```

`channel.js`文件应包含连接到通道的逻辑。建立到通道的连接后，您调用通道上的 join 方法。我们将监听的事件称为`change` ，应该在`upload_state:all`通道上触发。您需要将这个`channel.js`包含到您的 app.js 文件中。

## 安全性

您可以使用用户 id 作为生成令牌的手段。我正在使用 **Pow** 依赖来处理用户，我对此很满意。我添加了插件来处理令牌生成。第一个插件将用户 id 附加到连接上，下一个插件生成令牌。

```
# router.ex
pipeline :browser do
    ...
    plug :put_user_id
    plug :put_token
    ...
end

defp put_user_id(conn, _headers) do
  %{id: id} = conn
  |> Pow.Plug.current_user
  |> extract_user_id
  assign(conn, :user_id, id)
end

  defp put_token(conn, _headers) do
    id = conn.assigns.user_id
    first_token = Phoenix.Token.sign(SwaggUploadWeb.Endpoint, "salt", id)
    assign(conn, :token, first_token)
  end
```

现在我们可以从`conn`中提取令牌和用户 id，并将其附加到`window`对象上。

```
<!-- app.html.eex -->
**<script>**window.userToken = *"<%= assigns[:token] %>"*;**</script><script>**window.userId = *"<%= assigns[:user_id] %>"*;**</script>**
```

在我们将它附加到`window`之后，我们需要在`socket.js`中使用它

```
// socket.jslet socket = new Socket("/socket", {params: {token: window.userToken, user_id: window.userId}})
```

我们使用具有相同盐析的验证方法，在通过之后，我们得到具有`:ok`原子的元组。

```
# user_socket.ex
**def** **connect**(%{"token" => token, "user_id" => user_id}, socket, _connect_info) do
    case Phoenix.Token.verify(SwaggUploadWeb.Endpoint, "salt", token, max_age: **86400**) do
      {:ok, _} ->
        {:ok, assign(socket, :user_id, user_id) }
      _ ->
        :error
    end
  end
```

## 处理酏剂通道

混合工具具有特定任务，以便创建通道。它给出了如何设置频道的基本说明。您需要将您的通道添加到 socket。

```
# user_
channel "upload_state:*", SwaggUploadWeb.UploadStateChannel# your_channel.ex
YourWebAppModule.Endpoint.broadcast("upload_state:all", "change", state)
```

上述第二种方法通过更改事件向通道广播消息。为了处理过滤，我添加了拦截。所有用户都将收到特定于其活动的消息。我们可以从套接字结构中提取用户 id，并使用它来与有效负载 id 进行比较，在它们匹配之后，我们可以推送有效负载。

```
intercept [*"change"*]
  **def** handle_out(*"change"*, payload, socket) do
    %{id: id} = payload
    case (Integer.parse(socket.assigns[:user_id])) do
      ^id ->
         push socket, *"change"*, payload
         {:noreply, socket}
      _ ->
         {:noreply, socket}
    end
  end
```

**感谢您的阅读！**