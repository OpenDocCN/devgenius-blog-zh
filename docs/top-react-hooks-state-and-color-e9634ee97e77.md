# 顶部反应挂钩—状态和颜色

> 原文：<https://blog.devgenius.io/top-react-hooks-state-and-color-e9634ee97e77?source=collection_archive---------8----------------------->

![](img/af6ed42aa2eb53f9e5bfb6e2559a0d67.png)

罗伯特·卡茨基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应动力挂钩

react-powerhooks 库附带了各种可以在我们的应用程序中使用的钩子。

要安装它，我们运行:

```
yarn add react-powerhooks
```

然后我们可以使用它附带的钩子。

`useActive`让我们观察一个元素是否是活动的。

`useInterval`钩子让我们在 React 组件中定期运行代码。

要使用它，我们可以写:

```
import React from "react";
import { useInterval } from "react-powerhooks";export default function App() {
  const [time, setTime] = React.useState(null);
  const { start, stop } = useInterval({
    duration: 1000,
    startImmediate: false,
    callback: () => {
      setTime(new Date().toLocaleTimeString());
    }
  }); return (
    <>
      <div>{time}</div>
      <button onClick={() => stop()}>Stop</button>
      <button onClick={() => start()}>Start</button>
    </>
  );
}
```

我们在应用程序中使用了`useInterval`钩子。

它接受一个具有不同属性的对象。

`duration`是每个时期的持续时间。

`startImmediate`意味着如果是`true`，回调将在组件加载时运行。

`callback`是回调运行。

它返回一个数组，用`stop`和`start`函数分别停止和启动定时器。

然后我们在我们的`onClick`处理程序中使用它。

`useMap`钩子允许我们在组件中操作键值对。

例如，我们可以写:

```
import React from "react";
import { useMap } from "react-powerhooks";export default function App() {
  const {
    set: setKey,
    get: getKey,
    has,
    delete: deleteKey,
    clear,
    reset,
    values
  } = useMap({ name: "james", age: 20 }); return (
    <>
      <button onClick={() => setKey("foo", "bar")}>set key</button>
      <button onClick={() => deleteKey()}>delete</button>
      <button onClick={() => clear()}>clear</button>
      <button onClick={() => reset()}>reset</button>
      <div>{JSON.stringify(values)}</div>
    </>
  );
}
```

我们使用了带有一个作为初始值传入的对象的`useMap`钩子。

它返回一个带有添加、获取和移除数据的方法的对象。

`clear`和`reset`清除数据。

`deleteKey`删除数据。

`setKey`设置键值对。

# 反应配方

React Recipes 附带了许多钩子，我们可以用来做各种事情。

要安装它，我们运行:

```
npm i react-recipes --save
```

或者:

```
yarn add react-recipes
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useAdjustColor } from "react-recipes";export default function App() {
  const lightGray = useAdjustColor(0.8, "#000");
  const darkerWhite = useAdjustColor(-0.4, "#fff");
  const blend = useAdjustColor(-0.2, "rgb(20,50,200)", "rgb(300,70,20)");
  const linerBlend = useAdjustColor(
    -0.5,
    "rgb(20,60,200)",
    "rgb(200,60,20)",
    true
  ); return (
    <div>
      <div style={{ background: lightGray, height: "50px", width: "50px" }} />
      <div style={{ background: darkerWhite, height: "50px", width: "50px" }} />
      <div style={{ background: blend, height: "50px", width: "50px" }} />
      <div style={{ background: linerBlend, height: "50px", width: "50px" }} />
    </div>
  );
}
```

我们使用`useAdjustColor`钩子来调整颜色。

第一个参数是-1 和 1 之间的百分比。

正的较浅，负的较深。

第二个和第三个参数是颜色代码字符串。

它可以是十六进制或 RGB 值。

第三个参数是可选的。

第四个参数是库混合，可以是十六进制或 RGB 值。

默认为`false`。

它返回调整颜色后创建的颜色字符串。

它附带了更多我们可以使用的挂钩。

![](img/90bc932337f24315a657067e3383ee97.png)

戴维·皮斯诺伊在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

react-powerhooks 包附带了各种状态管理钩子。

反应食谱有颜色调整和许多其他种类的挂钩。