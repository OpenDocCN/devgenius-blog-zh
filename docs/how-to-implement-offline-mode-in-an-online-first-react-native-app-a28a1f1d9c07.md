# 如何在在线优先的 React 本地应用中实现离线模式

> 原文：<https://blog.devgenius.io/how-to-implement-offline-mode-in-an-online-first-react-native-app-a28a1f1d9c07?source=collection_archive---------5----------------------->

![](img/a915eb53820c98e48da15b9b4b887143.png)

艾莉娜·普哈卡泽在 [Unsplash](https://unsplash.com/s/photos/storm-sail?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

如果你曾经从事过网络密集型项目，你可能会意识到在恶劣的网络条件下确保应用程序稳定的难度。虽然人们常说应用程序应该被设计成“离线优先”，但事实是，对于大多数项目来说，离线模式是一种事后想法，当团队意识到他们的用户可能没有光纤连接时，就匆忙地从积压中恢复过来。

因此，让我们看看如何让我们的应用程序能够经受住慢速网络风暴，从而将我们的用户从不稳定连接的困境中拯救出来。

# 升起船帆！

您的项目经理告诉您一个令人震惊的消息，您将被部署到一个连接不太好的市场，并且您心爱的应用程序需要在不利的网络条件下可靠地运行。虽然你可能会因缺乏远见而诅咒过去的自己，但你记得后见之明是 20/20，是时候去甲板上扬起一些坚固的风暴帆了。

现在让我们检查一下手头的问题。我们的应用程序的主要功能是让我们的用户向系统中插入新数据，我们不支持批量激活。由于系统的性质，数据需要实时进入，我们不能有停机时间。这意味着如果用户离线，他必须能够以通常的速度继续插入数据。怎么才能让我们的用户这么做呢？

我们的第一直觉可能是在后端实现批量插入特性。然后，您将让用户知道没有网络连接，并继续收集数据，当连接恢复时，这些数据将被批量发送到新的端点。唉，后端团队有其他的优先级，他们不能在任何有用的时间框架内实现批量插入端点。

经过与团队的讨论，达成了一项协议:应用程序将在本地队列中存储新数据，并在连接恢复时将队列中的每个项目发送到后端。虽然有些粗糙，但这种方法是可行的，可以让团队很快发布新特性。

我们现在有了目标；让我们看看我们需要做些什么来完成它。

根据现有信息，我们需要以下内容:

*   实时了解连接状态的可靠信息源
*   一种在队列中存储数据的机制
*   一种在获得连接时将存储在队列中的数据发送到服务器的方法。

幸运的是，我们可以利用社区的出色工作，并使用`[react-native-offline](https://github.com/rgommezz/react-native-offline)`来帮助我们的探索。(免责声明:我很高兴能为这个项目做出贡献，因为它帮助我解决了我以前工作中的一个棘手问题。)

直接来自 repo 的自述文件，这是上述软件包附带的功能之一:

*   Redux 存储中保持连接状态的 Reducer
*   一个 Redux 中间件，用于在离线模式下拦截互联网请求动作并应用 DRY 原则
*   离线队列支持在连接恢复在线时自动重新调度操作，或基于其他调度的操作(即导航相关的操作)取消操作

答对了。我们拥有在应用中实现“离线模式”的所有必要构件。

让我们开始吧。

# 空谈不值钱，给我看看代码！

因此，根据 repo 的自述文件，我们待办事项列表中的第一项是让我们的应用程序了解当前的连接状态。这可以通过用库的`ReduxNetworkProvider`替换 Redux 的`Provider`来快速完成，库的【】将负责监听连接状态并相应地更新`network`缩减器。

来自`*react-native-offline*`[自述](https://github.com/rgommezz/react-native-offline#1--give-the-network-reducer-to-redux)。

来自`*react-native-offline*`[自述](https://github.com/rgommezz/react-native-offline#reduxnetworkprovider)。

如果你使用的是`sagas`，一个更优雅的方式是叉`network-saga`并保持安静，传奇将会为你监听和更新状态。

来自`*react-native-offline*`[自述](https://github.com/rgommezz/react-native-offline#1--give-the-network-reducer-to-redux)。

我们现在可以引用`network`缩减器来读取连接状态。

来自`*react-native-offline*`[自述](https://github.com/rgommezz/react-native-offline#state)。

然后，您可以编写一个自定义挂钩，让所有组件都知道当前的连接状态。

一个读取网络信息的基本钩子。

我们现在可以利用`useNetworkInfo`来通知用户该应用程序当前处于离线状态。

我们给用户的信息。

太好了，我们现在已经完成了第一个要求。

这是关于如何在 React Native 中实现离线模式的多系列文章的第一部分。[点击这里阅读第二部分](https://medium.com/@accounts_32070/how-to-implement-offline-mode-in-an-online-first-react-native-app-part-2-f0c5abc03900)！