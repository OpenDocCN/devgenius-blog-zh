# 如何在在线优先的本地应用程序中实现离线模式:第 2 部分

> 原文：<https://blog.devgenius.io/how-to-implement-offline-mode-in-an-online-first-react-native-app-part-2-f0c5abc03900?source=collection_archive---------8----------------------->

![](img/155e4e5ac9f2a7a6eaa8c96785e52060.png)

Johannes Plenio 在 [Unsplash](https://unsplash.com/s/photos/sail-storm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

[在前一部分](https://medium.com/dev-genius/how-to-implement-offline-mode-in-an-online-first-react-native-app-a28a1f1d9c07)中，我们已经开始探索如何升级我们现有的项目，以应对不利的网络条件。我们已经学会了如何利用现有的库，比如`[react-native-offline](https://github.com/rgommezz/react-native-offline)`，通过了解当前的网络状态来增强我们的组件。今天我们将学习如何实现第二个需求:**推迟对服务器的请求，直到我们重新获得连接**。

# 不要忘记我！

如果您遵循了前一部分中描述的步骤，现在您应该在 redux 状态中有一个名为`network`的有用分支，其结构如下。

摘自` react-native-offline` [自述](https://github.com/rgommezz/react-native-offline#state)。

您会注意到一个代表`Array<*>`的`actionQueue`元素。嗯，我的朋友，这个`actionQueue`是开启你的应用程序离线潜力的强大钥匙！

`actionQueue`做的是在 app 离线的时候，持有已经被`react-native-offline`中间件拦截的`actions`和`thunks`。一旦连接恢复，同一个中间件将负责清空队列并重新分发内容。

这意味着，如果您正在调度`actions`或`thunks`来触发对 API 的调用，那么在离线时拦截它们并以更好的连接性再次发送它们是非常容易的。让我们看看它是怎么做的！

首先，我们需要向我们的商店添加`react-native-offline`中间件。请注意，`createNetworkMiddleware` **应该是链中第一个正常工作的中间件**。

来自` react-native-offline` [自述](https://github.com/rgommezz/react-native-offline#usage-3)。

`createNetworkMiddleware`函数具有以下签名。

来自` react-native-offline` [自述](https://github.com/rgommezz/react-native-offline#createnetworkmiddleware)。

根据[文档](https://github.com/rgommezz/react-native-offline#usage-3)，这个中间件的默认行为是拦截所有遵循 Redux 的[约定](https://redux.js.org/advanced/async-actions)的异步动作，这意味着它会捕捉所有类似`FETCH_USER_ID_REQUEST`的动作。默认行为将阻止动作被发送，而是发送一个`@@network-connectivity/FETCH_OFFLINE_MODE`，我们可以在我们的缩减器中拦截它。

来自` react-native-offline` [自述](https://github.com/rgommezz/react-native-offline#usage-3)。

请注意，这只*阻止*我们的`FETCH`动作被发送，并发送一个*替代动作*。虽然有用，但我们想做的是*推迟*动作，并在连接恢复时再次发送。幸运的是，按照下面的结构，我们可以通过向想要推迟的动作添加一个`meta`字段来轻松做到这一点。

来自` react-native-offline` [自述](https://github.com/rgommezz/react-native-offline#plain-objects)。

所以，假设我们有一个`FETCH_USER_SELECTED_PRODUCT_REQUEST`动作，它将触发一个`saga`。saga 会告诉我们的服务器用户已经选择了一个新产品。如果我们离线，不要让呼叫失败，我们可以标记`FETCH_USER_SELECTED_PRODUCT_REQUEST`在离线时被拦截，因此推迟触发传奇直到我们重新上线。

我们的行动被拦截

然后我们可以在我们的 reducer 中监听`@@network-connectivity/FETCH_OFFLINE_MODE`，让我们的用户知道他的选择已经被记录下来，一旦连接可用，就会被上传到我们的服务器。

我们的“产品”行动减速器。

我们完了！我们的应用程序现在将能够在离线时拦截动作，在线时重新调度它们，并让用户了解当前的连接状态。