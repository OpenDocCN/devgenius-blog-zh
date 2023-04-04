# 如何使用 redux toolkit 和使用 slice，react(后半部分，在实际应用中使用)

> 原文：<https://blog.devgenius.io/how-to-use-redux-toolkit-with-using-slice-react-second-half-part-to-use-in-a-real-app-9043c64e4c87?source=collection_archive---------2----------------------->

![](img/e543ca5e72793b54c31da480cbbd5351.png)

图片来自[https://blog . dastasoft . com/posts/how-to-get-better-easy-state-management-redux-toolkit](https://blog.dastasoft.com/posts/how-to-get-better-easier-state-management-redux-toolkit)

**目的**

我写了一篇文章解释 redux 和 redux toolkit basic。正如我所说的，这篇文章将重点介绍如何在真正的应用程序中使用，并展示我如何在我的应用程序中使用该功能。如果你想知道 redux 的基本概念或者如何创建简单的计数器 app，请查看我写的上一篇文章。

**最终版本**

我想展示如何使用 redux toolkit 创建一个如下所示的汉堡配送应用程序。

![](img/11d8c241f556a88e9b835733c03490f2.png)

在我的应用程序中，我可以更改“购物车商品计数”、“添加购物车信息”、“删除购物车信息”、“删除所有购物车信息”。

**步骤(前三步同上)**

1.  创建 react 应用程序。在我的例子中，我使用下面的命令用 typescript install 创建了这个应用程序

```
npx create-react-app “YOUR APP NAME” --template typescript
```

2.使用以下命令安装 redux toolkit。

```
npm install @reduxjs/toolkit
```

3.如果要使用 typescript，应该像下面这样添加 tsconfig.json，否则跳过这一部分。

4.创建如下所示的“counterSlice.tsx(或 js，jsx)”文件。从 reduxtoolkit 导入 createSlice，并使用它。您应该使用 createSlice 作为导出变量，设置名称和初始状态，减少。正如我写的 reducer 是用来“创建新状态”的，所以你应该加上你想如何改变你的状态。上次我只为 reducer 添加了“附加数”和“减去数”,

但是在这种情况下，我创建了三个片归约器，它们是 counterSlice.tsx、changeCartSlice.tsx、changePriceSlice.tsx。你可以像上一篇文章一样导出每个动作和减少器。

一般来说，如果你想使用基本的，有用的应用程序，我建议你去 CRUD 应用程序。CRUD app 表示“创建”、“读取”、“更新”、“删除”的首字母，这是有用的 app 的基本功能，所以如果你想创建切片和减速器，你应该有意识地同时或逐步 CRUD app。

5.如下图创建“store.tsx(或 js，jsx)”。您需要导入您在步骤 4 中创建的“configureStore”和“counterReducer”，changeCartReducer，changePriceReducer，并如下设置这个 Reducer。实际上，如果你没有选择使用 typescript，你不需要导入 useSelecter 和 TypedUseSelectorHook，也不需要导出，因为这是为了在 redux-toolkit 中正确使用 typescript。

正如你所注意到的，一旦你创建了如下的商店，它是非常可重用的。

6.这在本文中不是非常重要的部分，但是您可以像下面这样设置“App.tsx(或 js，jsx)”，这只是显示其他组件。

7.设置 provider 并存储在“index.tsx(或 js，jsx)”中，如下所示。这个提供商意味着提供商店的整个应用程序的信息。换句话说，你可以在你的应用中的任何地方使用这个商店的信息。这一步与上一篇文章相同。

8.在您想要使用的地方使用切片文件。在我的例子中，我想使用 Menu.tsx 和 Header.tsx，所以我把我的切片放在这两个文件中。当你想使用这些片时，你应该使用 dispatch，在我的例子中，我在 react-redux 中使用了 useDispatch hook。

例如，当谈到 menu.tsx 时，由于我想在单击每个菜单卡片时使用“addCount”、“sumPrice”、“addCart”，所以我设置了 menu 参数来获取菜单数据，并在 add item 函数内使用这些数据来更改状态。

就 Header.tsx 而言，使用方法几乎相同，尽管略有不同。不同的是使用“使用选择器钩子”来显示数据。

9.搞定了。你可以创建真正的应用程序如下。

![](img/11d8c241f556a88e9b835733c03490f2.png)

**结论**

一旦你熟悉了 redux，管理你的状态就容易多了。起初你会觉得有点复杂，但是你会注意到你得到了一个很好的工具。

**参考**

Redux 和 React Native，简单登录示例流程:[https://medium . com/@ aurelie . lebec/redux-and-React-Native-simple-log in-example-flow-c 4874 cf 91 DDE](https://medium.com/@aurelie.lebec/redux-and-react-native-simple-login-example-flow-c4874cf91dde)

Redux 入門者向け初めてのRedux ToolkitとRedux Thunkの非同期処理: [https://reffect.co.jp/react/redux-toolkit](https://reffect.co.jp/react/redux-toolkit)

Redux-toolkitを使ったStoreをtypescriptで作ってみる: [https://zenn.dev/engstt/articles/293e7420c93a18](https://zenn.dev/engstt/articles/293e7420c93a18)

我的 git hub 回购:[https://github.com/aujourdui/burger-delivery](https://github.com/aujourdui/burger-delivery)

我的观点:[https://gist.github.com/aujourdui](https://gist.github.com/aujourdui)

感谢您的阅读！！