# 顶部反应挂钩—模态和可见性

> 原文：<https://blog.devgenius.io/top-react-hooks-modals-and-visibility-33d2801f3705?source=collection_archive---------10----------------------->

![](img/83ebf2a28f30458325ffb657890e24f3.png)

Todd Quackenbush 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应-挂钩-使用-模态

react-hooks-use-modal 库让我们可以轻松地创建一个模型。

要安装它，我们运行:

```
npm i react-hooks-use-modal
```

然后我们可以通过写来使用它:

```
import React from "react";
import useModal from "react-hooks-use-modal";export default function App() {
  const [Modal, open, close, isOpen] = useModal("root", {
    preventScroll: true
  });
  return (
    <div>
      <p>{isOpen ? "Yes" : "No"}</p>
      <button onClick={open}>OPEN</button>
      <Modal>
        <div>
          <h1>Title</h1>
          <p>hello world.</p>
          <button onClick={close}>CLOSE</button>
        </div>
      </Modal>
    </div>
  );
}
```

我们在一些参数中使用了`useModal`钩子。

第一个参数是模态附加到的元素的 ID。

第二个参数有一些选项。

`preventScroll`设置为`true`时，防止主体滚动。

然后我们可以使用它返回的数组。

`Modal`有情态成分。

`open`是打开模态的功能。

`close`是关闭模态的功能。

`isOpen`具有指示模态是否打开的状态。

# 反应-挂钩-可见

react-hooks-visible 库为我们提供了一个钩子来检测元素的可见性。

它使用 Observer API 来检测元素的可见性。

安装它时，我们运行:

```
yarn add react-hooks-visible
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useVisible } from "react-hooks-visible";export default function App() {
  const [targetRef, visible] = useVisible();
  return (
    <div ref={targetRef}>
      <p style={{ position: "fixed" }}>{visible * 100} % visible</p>
      {Array(1000)
        .fill()
        .map((_, i) => (
          <p>{i}</p>
        ))}
    </div>
  );
}
```

我们使用`useVisible`钩子返回`targetRef`，并将其传递给我们想要观察的元素的`ref`道具。

`visible`返回可见元素的百分比。

它会随着可见性百分比的变化而更新。

我们还可以向钩子传递一个参数来格式化数字。

例如，我们可以写:

```
import React from "react";
import { useVisible } from "react-hooks-visible";export default function App() {
  const [targetRef, visible] = useVisible(vi => (vi * 100).toFixed(2));
  return (
    <div ref={targetRef}>
      <p style={{ position: "fixed" }}>{visible} % visible</p>
      {Array(1000)
        .fill()
        .map((_, i) => (
          <p>{i}</p>
        ))}
    </div>
  );
}
```

将`visible`数字四舍五入到小数点后两位。

我们也可以返回一个数字以外的东西。

例如，我们可以写:

```
import React from "react";
import { useVisible } from "react-hooks-visible";export default function App() {
  const [targetRef, isVisible] = useVisible(vi => vi > 0.5);
  return (
    <div ref={targetRef}>
      <p style={{ position: "fixed" }}>{isVisible.toString()} </p>
      {Array(10)
        .fill()
        .map((_, i) => (
          <p>{i}</p>
        ))}
    </div>
  );
}
```

我们有一个回调函数，它返回一个布尔表达式，将可见性小数与 0.5 进行比较。

`isVisible`是一个布尔值，表示元素是否可见一半以上。

![](img/48303c065504c2bdb61c672cffc686f1.png)

照片由[马腾·范登·霍维尔](https://unsplash.com/@mvdheuvel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

react-hooks-use-modal 库让我们可以轻松地创建一个模型。

react-hooks-visible 让我们观察元素的可见性。