# 通过 React、Redux 和“无服务器”使用 API 网关 Web Socket API:第 2 部分

> 原文：<https://blog.devgenius.io/using-api-gateway-web-socket-api-with-react-redux-and-serverless-part-02-da407a47d070?source=collection_archive---------1----------------------->

![](img/4b7657110965dbf5f973ca4af10a34fa.png)

大家好，欢迎回到本文的第 2 部分。如果您在看完[上一篇文章](https://medium.com/@iwiick/using-api-gateway-web-socket-api-with-react-redux-and-serverless-part-01-2a9ec01c8a40)后不在这里，快速回顾一下，我们看了如何设置一个使用 API 网关 Web Sockets 的无服务器后端，同时解释了代码的不同部分，如何和为什么使用它们，以及我们将使用的路线。

好吧。现在，我们来看看如何设置前端。本文重点介绍在 React 应用程序中使用 **React 钩子**和 **Redux** 来建立套接字连接并访问从后端传回客户端的数据:)

## 为什么使用 React 钩子

简单的回答是，它使代码整洁一致。这样一旦钩子写好了，就只需要在应用程序的功能组件中使用它们了。有了编写在钩子内部的关于套接字连接的逻辑，我们就可以很容易地在钩子内部修改套接字，而不需要修改组件。

## 为什么 Redux

本文不关注 Redux。我们将只使用它来访问整个应用程序中单个 Web Socket 实例的数据:)。因为 Redux 是为应用程序中的**状态管理**而创建的，所以我们将这么做。如果您不熟悉使用 Redux，本文的某些部分会让您感到有些困惑。

## 概观

好吧。这就是我们现在要在坚果壳里做的事情。我们将创建 2 个挂钩。一个称为 **useSocketConnection()** ，另一个称为 **useSocketdata()** 。useSocketConnection 挂钩将在应用程序的高级组件中使用，它将只被调用一次。这将创建一个 web Socket 实例，它的数据如连接状态(真、假)、连接 Id 等存储在 Redux 状态中，通过回调进行更新。useSocketData 挂钩将在低级组件中使用。只要 useSocketConnection 钩子以前在高级别调用过，它就可以用在任何数量的组件中。useSocketData hook 将返回关于特定 Web 套接字实例的相同数据，而不会创建不同的实例:)。让我们现在开始…

# useSocketConnection 挂钩

第一行非常简单。我正在导入这些，因为我以后会需要它们:)。第 2 行用于导入我的 **Redux action** 类型。如果您不熟悉它，这基本上是每种类型的唯一字符串。在我的例子中，我有 3 个唯一的字符串值作为“SOCKET_CONNECTION”、“SOCKET_CONNECTION_OPEN”和“SOCKET_CONNECTION_CLOSE”导入。我已经在第 7 行从 React-redux 导入了**used dispatch**钩子。我在第 10 行有服务 URL 和一些州。当这些状态改变时，一个 useEffect 触发 **connectSocket** 函数。

一个简单的 if 条件检查套接字是否打开。如果没有，它将在第 23 行创建一个新的 Web Socket 实例。我们还添加了事件侦听器，用于套接字打开、关闭以及套接字从后端发送消息的时间。让我们为 socket“open”、“close”和“message”创建回调函数(用 useCallback 实现):)。

这些是套接字的“打开”和“关闭”侦听器。这里需要注意的重要一点是，我们向第 3 行中的套接字发送了一条消息。我们将**动作作为$default** 并且将**对象字符串化为**。剩下的就是基本的 JS，更新 react 状态，分派相关的 redux 动作。最后一个回调是套接字“消息”。

这里需要注意的重要一点是第 4 行的 JSON.parse。我们将如何发送通知在第 15 行。在上一篇文章中，$default route 被配置为发送一个数据对象，包括 **connectionId** 和 **callbackUrlForAws** 。当我需要从后端发送通知时，我简单地添加了第三个属性，称为“**通知**”。同样，这可以是您想要的任何属性:)。在我的例子中，我有一个名为 notification 的属性，带有一个对象值。该对象包含 web 通知所需的数据。

> 在这个特定的例子中，当后端发送一个带有“notification”属性的消息对象时，useSocketConnection 钩子将使用提供的数据发送一个通知。我个人正在使用 [AntD 通知](https://ant.design/components/notification/)，为了不混淆你们，我已经把它去掉了:)

完整的 useSocketConnection 钩子看起来会像这样…

注意第 33 行中的 **reconnectSocket** 函数，稍后我们将从 **useSocketData** 钩子中使用它:)

只是为了让你们知道我的 Redux Reducer 减速器是什么样子……它只是一个用于任何 Redux 项目的普通减速器。

# useSocketData 挂钩

好吧。这就把我们带到了下一个问题。正如我之前简单提到的，这个钩子必须在我们之前创建的 useSocketConnection 钩子之后，在一个低级组件中调用。这个钩子可以从任何组件调用。

> 注意，这个钩子将访问并返回与套接字连接相关的 redux 状态，并使用我们之前在 useSocketConnection 钩子中创建的 **reconnectSocket** 函数重新建立一个断开的连接。

这里有两点需要注意。一个是我们使用来自 **react-redux** 的 **useSelector** 钩子从 redux 状态访问套接字相关数据。第二个是在第 17 行使用了重新连接函数。我们已经使用了一个 useEffect，它将检查套接字**已连接**状态的变化，并在套接字连接由于某种原因中断时重新连接。catch 语句是为了**确保 useSocketConnection 钩子在这个**之前被调用。

现在，可以从 **useSocketConnection** 钩子和 **onSocketMessage** 回调来处理和定制通知。因此，调用 useSocketData 挂钩的组件的 socketData 属性的一个示例是，使用它来创建刷新功能。

> 我细说的话，就以之前的“[作业服务](https://medium.com/@iwiick/create-a-background-job-service-with-dynamodb-streams-and-lambda-functions-with-serverless-60b7bc49fd48)为例。我们触发一个后台作业，通过电子邮件邀请一些参与者。一旦工作完成，我们可以使用 **connectionId** 和 **callbackUrlForAws** 将数据对象发送回客户端。在上面的实现中，如果数据对象包含通知属性，useSocketConnection 钩子将从 onSocketMessage 回调中返回通知。如果 Object.notification 需要一个关于作业的惟一标识符，我们可以用它来刷新客户端的一些数据。

这个特定上下文中刷新功能的一个示例如下。

就这样了，伙计们。希望你们能从中学到一些东西，并随时向我寻求任何澄清…干杯:)

*由* **到*鹤山***