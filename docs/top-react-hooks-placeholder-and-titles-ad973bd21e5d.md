# 顶部反应挂钩—占位符和标题

> 原文：<https://blog.devgenius.io/top-react-hooks-placeholder-and-titles-ad973bd21e5d?source=collection_archive---------2----------------------->

![](img/31e945751286eccf694631f1f62e2742.png)

由[万花筒](https://unsplash.com/@kaleidico?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# react-乐观-ui-hook

react-optimistic-ui-hook 库允许我们创建一个 ui，它不等待操作完成来更新最终状态。

当读取的数据正在加载时，它们立即切换到最终状态并显示假数据。

我们可以通过运行以下命令来安装它:

```
npm install react-optimistic-ui-hook
```

或者:

```
yarn add react-optimistic-ui-hook
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useOptimisticUI } from "react-optimistic-ui-hook";const USERNAME = "facebook";
const PREDICTED_AVATAR_URL = "http://placekitten.com/200/200";
const DELAY_IN_MS = 2000;async function getGithubAvatarURL(username) {
  const response = await fetch(`https://api.github.com/users/${username}`);
  const data = await response.json(); return new Promise(resolve => {
    setTimeout(() => {
      resolve(data.avatar_url);
    }, DELAY_IN_MS);
  });
}export default function App() {
  const { status, result, error } = useOptimisticUI(
    () => getGithubAvatarURL(USERNAME),
    PREDICTED_AVATAR_URL
  ); return (
    <div>
      <img src={result} alt="avatar" />
      <p>Status: {status}</p>
      <p>Error: {JSON.stringify(error)}</p>
    </div>
  );
}
```

我们有异步获取 Github 用户头像的`getGithubAvatarURL`函数。

然后我们可以用这个和`useOptimisticUI`挂钩来运行这个承诺。

第一个参数具有获取数据的回调。

第二个是占位符的 URL，当真正的数据被下载时显示。

钩子返回图像加载状态的`status`。

`result`是结果，是从`getGithubAvatarURL`返回的承诺的解析值。

`error`有任何错误发生。

# 反应页面名称

React Page Name 是一个库，允许我们在 React 应用程序中更新页面标题。

要安装它，我们运行:

```
npm install react-page-name
```

然后我们可以通过写来使用它:

```
import React from "react";
import { usePageName } from "react-page-name";export default function App() {
  usePageName("hello world"); return <div>hello world</div>;
}
```

`usePageName`钩子将带有页面标题的字符串作为参数。

它还带有一个高阶分量。

例如，我们可以写:

```
import React from "react";
import { withPageName } from "react-page-name";let Page = () => {
  return <div>hello world</div>;
};Page = withPageName("hello world")(Page);export default function App() {
  return (
    <div>
      <Page />
    </div>
  );
}
```

我们创建了`Page`组件，并使用`withPageName`高阶组件来设置页面标题。

高阶组件也将使`setPageName`方法对我们的组件可用，所以我们可以写:

```
import React from "react";
import { withPageName } from "react-page-name";let Page = ({ setPageName }) => {
  setPageName("hello world"); return <div>hello world</div>;
};Page = withPageName()(Page);export default function App() {
  return (
    <div>
      <Page />
    </div>
  );
}
```

![](img/34e418fefbd1433d47aca2aab95956dd.png)

照片由 [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

react-optimistic-ui-hook 让我们显示一个占位符，直到真正的数据被加载。

React Page Name 允许我们用一个钩子或者更高阶的组件来改变页面标题。