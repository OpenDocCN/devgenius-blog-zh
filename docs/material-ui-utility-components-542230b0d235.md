# 材料用户界面-实用组件

> 原文：<https://blog.devgenius.io/material-ui-utility-components-542230b0d235?source=collection_archive---------5----------------------->

![](img/b7e719c6a6f9c2b66287956f41f3b0a3.png)

Todd Quackenbush 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将研究如何添加一个点击式监听器、CSS 重置和带有材料 UI 的门户。

# 单击离开监听器

我们可以添加一个 click away 监听器，当我们在元素外部单击时可以监听它。

这让我们可以很容易地添加菜单之类的东西。

例如，我们可以写:

```
import React from "react";
import ClickAwayListener from "[@material](http://twitter.com/material)-ui/core/ClickAwayListener";export default function App() {
  const [open, setOpen] = React.useState(false);const handleClick = () => {
    setOpen(prev => !prev);
  };const handleClickAway = () => {
    setOpen(false);
  };return (
    <ClickAwayListener onClickAway={handleClickAway}>
      <div>
        <button type="button" onClick={handleClick}>
          menu
        </button>
        {open ? <div>some menu</div> : null}
      </div>
    </ClickAwayListener>
  );
}
```

向我们的应用程序添加菜单。

我们有`ClickAwayListener`到我们的`App`。

然后我们放上当用户从里面点击时我们想要做的东西。

我们有一个 div，当我们单击它的外部时，它将被删除。

`onClickAway`道具有一个功能，当我们点击离开某个东西时它就会运行。

`handleClick`按钮切换`open`按钮来切换菜单。

`handleClickAway`将`open`设置为`false`关闭菜单。

当我们点击按钮时，菜单就会打开。

当我们点击它的外部时，它就会关闭。

# 入口

`Portal`组件类似于 React 门户组件。

它让我们将下拉菜单呈现到 DOM 层次结构之外的新位置。

要使用它，我们可以写:

```
import React from "react";
import ClickAwayListener from "[@material](http://twitter.com/material)-ui/core/ClickAwayListener";
import Portal from "[@material](http://twitter.com/material)-ui/core/Portal";export default function App() {
  const [open, setOpen] = React.useState(false); const handleClick = () => {
    setOpen(prev => !prev);
  }; const handleClickAway = () => {
    setOpen(false);
  }; return (
    <ClickAwayListener onClickAway={handleClickAway}>
      <div>
        <button type="button" onClick={handleClick}>
          menu
        </button>
        {open ? (
          <Portal>
            <div>some menu.</div>
          </Portal>
        ) : null}
      </div>
    </ClickAwayListener>
  );
}
```

我们添加了`Portal`组件，它将在主体中呈现 div，而不是在`ClickAwayListener`中的 div 内。

# 前沿

我们可以配置`ClickAwayListener`来监听像鼠标按下和触摸开始这样的主要事件。

例如，我们可以写:

```
import React from "react";
import ClickAwayListener from "[@material](http://twitter.com/material)-ui/core/ClickAwayListener";
import Portal from "[@material](http://twitter.com/material)-ui/core/Portal";export default function App() {
  const [open, setOpen] = React.useState(false); const handleClick = () => {
    setOpen(prev => !prev);
  }; const handleClickAway = () => {
    setOpen(false);
  }; return (
    <ClickAwayListener
      mouseEvent="onMouseDown"
      touchEvent="onTouchStart"
      onClickAway={handleClickAway}
    >
      <div>
        <button type="button" onClick={handleClick}>
          menu
        </button>
        {open ? <div>some menu.</div> : null}
      </div>
    </ClickAwayListener>
  );
}
```

我们有一个`mouseEvent`和`touchEvent`道具来设置我们收听的事件。

因此，我们监听鼠标按下和触摸开始事件。

# CSS 基线

`CssBaseline`组件提供 CSS 复位。

它提供了由 normalize.css 提供的样式规范化。

它将 CSS 默认设置重置为每个浏览器相同的设置。

例如，我们可以这样使用它:

```
import React from "react";
import CssBaseline from "[@material](http://twitter.com/material)-ui/core/CssBaseline";export default function App() {
  return (
    <>
      <CssBaseline />
      reset
    </>
  );
}
```

`CssBaseline`组件提供了一个全局 CSS 复位。

如果我们想将 CSS 重置的范围扩大到某些组件，我们可以使用`ScopedCssBaseline`组件。

例如，我们可以写:

```
import React from "react";
import ScopedCssBaseline from "[@material](http://twitter.com/material)-ui/core/ScopedCssBaseline";export default function App() {
  return (
    <>
      <ScopedCssBaseline>reset</ScopedCssBaseline>
    </>
  );
}
```

我们将`ScopedCssBaseline`组件包装在想要重置默认值的元素周围。

![](img/b5bac2c867c3ba61b4e3505a7f086d3b.png)

由[大卫·维格](https://unsplash.com/@davidvig?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以使用 click away 监听器监听用户远离元素的点击。

Material UI 提供了 CSS 重置组件，我们可以用它来重置默认值，使它们在任何地方都保持一致。

门户允许我们在 DOM 层次结构中的任何地方呈现项目。