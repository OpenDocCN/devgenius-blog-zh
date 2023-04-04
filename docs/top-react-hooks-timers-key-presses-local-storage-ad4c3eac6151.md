# 顶部反应挂钩—定时器、按键、本地存储

> 原文：<https://blog.devgenius.io/top-react-hooks-timers-key-presses-local-storage-ad4c3eac6151?source=collection_archive---------5----------------------->

![](img/fafe37783dafbc7a6e103e4f81e0dcea.png)

照片由[丹金](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应配方

React Recipes 附带了许多钩子，我们可以用来做各种事情。

钩子让我们定期运行一段代码。

例如，我们可以写:

```
import React from "react";
import { useInterval } from "react-recipes";export default function App() {
  const [count, setCount] = React.useState(0);
  useInterval(
    () => {
      setCount(count => count + 1);
    },
    1000,
    true
  ); return <div>{count}</div>;
}
```

我们有`useInterval`钩子。

回调是在一段时间后运行的。

第二个参数是周期。

第三个是我们是否在装载时运行回调。

我们可以使用`useIsClient`钩子来检查客户端是否加载了 JavaScript。

例如，我们可以写:

```
import React from "react";
import { useIsClient } from "react-recipes";export default function App() {
  const isClient = useIsClient(); return <div>{isClient && "client side rendered"}</div>;
}
```

我们调用`useIsClient`钩子，它返回一个布尔值，如果是`true`，则表明代码是从客户端加载的。

`useKeyPress`钩子允许我们给任何键添加 keydown 和 keyup 监听器。

例如，我们可以写:

```
import React from "react";
import { useKeyPress } from "react-recipes";export default function App() {
  const hPress = useKeyPress("h"); return <div>{hPress && "h key pressed"}</div>;
}
```

我们使用`useKeyPress`钩子，传入一个字符串，其中包含我们想要观察的表单的键。

如果按下“h”键，则`hPress`为`true`。

`useLocalStorage`钩子让我们在本地存储器中设置和存储值。

例如，我们可以写:

```
import React from "react";
import { useLocalStorage } from "react-recipes";export default function App() {
  const [name, setName] = useLocalStorage("name", "james"); return (
    <div>
      <input
        type="text"
        placeholder="Enter your name"
        value={name}
        onChange={e => setName(e.target.value)}
      />
    </div>
  );
}
```

我们使用`useLocalStorage`钩子来存储键和值。

它返回一个带有状态的数组和按此顺序设置状态的函数。

该状态将自动保存在本地存储中。

`useLockBodyScroll`钩子让我们定位滚动。

这对于创建像情态动词这样的东西很方便。

例如，我们可以写:

```
import React from "react";
import { useLockBodyScroll } from "react-recipes";function Modal({ title, children, onClose }) {
  useLockBodyScroll(); return (
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal">
        <h2>{title}</h2>
        <p>{children}</p>
      </div>
    </div>
  );
}export default function App() {
  const [isOpen, setIsOpen] = React.useState(false); return (
    <>
      <button onClick={() => setIsOpen(true)}>open modal</button>
      {isOpen && (
        <Modal title="The title" onClose={() => setIsOpen(false)}>
          modal content
        </Modal>
      )}
    </>
  );
}
```

我们在`Modal`组件中使用不带任何参数的`useLockBodyScroll`钩子。

然后我们可以使用`App`中的`Modal`来打开和关闭它。

![](img/c0bfcf2e7e8ad3633904b10b64955511.png)

照片由[里克·波尔拉斯](https://unsplash.com/@rykporras?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

React Recipes 附带了用于定期运行代码、锁定滚动、设置本地存储和监视按键的钩子。