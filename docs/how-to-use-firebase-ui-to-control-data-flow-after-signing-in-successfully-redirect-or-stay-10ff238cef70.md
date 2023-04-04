# 登录成功后如何使用 Firebase UI 控制数据流(重定向还是停留？)

> 原文：<https://blog.devgenius.io/how-to-use-firebase-ui-to-control-data-flow-after-signing-in-successfully-redirect-or-stay-10ff238cef70?source=collection_archive---------5----------------------->

![](img/2f00cfcd971d76b0f9d9735517d84dd3.png)

要构建用户认证系统，Firebase UI 是一个出色的工具！一旦你正确地安装和配置了它(在你的代码和 Firebase 控制台中)，你会得到 11 个漂亮的工作用户界面来自动登录，对于 GCP(Google 云平台)用户，你甚至会有两种方法来启用 SAML 和 OIDC 提供者。

# 安装和配置

启动并不太难，你可以按照[文档](https://github.com/firebase/firebaseui-web#installation)进行安装，在 Firebase 控制台中启用特定的提供者并进行配置。由于我将它用于我的 Gatsby(这是一个用 React 编写的框架)应用程序，我也使用这个[文档](https://github.com/firebase/firebaseui-web-react#firebaseui-react-components)在 React 中进行设置。

有两个组件可以使用 FirebaseUI : `FirebaseAuth`和`StyledFirebaseAuth`。文档中对此差异进行了说明:

> `FirebaseAuth`有一个对 FirebaseUI CSS 文件的引用(它`requires`CSS)。
> 
> `StyledFirebaseAuth`是直接与 CSS 捆绑在一起的。

我用的是后一种。这是文档中一个`StyledFirebaseAuth`的基本示例:

```
// Import FirebaseAuth and firebase.
import React from 'react';
import StyledFirebaseAuth from 'react-firebaseui/StyledFirebaseAuth';
import firebase from 'firebase';// Configure Firebase.
const config = {
  apiKey: 'AIzaSyAeue-AsYu76MMQlTOM-KlbYBlusW9c1FM',
  authDomain: 'myproject-1234.firebaseapp.com',
  // ...
};
firebase.initializeApp(config);// Configure FirebaseUI.
const uiConfig = {
  // Popup signin flow rather than redirect flow.
  signInFlow: 'popup',
  // Redirect to /signedIn after sign in is successful. Alternatively you can provide a callbacks.signInSuccess function.
  signInSuccessUrl: '/signedIn',
  // We will display Google and Facebook as auth providers.
  signInOptions: [
    firebase.auth.GoogleAuthProvider.PROVIDER_ID,
    firebase.auth.FacebookAuthProvider.PROVIDER_ID
  ]
};class SignInScreen extends React.Component {
  render() {
    return (
      <div>
        <h1>My App</h1>
        <p>Please sign-in:</p>
        <StyledFirebaseAuth uiConfig={uiConfig} firebaseAuth={firebase.auth()}/>
      </div>
    );
  }
}
```

# 登录后回拨

文档非常清楚，所以我不打算对此谈论太多。我最关心的是数据流，因为当我几天前在我的盖茨比应用程序中使用时，我没有看到太多的信息。所以我想把它写下来，以防将来有人可能需要它。

配置好之后，您可以登录到提供者，将其连接到`Firestore`，并在`Firestore`的`Authentication`中创建一个用户。之后，`signInSuccessUrl: ‘/signedIn’`将把流重定向到`/signedIn`。

但是如果你想做的不仅仅是重定向呢？那么你应该使用一个叫做:`signInSuccessWithAuthResult`的回调。它应该嵌套在`uiConfig`中的`callbacks`对象中。因此[文档](https://github.com/firebase/firebaseui-web-react#using-firebaseauth-with-local-state)中的示例如下所示:

```
// Configure FirebaseUI.
  uiConfig = {
    // Popup signin flow rather than redirect flow.
    signInFlow: 'popup',
    // We will display Google and Facebook as auth providers.
    signInOptions: [
      firebase.auth.GoogleAuthProvider.PROVIDER_ID,
      firebase.auth.FacebookAuthProvider.PROVIDER_ID
    ],
    callbacks: {
      signInSuccessWithAuthResult: () => false
    }
  }
```

## 使用`signInSuccessWithAuthResult`回调来决定重定向或停留

回调将是一个接受两个参数的函数，`authObject`(通常是`user`)和`redirectURL`。因此，您可以在成功登录后执行更多操作。

它将返回的实际上是一个简单的`boolean`值。如果它返回 true，那么在登录后，用户将被重定向到由`signInSuccessUrl`指定的 URL。当它返回 false 时，用户将停留在同一页面上。

## 在`signInSuccessWithAuthResult`中处理异步请求

我在使用 `signInSuccessWithAuthResult`时遇到的另一个问题是，由于它确切地要求一个`boolean`值，当我使用`async/await`试图从数据库或 API 获取数据时，我得到一个错误，指出我返回的是一个`Promise<boolean>`，这与`signInSuccessWithAuthResult`预期接收的不完全匹配。

要处理这个问题，只需使用一个非常小的技巧:使用`.then`来处理函数内部的承诺。它只是一个小技巧，但我在网上没有看到太多关于它的信息，所以我写在这里。希望有一天能帮到你。

# 笔记

就试着分享一个关于`signInFlow`的笔记吧。有两种方式:`popup`和`redirect`。它默认为`redirect`，所以如果你想加快加载过程，将其指定为`popup`！

感谢阅读！