# 颤动中的腹板插口

> 原文：<https://blog.devgenius.io/web-socket-in-flutter-615d21ddf1c5?source=collection_archive---------0----------------------->

![](img/6b4a3de19f9b664cb1eaafb0e6846384.png)

尼尔·索尼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

用于双向通信，如实时消息、GPS 跟踪、视频通话等。我们经常使用 web socket。让我们深入了解它是如何工作的，它的实现和最佳实践是什么。

附注:在阅读过程中，你会得到很多专业建议。

> Web 套接字在发布/订阅模式下工作

# 什么是发布/订阅模式？

发布/订阅消息传递意味着发布/订阅消息传递模型。我们必须订阅数据，socket 将开始提供更新。很简单的一句话，订阅获取数据流，退订停止获取数据。

在连接 websocket 之后，我们必须通过双向通信来保持它的连接。如果任何一种方式停止发送数据，连接将会丢失，您必须重新连接。让我们开始实现 Flutter。为此，我们将使用下面给出的插件:

# 1.将它添加到您的 pubspec.yaml 中

```
web_socket_channel: ^2.1.0
```

将这一行添加到 *pubspec.yaml* 的依赖项中，运行 *flutter pub get* 。

将其导入到您的文件中

```
import 'package:web_socket_channel/io.dart';
```

> Pro tip 1:让你的助手类成为一个[单例类](https://flutterbyexample.com/lesson/singletons)，并在这里保留所有重要的方法。

# 2.连接到插座

```
IOWebSocketChannel _client;WebSocket.*connect*(_endPoint).then((ws) {
  _client = IOWebSocketChannel(ws);
  if (_client != null) {
    _client.sink.add("Socket added");
  }
}).onError((error, stackTrace) {

});
```

这里，我们正在建立与套接字的连接。端点将是你的 url 以 *ws 或 wss 开头。*连接后，我们将获得一个套接字客户端。保持低调。IOWebSocketChannel.sink.add 将用于发送数据。

# 3.订阅一个主题

```
_client.sink.add(subscriptionText);
```

这里，订阅文本将是您想要发送到后端以开始订阅的消息。

# 4.开始收听消息

```
_client.stream.listen((message){
    //parse your message here
 },
 onDone: () {

 },
);
```

IOWebSocketChannel.stream 将开始发送可以通过上述方法监听的数据。你可以解析你的信息并在你的应用中使用。

onDone:它将调用断开插座。

> 专业技巧 2:总是以字节发送和接收数据，或者使用 ProtoBuf

# 4.取消订阅该主题

```
_client.sink.add(unSubscriptionText);
```

发送后端提到的取消订阅消息。它将停止数据流。

# 5.断开插座

```
_client?.sink?.close(status.goingAway);
```

IOWebSocketChannel.sink.close 将关闭连接。这里 status.goingAway 是在 library 中定义的状态。您也可以使用自定义文本。

# 最佳实践

1.  避免多次发送订阅消息。
2.  每 10 秒发送一次心跳。心跳是保持连接活动的文本。这是后端服务器上提到的文本。
3.  每次连接/断开互联网时，您都应该连接您的插座，并再次订阅所有主题。
4.  保持一个缓冲队列，以保存订阅，如果你失去了互联网之间的连接。您可以在重新连接互联网时发送此订阅。
5.  维护一个映射来检索以前订阅的数据。
6.  如果你得到错误或由于互联网断开，保持重试至少 3 次(或者你可以继续尝试)。
7.  如果不需要，不要忘记退订主题。
8.  应用程序恢复时连接套接字，应用程序暂停时断开连接。

> Pro tip 3:尽量少一次订阅。

这是工作代码。您可以根据您的使用情况对其进行修改。

今天到此为止。谢谢你坚持到最后。欢迎建议。快乐编码。

让我们现在就在 [Twitter](https://twitter.com/iamAshujha) 、 [Instagram](https://www.instagram.com/ashutosh.j_/) 或 [LinkedIn](https://www.linkedin.com/in/ashujhaji/) 上连线。