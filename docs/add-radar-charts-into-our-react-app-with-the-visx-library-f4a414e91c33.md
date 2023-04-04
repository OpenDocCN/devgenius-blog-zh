# 使用 Visx 库将雷达图添加到 React 应用程序中

> 原文：<https://blog.devgenius.io/add-radar-charts-into-our-react-app-with-the-visx-library-f4a414e91c33?source=collection_archive---------6----------------------->

![](img/a1581ef51a750b82da693e851de2712e.png)

汉斯·维维克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将雷达图添加到 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/group @visx/mock-data @visx/responsive @visx/point @visx/scale @visx/shape
```

安装软件包。

# 雷达图

为了创建雷达图，我们写下:

```
import React from "react";
import { Group } from "@visx/group";
import letterFrequency from "@visx/mock-data/lib/mocks/letterFrequency";
import { scaleLinear } from "@visx/scale";
import { Point } from "@visx/point";
import { Line, LineRadial } from "@visx/shape";const orange = "#ff9933";
const pumpkin = "#f5810c";
const silver = "#d9d9d9";
const background = "#FAF7E9";const degrees = 360;
const data = letterFrequency.slice(2, 12);const y = (d) => d.frequency;const genAngles = (length) =>
  [...new Array(length + 1)].map((_, i) => ({
    angle: i * (degrees / length)
  }));const genPoints = (length, radius) => {
  const step = (Math.PI * 2) / length;
  return [...new Array(length)].map((_, i) => ({
    x: radius * Math.sin(i * step),
    y: radius * Math.cos(i * step)
  }));
};function genPolygonPoints(dataArray, scale, getValue) {
  const step = (Math.PI * 2) / dataArray.length;
  const points = new Array(dataArray.length).fill({
    x: 0,
    y: 0
  });
  const pointString = new Array(dataArray.length + 1)
    .fill("")
    .reduce((res, _, i) => {
      if (i > dataArray.length) return res;
      const xVal = scale(getValue(dataArray[i - 1])) * Math.sin(i * step);
      const yVal = scale(getValue(dataArray[i - 1])) * Math.cos(i * step);
      points[i - 1] = { x: xVal, y: yVal };
      res += `${xVal},${yVal} `;
      return res;
    }); return { points, pointString };
}const defaultMargin = { top: 40, left: 80, right: 80, bottom: 80 };function Example({ width, height, levels = 5, margin = defaultMargin }) {
  const xMax = width - margin.left - margin.right;
  const yMax = height - margin.top - margin.bottom;
  const radius = Math.min(xMax, yMax) / 2;const radialScale = scaleLinear({
    range: [0, Math.PI * 2],
    domain: [degrees, 0]
  });const yScale = scaleLinear({
    range: [0, radius],
    domain: [0, Math.max(...data.map(y))]
  });const webs = genAngles(data.length);
  const points = genPoints(data.length, radius);
  const polygonPoints = genPolygonPoints(data, (d) => yScale(d) ?? 0, y);
  const zeroPoint = new Point({ x: 0, y: 0 }); return width < 10 ? null : (
    <svg width={width} height={height}>
      <rect fill={background} width={width} height={height} rx={14} />
      <Group top={height / 2 - margin.top} left={width / 2}>
        {[...new Array(levels)].map((_, i) => (
          <LineRadial
            key={`web-${i}`}
            data={webs}
            angle={(d) => radialScale(d.angle) ?? 0}
            radius={((i + 1) * radius) / levels}
            fill="none"
            stroke={silver}
            strokeWidth={2}
            strokeOpacity={0.8}
            strokeLinecap="round"
          />
        ))}
        {[...new Array(data.length)].map((_, i) => (
          <Line
            key={`radar-line-${i}`}
            from={zeroPoint}
            to={points[i]}
            stroke={silver}
          />
        ))}
        <polygon
          points={polygonPoints.pointString}
          fill={orange}
          fillOpacity={0.3}
          stroke={orange}
          strokeWidth={1}
        />
        {polygonPoints.points.map((point, i) => (
          <circle
            key={`radar-point-${i}`}
            cx={point.x}
            cy={point.y}
            r={4}
            fill={pumpkin}
          />
        ))}
      </Group>
    </svg>
  );
}export default function App() {
  return (
    <div className="App">
      <Example width={500} height={300} />
    </div>
  );
}
```

我们用图表的颜色作为值来定义变量。

`degrees`有雷达图的度数。

`data`有图表的数据。

`y`是一个让我们得到 y 轴值的 getter。

`genAngles`让我们为图表创建角度，`genPoints`创建点。

`genPolygonPoints`生成雷达图的点数。

然后，为了创建雷达图，我们添加了`LineRadial`组件来为雷达图添加 web。

我们从`radialScale`函数中得到角度。

`radius`是从`radius`道具计算出来的。

用`Line`组件创建图形的线段。

`polygon`让我们为雷达图创建多边形填充。

`polygonPoints.map`回调中的`circle`创建位于角度的点。

# 结论

我们可以通过 Visx 库轻松地将雷达图添加到 React 应用程序中。