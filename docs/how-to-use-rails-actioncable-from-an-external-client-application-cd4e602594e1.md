# 如何从外部客户端应用程序使用 Rails ActionCable

> 原文：<https://blog.devgenius.io/how-to-use-rails-actioncable-from-an-external-client-application-cd4e602594e1?source=collection_archive---------0----------------------->

![](img/411bdbaef9e42e81bf198fdb29192d9a.png)

[翁贝托](https://unsplash.com/@umby?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

> WebSockets 是一种很好的方式，它允许使用持久的双向通道进行客户机-服务器通信，而客户机不必轮询服务器。

在这篇文章**中，我将解释如何从一个通用应用程序**开始使用 Ruby on Rails 中的 websocket。我不会深究什么是 Websocket 或者如何创建 Rails 项目。我就在本文最后留一些链接吧。

## 首先，Rails 中的 WebSocket 被称为 ActionCable。

**问题**:来自[官方文档](https://guides.rubyonrails.org/action_cable_overview.html#parent-channel-setup)或其他类似[的教程本](https://dev.to/nkemjiks/simple-chatroom-with-rails-6-and-actioncable-3bc3)解释了如何配置服务器部分，并展示了使用 *rails/actioncable* 节点模块从 Javascript 前端进行订阅和通信的代码。但是**如果你有一个只有 API 的 Rails 项目或者你有一个除了 JS** 之外的前端，比如一个 Android 或者 iOS 应用程序，那该怎么办？这也是我写这篇文章的原因。

**开始:**假设我们有一个 Rails 项目，您运行下面的命令

它将生成一个名为*<your _ name>_ channel . Rb*的文件，我们可以在其中处理客户端订阅或/和我们可以编写客户端调用的自定义方法……为了保持简单，我们将实现订阅的方法，并从头开始编写一个自定义的哑方法(“my_method”)，该方法接收“data”参数。我们稍后会看到什么是“数据”。在这个例子中，让我们用 ChatChannel 替换< your_name >。这是文件的样子。

## 我们的目标是从 JS 之外的外部客户端或 Rails 项目之外访问“subscribed”和“my_method”方法。开始吧！！！

![](img/59b443adafce06bb00659fe6e457efe9.png)

布拉登·科拉姆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

*第一个问题:如何创建 WebSocket 客户端？或者我们可以使用一个“邮差”一样的客户端已经可用吗？*

有许多方法和库可以为许多平台建立 WebSocket 连接，但是要开始，我建议使用 WebSocket 客户端

*   [http://www.websocket.org/echo.html](http://www.websocket.org/echo.html)
*   [这个 Chrome 插件](https://chrome.google.com/webstore/detail/websocket-test-client/fgponpodhbmadfljofbimhhlengambbn)

*第二个问题:我应该调用哪个 URL？在我的项目路线中，我看不到任何与 ActionCable* 相关的内容

*   首先，我们必须将协议从 http/https 改为 ws。
*   假设我们的 Rails 项目运行在 localhost 端口 3000 上。
*   默认情况下，在没有指定任何其他内容的情况下，Rails 在 */cable* 路径上公开 WebSocket 服务

把这些东西放在一起**URL 是 ws://localhost:3000/cable**

*第三个问题:我们如何沟通？*

**基本上与 WebSocket 协议**规定的 JSON **消息一致。像 *rails/actioncable* JS 模块一样，libraries 提供了一种“SDK 方式”来管理这些消息，但是在我们简单的 web 客户端中，我们只有一个“消息字段”来写入命令，仅此而已。我努力寻找请求的格式，在我写作的时候，我想还有很多需要寻找的东西。为了指引我正确的方向，我找到了这两篇文章( [1](https://medium.com/@emikaijuin/connecting-to-action-cable-without-rails-d39a8aaa52d5) 、 [2](https://thoughtbot.com/blog/talking-to-actioncable-without-rails) )。 *2* 也解释了如何验证用户。为了以简单的方式了解“步骤 0”的工作原理，我将忽略这一步。从 *1* 中，我尝试将 *sendTXT* 方法中的字符串按原样复制/粘贴到 web 客户端消息字段中(即使使用了开始/结束双引号和所有转义字符: *"{\"command\":\"subscribe\ "，\ " identifier \ ":\ " { \ \ \ " channel \ \ \ ":\ \ \ " ArduinoChannel \ \ \ " } \ "*)，但没有成功(我先将名称改为我的通道名称)。我试图删除开始/结束引号，我认为一个问题是转义字符太多。因此，我试图在 Rails 控制台的帮助下序列化一个表示上述 JSON 的字典。我终于得到了这个 JSON:**

成功了！！！！网络控制台回复我“欢迎”！只需查看一下 Rails 日志，确保调用了“subscribed”方法并打印出“subscribed”。是啊！！！好了，那么:现在如何调用 *my_method* 方法？

这是最棒的部分:我们可以传递尽可能多的参数，并且可以用非常相似的 JSON 调用任何方法

区别在于:

*   命令类型已从“订阅”更改为“消息”
*   标识符**必须**与订阅保持一致
*   我们添加了带有 JSON 字符串(带有所有转义字符)的“data”键

让我们从控制台发送这条消息，并查看一下 Rails 日志。我们会看到这些线条

*   *chat channel # my _ method({ " code " =>" a " })。*它识别“action”关键字，并将 JSON 解释为散列参数。如果我们打印*数据*参数，我们得到的确实是 *{"action"= > "my_method "，" code"= > "a"}*

# 我们的目标达到了！！！太好了！

最后一点:在普通的 Rails (http)控制器方法中，您可以编写以下代码:

您正在通过 WebSocket 在“chat_channel”上发送“更新已经到达…”字符串:这甚至会打印在您的 web 控制台上。这看起来不像通知吗？

# 更多参考

要了解 WebSocket 或创建 Rails 项目

*   [什么是 WebSocket？](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)
*   [用 Docker](https://docs.docker.com/compose/rails/) 创建一个 Rails 应用
*   [Rails 官方指南创建新项目](https://guides.rubyonrails.org/getting_started.html)

*感谢阅读……*