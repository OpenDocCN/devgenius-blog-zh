# 用 Visx 库创建曲线

> 原文：<https://blog.devgenius.io/create-curves-with-the-visx-library-f4abf966bc52?source=collection_archive---------3----------------------->

![](img/ab5a22175e445202e0a62c3990306c02.png)

照片由 [Fabian Quintero](https://unsplash.com/@onefabian?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将曲线添加到 React 应用程序中

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/curve @visx/gradient @visx/group @visx/marker @visx/mock-data @visx/responsive @visx/scale @visx/shape
```

安装软件包。

# 创建曲线

我们可以通过添加模块提供的项目来创建图表。

我们使用来自`@visx/mock-data`模块的数据。

为了创建曲线，我们编写:

```
import React, { useState } from "react";
import { extent, max } from "d3-array";
import * as allCurves from "@visx/curve";
import { Group } from "@visx/group";
import { LinePath } from "@visx/shape";
import { scaleTime, scaleLinear } from "@visx/scale";
import {
  MarkerArrow,
  MarkerCross,
  MarkerX,
  MarkerCircle,
  MarkerLine
} from "@visx/marker";
import generateDateValue from "@visx/mock-data/lib/generators/genDateValue";const curveTypes = Object.keys(allCurves);
const lineCount = 3;
const series = new Array(lineCount)
  .fill(null)
  .map((_) =>
    generateDateValue(25).sort((a, b) => a.date.getTime() - b.date.getTime())
  );
const allData = series.reduce((rec, d) => rec.concat(d), []);const getX = (d) => d.date;
const getY = (d) => d.value;// scales
const xScale = scaleTime({
  domain: extent(allData, getX)
});
const yScale = scaleLinear({
  domain: [0, max(allData, getY)]
});function Example({ width, height, showControls = true }) {
  const [curveType, setCurveType] = useState("curveNatural");
  const [showPoints, setShowPoints] = useState(true);
  const svgHeight = showControls ? height - 40 : height;
  const lineHeight = svgHeight / lineCount; xScale.range([0, width - 50]);
  yScale.range([lineHeight - 2, 0]); return (
    <div className="visx-curves-demo">
      {showControls && (
        <>
          <label>
            Curve type &nbsp;
            <select
              onChange={(e) => setCurveType(e.target.value)}
              value={curveType}
            >
              {curveTypes.map((curve) => (
                <option key={curve} value={curve}>
                  {curve}
                </option>
              ))}
            </select>
          </label>
          &nbsp;
          <label>
            Show points&nbsp;
            <input
              type="checkbox"
              checked={showPoints}
              onChange={() => setShowPoints(!showPoints)}
            />
          </label>
          <br />
        </>
      )}
      <svg width={width} height={svgHeight}>
        <MarkerX
          id="marker-x"
          stroke="#333"
          size={22}
          strokeWidth={4}
          markerUnits="userSpaceOnUse"
        />
        <MarkerCross
          id="marker-cross"
          stroke="#333"
          size={22}
          strokeWidth={4}
          strokeOpacity={0.6}
          markerUnits="userSpaceOnUse"
        />
        <MarkerCircle id="marker-circle" fill="#333" size={2} refX={2} />
        <MarkerArrow
          id="marker-arrow-odd"
          stroke="#333"
          size={8}
          strokeWidth={1}
        />
        <MarkerLine id="marker-line" fill="#333" size={16} strokeWidth={1} />
        <MarkerArrow id="marker-arrow" fill="#333" refX={2} size={6} />
        <rect width={width} height={svgHeight} fill="#efefef" rx={14} ry={14} />
        {width > 8 &&
          series.map((lineData, i) => {
            const even = i % 2 === 0;
            let markerStart = even ? "url(#marker-cross)" : "url(#marker-x)";
            if (i === 1) markerStart = "url(#marker-line)";
            const markerEnd = even
              ? "url(#marker-arrow)"
              : "url(#marker-arrow-odd)";
            return (
              <Group key={`lines-${i}`} top={i * lineHeight} left={13}>
                {showPoints &&
                  lineData.map((d, j) => (
                    <circle
                      key={i + j}
                      r={3}
                      cx={xScale(getX(d))}
                      cy={yScale(getY(d))}
                      stroke="rgba(33,33,33,0.5)"
                      fill="transparent"
                    />
                  ))}
                <LinePath
                  curve={allCurves[curveType]}
                  data={lineData}
                  x={(d) => xScale(getX(d)) ?? 0}
                  y={(d) => yScale(getY(d)) ?? 0}
                  stroke="#333"
                  strokeWidth={even ? 2 : 1}
                  strokeOpacity={even ? 0.6 : 1}
                  shapeRendering="geometricPrecision"
                  markerMid="url(#marker-circle)"
                  markerStart={markerStart}
                  markerEnd={markerEnd}
                />
              </Group>
            );
          })}
      </svg>
      <style jsx>{`
        .visx-curves-demo label {
          font-size: 12px;
        }
      `}</style>
    </div>
  );
}export default function App() {
  return (
    <div className="App">
      <Example width={500} height={300} />
    </div>
  );
}
```

我们从`@visx/curve`库中获取曲线类型。

`curveTypes`是从模块对象的键中生成的。

曲线数据由`@visx/mock-data`模块的`generateDataValue`函数创建。

`getX`和`getY`函数是数据存取函数，分别获取 x 轴和 y 轴的数据。

`xScale`有 x 轴刻度。

`yScale`具有 y 轴刻度。

`Example`组件具有曲线。

我们有一个下拉菜单来设置`curveType`状态。

我们有一个复选框来设置`showPoints`状态。

我们为带有`MarkerX`、`MarkerCross`、`MarkerCircle`、`MarkerLine`和`MarkerArrow`组件的曲线添加标记。

然后为了添加曲线，我们添加了`LinePath`组件。

我们将`curve`道具设置为我们想要渲染的曲线类型。

`data`有行数据。

`x`和`y`分别有要渲染的 x 和 y 值。

`stroke`和`strokeWidth`有笔画样式。

我们用`markerMid`、`markerStart`和`markerEnd`道具设置标记显示在曲线上。

`markerMid`在曲线的中间有标记。

其他的显示在末端。

# 结论

我们可以通过 Visx 库轻松地将曲线添加到 React 应用程序中。