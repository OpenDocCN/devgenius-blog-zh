# 使用 Visx 库将多边形添加到 React 应用程序中

> 原文：<https://blog.devgenius.io/add-polygons-into-our-react-app-with-the-visx-library-517e13170459?source=collection_archive---------4----------------------->

![](img/4396b8d3ef92fb09838a0496bf961fe6.png)

乔治·帕甘三世在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将多边形添加到 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/gradient @visx/group @visx/responsive @visx/scale @visx/shape
```

安装软件包。

# 添加多边形

为了添加多边形，我们编写:

```
import React from "react";
import { Polygon } from "@visx/shape";
import { Group } from "@visx/group";
import { scaleBand } from "@visx/scale";
import { GradientPinkRed } from "@visx/gradient";export const background = "#7f82e3";
const polygonSize = 25;
const defaultMargin = { top: 10, right: 10, bottom: 10, left: 10 };const polygons = [
  {
    sides: 3,
    fill: "rgb(174, 238, 248)",
    rotate: 90
  },
  {
    sides: 4,
    fill: "lightgreen",
    rotate: 45
  },
  {
    sides: 6,
    fill: "rgb(229, 130, 255)",
    rotate: 0
  },
  {
    sides: 8,
    fill: "url(#polygon-pink)",
    rotate: 0
  }
];const yScale = scaleBand({
  domain: polygons.map((p, i) => i),
  padding: 0.8
});const Example = ({ width, height, margin = defaultMargin }: PolygonProps) => {
  yScale.rangeRound([0, height - margin.top - margin.bottom]);
  const centerX = (width - margin.left - margin.right) / 2;
  return (
    <svg width={width} height={height}>
      <rect width={width} height={height} fill={background} rx={14} />
      <GradientPinkRed id="polygon-pink" />
      {polygons.map((polygon, i) => (
        <Group
          key={`polygon-${i}`}
          top={(yScale(i) || 0) + polygonSize / 2}
          left={margin.left + centerX}
        >
          <Polygon
            sides={polygon.sides}
            size={polygonSize}
            fill={polygon.fill}
            rotate={polygon.rotate}
          />
        </Group>
      ))}
    </svg>
  );
};export default function App() {
  return (
    <div className="App">
      <Example width={500} height={300} />
    </div>
  );
}
```

`background`变量具有背景的颜色。

`polygonSize`是每个多边形的大小，以像素为单位。

我们用多边形的属性创建了`polygons`数组。

我们用`sides`设定边数。

`fill`是多边形的背景。

`rotate`是旋转它的度数。

在`Example`组件中，我们用`polygons.map`回调函数将多边形添加到屏幕上。

我们添加带有道具的`Polygon`组件来设置侧面、填充和旋转角度。

# 结论

我们可以使用 Visx 库将多边形添加到 React 应用程序中。