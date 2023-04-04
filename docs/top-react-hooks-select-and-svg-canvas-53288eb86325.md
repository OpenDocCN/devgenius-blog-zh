# 顶部反应挂钩—选择和 SVG 画布

> 原文：<https://blog.devgenius.io/top-react-hooks-select-and-svg-canvas-53288eb86325?source=collection_archive---------0----------------------->

![](img/866939b29c48bbd7b80739ec623d39fd.png)

安德烈·费塔多在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应钩子库

React Hooks Lib 是一个库，有许多可重用的 React Hooks。

要安装它，我们可以运行:

```
npm i react-hooks-lib --save
```

然后我们可以使用库附带的钩子。

我们可以使用`useField`属性将选择的值绑定到一个状态。

例如，我们可以写:

```
import React from "react";
import { useField } from "react-hooks-lib";export default function App() {
  const { value, bind } = useField("apple"); return (
    <div>
      <select {...bind}>
        <option value="apple">apple</option>
        <option value="orange">orange</option>
        <option value="grape">grape</option>
      </select>
      <p>{value}</p>
    </div>
  );
}
```

我们有一个带有选择元素初始值的`useField`钩子。

`bind`是一个用`value`和`onChange`处理程序自动绑定到选择框的对象。

`value`具有从下拉列表中选择的值。

它附带了许多其他钩子来操作其他类型的状态，比如数组，并且还监视 DOM 元素的变化。

此外，它有钩子来模仿 React 类组件的生命周期方法。

# react-hooks-svgdrawing

react-hooks-svgdrawing 库让我们可以轻松地在应用程序中创建一个绘图区域。

我们可以通过运行以下命令来安装它:

```
yarn add react react-hooks-svgdrawing
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useSvgDrawing } from "react-hooks-svgdrawing";export default function App() {
  const [renderRef, draw] = useSvgDrawing();
  return <div style={{ width: 500, height: 500 }} ref={renderRef} />;
}
```

然后将`renderRef`作为`ref`属性的值传递给 div，使 div 成为一个绘图区域。

钩子可以带不同选项的对象。

例如，我们可以写:

```
import React from "react";
import { useSvgDrawing } from "react-hooks-svgdrawing";export default function App() {
  const [renderRef, draw] = useSvgDrawing({
    penWidth: 10,
    penColor: "red",
    close: true,
    curve: false,
    delay: 60,
    fill: ""
  });
  return <div style={{ width: 500, height: 500 }} ref={renderRef} />;
}
```

`penWidth`有笔的宽度。

`penColor`有笔的颜色。

`close`设置为`true`表示绘图路径将被关闭。

`curve`对路径使用曲线命令。

`delay`是在绘制点之前等待的毫秒数。

`fill`有路径的填充。

它有各种我们可以使用的方法。

它们是`draw`方法的一部分。

例如，我们可以写:

```
import React from "react";
import { useSvgDrawing } from "react-hooks-svgdrawing";export default function App() {
  const [renderRef, draw] = useSvgDrawing();
  return (
    <>
      <button onClick={() => draw.download("png")}>download</button>
      <button onClick={() => draw.undo()}>undo</button>
      <button onClick={() => draw.changePenColor("red")}>changePenColor</button>
      <button onClick={() => draw.changePenWidth(10)}>changePenWidth</button>
      <button onClick={() => draw.changeFill("green")}>changeFill</button>
      <button onClick={() => draw.changeDelay(10)}>changeDelay</button>
      <button onClick={() => draw.changeCurve(false)}>changeCurve</button>
      <button onClick={() => draw.changeClose(true)}>changeClose</button>
      <div style={{ width: 500, height: 500 }} ref={renderRef} />
    </>
  );
}
```

来调用`draw`的各种方法。

`download`下载给定格式的图纸。

`undo`撤销最后一次更改。

`changePenColor`改变笔的颜色。

`changePenWidth`改变笔的宽度。

`changeFill`改变钢笔的填充。

`changeDelay`改变绘制圆点的延迟。

`changeCurve`设置 SVG 路径元素是否使用弯逗号。

`chnageClose`设置是否关闭路径。

我们可以使用`getSvgXML`方法来获取 SVG 代码。

![](img/9073ad5131a72f8636d17010f79f4abb.png)

安德烈·费塔多在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

React 钩子库有`useField`和许多其他有用的钩子。

react-hooks-svgdrawing 为我们提供了一个可配置的绘图区域。

我们可以下载绘图并从中获得 SVG 代码。