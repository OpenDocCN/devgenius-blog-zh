# 顶部反应挂钩—悬停、切换和标题

> 原文：<https://blog.devgenius.io/top-react-hooks-hover-toggles-and-titles-fc9c0048bdb3?source=collection_archive---------5----------------------->

![](img/9b6ee76310418dbfb4e299c5bd7b1ac0.png)

照片由 [clement fusil](https://unsplash.com/@clementfusil?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 坩埚

熔炉有很多钩子，我们可以在 React 应用程序中使用。

这是一个有许多挂钩的实用程序库。

要安装它，我们运行:

```
npm install @withvoid/melting-pot --save
```

或者:

```
yarn add @withvoid/melting-pot
```

然后我们可以使用`useHover`钩子来观察元素上的悬停。

为了使用它，我们写:

```
import React from "react";
import { useHover } from "@withvoid/melting-pot";export default function App() {
  const { hover, bind } = useHover();
  const styles = {
    emphasis: {
      display: "inline-block",
      backgroundColor: "red",
      color: "white",
      padding: 5,
      width: 55,
      textAlign: "center"
    }
  }; return (
    <div {...bind}>
      <p>Hover me</p>
      <p>
        Status: <span style={styles.emphasis}>{String(hover)}</span>
      </p>
    </div>
  );
}
```

我们将`bind`对象传递给 div，这样当它们被悬停时，我们可以观察 div 中的所有内容。

然后我们可以用`useHover`返回的`hover`值得到悬停状态。

`useKeyPress`钩子让我们观察特定的按键。

要使用它，我们可以写:

```
import React from "react";
import { useKeyPress } from "@withvoid/melting-pot";export default function App() {
  const [isKeyPressed, key] = useKeyPress("a"); return (
    <p>
      {isKeyPressed && "a is pressed"} {key}
    </p>
  );
}
```

我们使用`useKeyPress`钩子来获取按键。

参数是按下的键。

然后我们可以用`isKeyPress`状态得到按下的状态。

`key`拥有被监视的密钥。

`useOnlineStatus`钩子让我们获得应用程序的在线状态。

为了使用它，我们写:

```
import React from "react";
import { useOnlineStatus } from "@withvoid/melting-pot";export default function App() {
  const { online } = useOnlineStatus();
  return (
    <div>
      <p>{online ? "You Are Online" : "You Are Offline"}</p>
    </div>
  );
}
```

然后我们可以使用`useOnlineStatus`钩子来获取状态为的`online`属性。

钩子让我们设置页面的标题。

例如，我们可以这样使用它:

```
import React from "react";
import { useTitle } from "@withvoid/melting-pot";export default function App() {
  const [count, setCount] = React.useState(1);
  useTitle(count);
  const onChange = value => {
    setCount(count => count + value);
  };
  const onIncrement = () => onChange(1);
  const onDecrement = () => onChange(-1); return (
    <div>
      <section>
        <button type="button" onClick={onIncrement}>
          +
        </button>
        <p>Count {count}</p>
        <button type="button" onClick={onDecrement}>
          -
        </button>
      </section>
    </div>
  );
}
```

我们使用`useTitle`钩子通过计数来设置标题。

当我们点击按钮时,`count`被更新。

所以它的最新值将用于设置标题。

`useToggle`钩子让我们观察一个值的切换。

例如，我们可以写:

```
import React from "react";
import { useToggle } from "@withvoid/melting-pot";export default function App() {
  const { on, onToggle } = useToggle(); return (
    <div>
      <button onClick={onToggle}>{on ? "On" : "Off"}</button>
    </div>
  );
}
```

增加`useToggle`挂钩。

它返回一个状态为`on`的对象，该对象具有 toggle 状态。

而`onToggle`功能让我们切换`on`状态。

![](img/75016bdf8162f0f3bd3800c398b7bbc3.png)

[杰森排](https://unsplash.com/@jasonrowphotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们可以使用熔炉库做很多事情，包括观察网络状态、设置标题、观察悬停和切换状态。