# JavaScript 的状态管理

> 原文：<https://blog.devgenius.io/the-state-management-of-javascript-1d557766e19a?source=collection_archive---------2----------------------->

![](img/88c2d4766f709dbf9f1d8e50866f00a0.png)

# 什么是商店？

如果你把一个应用程序归结为两个最基本的组件，你会得到 UI 和状态。一个 app 主要由 UI 和状态组成。

如果你要把你写过的任何一个 bug 的根本原因归结起来，很可能这个 bug 是由国家管理不善引起的。应用程序的预期状态和它获得的状态不同步。你有没有想过为什么技术支持的第一条规则是先关机再开机？这是因为它重置了状态，从而解决了问题。

> 通过增加状态的可预测性，可以减少应用程序中的错误数量。

可能让你惊讶的是，这不是 Redux 的课程，也不是 React 的课程，而是如何让你的状态更可预测的课程。您将在本课程中学到的模式和心智模型超越了任何特定的前端库。

所以如果这门课的目标是增加你状态的可预测性，我们该怎么做呢？如果你想到你以前写过的任何一个普通的应用程序，你很可能在你的整个应用程序中都有状态。这很糟糕吗？当然不是。然而，这并没有让我们的状态变得更加可预测。这可能使它更难预测。

我们要学习的第一个原则是把所有的状态放在一个位置，而不是把状态分散在你的应用程序中。我们将这个位置称为“状态树”。通过将我们的状态放在一个位置，我们可以为如何访问和更新该状态定义清晰的规则。

手挥够了，我们的状态树会给我们什么实际的好处？

第一，共享缓存。您是否遇到过 React 应用程序中的两个组件需要访问同一个状态？通常情况下，你要做的是将状态“上移”到最近的父组件。这行得通，直到行不通。通过将我们的状态放在一个单一的状态树中，任何需要访问它的组件都可以获得访问权——不管它们在应用程序中的位置如何。

另一个好处是可预测的状态变化。如前所述，通过为谁可以更新和访问状态建立严格的规则，我们自然增加了应用程序中状态的可预测性。

下一个好处是改进了开发人员工具。因为我们所有的状态都存在于一个位置，所以应用程序的其他部分可以在不丢弃状态的情况下重新加载。除此之外，围绕“状态是如何形成的？”这一问题构建工具也更加容易。

另一个好处与使用纯函数有关。这里的想法是，通过将我们所有的状态放在一个位置，根据定义，我们应用程序的其余部分只是接收状态并基于该状态显示 UI。

最后，服务器渲染。如果您以前曾经使用过服务器呈现，您会知道整个想法是当最初的请求进来时，将应用程序的状态和标记一起返回。事实证明，如果您的所有状态都在一个位置，那么将状态和标记作为响应发回会更容易。

既然我们已经认识到将我们的状态放在一个位置可能是一个好主意，我们还需要一种与之交互的方法。具体来说，我们需要一种方法来获取我们的状态，更新我们的状态，并监听我们状态的变化。所以我们有一个名字，姑且称之为“商店”。

> 存储由我们的状态树和我们与之交互的三种方式组成——获取状态树、更新状态树和监听状态树的变化。

# 将状态转换表示为动作

在这一点上，为了增加可预测性，我们希望创建一个应用程序中可能发生的所有事件的集合，这些事件将更新状态。我们将使用普通的旧 JavaScript 对象来表示这些事件，但是将它们重新命名为“动作”。

动作是信息的有效负载，发送关于对应用程序的状态进行何种类型的转换的指令以及任何其他相关信息。这是“动作是一个对象，它描述了你想要对你的状态进行什么样的转换”的一种奇特的说法。对象构成了一个完美的动作数据结构，因为它可以完美地描述应用程序中发生的事件类型。

让我们看看一个典型的动作是什么样子的

```
const action = {
  type: "LIKE_TWEET",
  id: 999417610456724890,
  uid: "denislistiadi",
};
```

那真是虎头蛇尾。如上所述，动作只是一个对象，它描述了应用程序中可能发生的一些动作(或事件)。注意，动作必须有一个`type`属性来指定正在发生的动作的“类型”。除了`type’`之外，动作的结构由你决定。

# 还原剂成分

Reducer composition 听起来很吓人，但是它比你想象的要简单。其思想是，您可以创建一个缩减器，不仅管理 Redux 存储的每个部分，还管理任何嵌套的数据。

假设我们正在处理一个具有这种形状的状态树。

```
{
  users: {},
  setting: {},
  tweets: {
    hfolmg: {
      id: 'hfolmg',
      text: 'What is a javascript?',
      author: {
        name: 'Denis Listiadi',
        id: 'denislistiadi',
        avatar: 'twt.com/dl.jpg'
      }
    }
  }
}
```

我们的状态树上有三个主要属性，`users`、`settings`和`tweets`。自然地，我们会为它们中的每一个创建一个单独的缩减器，然后使用 Redux 的`combineReducers`方法创建一个根缩减器。

```
const reducer = combineReducers({
  users,
  settings,
  tweets,
});
```

`combineReducers`、引擎盖下，是我们先来看看减速器的组成。`combineReducers`负责调用所有其他 reducers，将它们关心的状态部分传递给它们。我们正在制作一个根变径管，将一堆其他变径管组合在一起。记住这一点，让我们更仔细地看看我们的`tweets`减速器，以及我们如何再次利用减速器的组成，使其更加条块化。具体来说，让我们看看用户可能会如何改变他们的头像与我们的商店目前的结构方式。

这是我们开始时的骨架

```
function tweets (state = {}, action) {
  switch(action.type){
    case ADD_TWEET :
      ...
    case REMOVE_TWEET :
      ...
    case UPDATE_AVATAR :
      ???
  }
}
```

我们感兴趣的是最后一个，`UPDATE_AVATAR`。这个很有趣，因为我们有一些嵌套数据——记住，reducers 必须是纯的，不能改变任何状态。

这里有一种方法。

```
function tweets (state = {}, action) {
  switch(action.type) {
    case ADD_TWEET :
      ...
    case REMOVE_TWEET :
      ...
    case UPDATE_AVATAR :
      return {
        ...state,
        [action.tweetId]: {
          ...state[action.tweetId],
          author: {
            ...state[action.tweetId].author,
            avatar: action.newAvatar
          }
        }
      }
  }
}
```

这是一个很大的传播运营商。这样做的原因是，对于每一层，我们希望将该层的所有属性分布到我们正在创建的新对象上(因为不变性)。如果，就像我们通过将他们关心的状态树的切片传递给他们来分离我们的`tweets`、`users`和`settings`reducer 一样，我们对我们的`tweets` reducer 及其嵌套数据做同样的事情会怎么样？

通过这样做，上面的代码将被转换成这样。

我们所做的就是将嵌套的`tweets`数据的每一层分离到它自己的 reducers 中。然后，就像我们对根归约器所做的那样，我们把它们所关心的状态片段传递给这些归约器。

# Redux 中间件

我喜欢 Redux 中间件的地方在于，它为您提供了工具，让您决定想要用它们构建什么。我喜欢 JavaScript 生态系统的原因是，如果你需要做一些事情，有一个包可以满足你。

下面是 Redux 生态系统中一些通过中间件实现的流行包。

*   [redux-api-中间件](https://github.com/agraboso/redux-api-middleware):调用 API 的 Redux 中间件。
*   [Redux-logger](https://github.com/evgenyrodionov/redux-logger):Redux 的 Logger 中间件。
*   [Redux-promise-middleware](https://github.com/pburtchaell/redux-promise-middleware):Redux 中间件，用于解决和拒绝带有条件乐观更新的承诺。
*   [Redux-thunk](https://github.com/gaearon/redux-thunk):Redux 的 Thunk 中间件。
*   [redux-logic](https://github.com/jeffbski/redux-logic) :组织业务逻辑和动作副作用的 redux 中间件。
*   [redux-observable](https://github.com/redux-observable/redux-observable) : RxJS 中间件，用于 redux 中使用“Epics”的动作副作用。
*   [Redux-test-recorder](https://github.com/conorhastings/redux-test-recorder):Redux 中间件，通过 UI 交互自动生成对减速器的测试。
*   [redux-reporter](https://github.com/ezekielchentnik/redux-reporter) :向第三方提供商报告操作&元数据，对分析和错误处理非常有用(New Relic、Sentry、Adobe DTM、Keen 等)。)
*   redux-localstorage:Store enhancer，将 Redux 存储状态(子集)同步到 local storage。
*   [redux-node-logger](https://github.com/low-ghost/redux-node-logger) :一个用于节点环境的 Redux Logger
*   [redux-catch](https://github.com/sergiodxa/redux-catch) :错误捕捉中间件
*   redux-cookies-middleware :一个 redux 中间件，它将您的 Redux 存储状态的子集与 cookies 同步。
*   [redux-test-recorder](https://github.com/conorhastings/redux-test-recorder):Redux 测试记录器是一个 Redux 中间件+包含一个组件，用于根据应用程序中的动作自动为您的减速器生成测试