# 使用 React Draggable 和 React Grid 布局创建可拖动组件

> 原文：<https://blog.devgenius.io/creating-draggable-components-with-react-draggable-and-react-grid-layout-c0038bf6f1cb?source=collection_archive---------5----------------------->

![](img/f6bf9ec925eeea5043ef38a1a8f36d5b.png)

[食客集体](https://unsplash.com/@eaterscollective?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

React-Draggable 和 React-Grid-Layout 是让我们在 React 应用程序中创建可拖动元素的包。

在本文中，我们将看看如何用它们创建可拖动的元素。

# 创建可拖动元素

首先，我们可以运行以下命令来安装它:

```
npm install react-draggable
```

然后我们可以用它来创建我们的可拖动元素，方法是:

```
import React from "react";
import Draggable from "react-draggable";export default function App() {
  const handleStart = (e, data) => {
    console.log(e, data);
  }; const handleDrag = (e, data) => {
    console.log(e, data);
  }; const handleStop = (e, data) => {
    console.log(e, data);
  }; return (
    <Draggable
      axis="x"
      handle=".handle"
      defaultPosition={{ x: 0, y: 0 }}
      position={null}
      grid={[25, 25]}
      scale={1}
      onStart={handleStart}
      onDrag={handleDrag}
      onStop={handleStop}
    >
      <div>
        <div className="handle">Drag from here</div>
        <div>Drag me.</div>
      </div>
    </Draggable>
  );
}
```

我们使用包中的`Draggable`组件来拖动项目。

`handle`是句柄的类。

`defaultPosition`是屏幕的 x 和 y 坐标

`axis`是我们可以拖动组件的轴。

`x`将运动限制在水平轴上。

`grid`是拖动应该对齐的 x 和 y 坐标。

是我们拖动元素的画布的比例。

`onStart`在拖动开始时被调用。

`onDrag`是拖动时调用的函数。

`onStop`在拖动停止时被调用。

# 创建可拖动的网格反应网格布局

我们可以使用 react-grid-layout 库来创建一个可拖动的网格。

首先，我们通过运行以下命令安装它:

```
npm install react-grid-layout
```

然后我们可以通过写来使用它:

```
import React from "react";
import GridLayout from "react-grid-layout";const layout = [
  { i: "a", x: 0, y: 0, w: 1, h: 2, static: true },
  { i: "b", x: 1, y: 0, w: 3, h: 2, minW: 2, maxW: 4 },
  { i: "c", x: 4, y: 0, w: 1, h: 2 }
];export default function App() {
  return (
    <>
      <GridLayout
        className="layout"
        layout={layout}
        cols={12}
        rowHeight={30}
        width={1200}
      >
        <div key="a">foo</div>
        <div key="b">bar</div>
        <div key="c">baz</div>
      </GridLayout>
    </>
  );
}
```

我们使用`GridLayout`组件来创建布局，

`layout`道具有我们的布局。

`i`是 ID，`x`是网格项的 x 坐标。

`y`是网格项的 y 坐标。

`w`是宽度。

`h`是身高，。

`static`设置为`true`使其固定。

`minW`是最小宽度。

`maxW`是最大宽度。

那么元素里面的`key`道具必须匹配对象的`i`属性。

我们也可以制作一个响应网格。

为此，我们可以使用包装中的`Responsive`组件。

例如，我们可以写:

```
import React from "react";
import { Responsive as ResponsiveGridLayout } from "react-grid-layout";const layouts = {
  xs: [
    { i: "a", x: 0, y: 0, w: 1, h: 2, static: true },
    { i: "b", x: 1, y: 0, w: 1, h: 2, minW: 1, maxW: 4 },
    { i: "c", x: 2, y: 0, w: 1, h: 2 }
  ],
  sm: [
    { i: "a", x: 0, y: 0, w: 1, h: 2, static: true },
    { i: "b", x: 1, y: 0, w: 3, h: 2, minW: 1, maxW: 4 },
    { i: "c", x: 2, y: 0, w: 1, h: 2 }
  ]
};export default function App() {
  return (
    <>
      <ResponsiveGridLayout
        className="layout"
        layouts={layouts}
        cols={{ sm: 12, xs: 12 }}
        breakpoints={{ sm: 768, xs: 480 }}
        rowHeight={30}
        width={window.innerWidth}
      >
        <div key="a">foo</div>
        <div key="b">bar</div>
        <div key="c">baz</div>
      </ResponsiveGridLayout>
    </>
  );
}
```

我们为在`breakpoints`和`cols`道具中定义的`sm`和`xs`断点创建了布局。

我们使用`ResponveGridLayout`组件来创建网格，而不是`GridLayout`。

我们将布局传递给`layouts`道具。

![](img/58460209b4e9dfc2936d6237901d11ee.png)

照片由[左 eris kallergis](https://unsplash.com/@lefterisk?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用 React-Draggable 库创建可拖动的组件。

使用 React-Grid-Layout 库，我们可以创建自己的网格布局。