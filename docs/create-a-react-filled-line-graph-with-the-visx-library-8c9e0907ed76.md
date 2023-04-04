# 用 Visx 库创建一个反应填充线图

> 原文：<https://blog.devgenius.io/create-a-react-filled-line-graph-with-the-visx-library-8c9e0907ed76?source=collection_archive---------3----------------------->

![](img/776d2948687c4fd2fc74d945da9f1028.png)

[Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将填充线图添加到 React 应用程序中。

# 入门指南

首先，我们必须安装 Visx 提供的几个模块。,

为了安装条形图所需的，我们运行:

```
$ npm install --save @visx/axis @visx/curve @visx/gradient @visx/grid @visx/group @visx/mock-data @visx/react-spring @visx/responsive @visx/shape @visx/scale
```

`@visx/mock-data`库有模拟数据，我们可以在条形图中使用。

# 添加填充线图

要添加填充线图，我们写:

```
import React, { useState, useMemo } from "react";
import AreaClosed from "@visx/shape/lib/shapes/AreaClosed";
import { curveMonotoneX } from "@visx/curve";
import {
  scaleUtc,
  scaleLinear,
  scaleLog,
  scaleBand,
  coerceNumber
} from "@visx/scale";
import { Orientation } from "@visx/axis";
import {
  AnimatedAxis,
  AnimatedGridRows,
  AnimatedGridColumns
} from "@visx/react-spring";
import { LinearGradient } from "@visx/gradient";
import { timeFormat } from "d3-time-format";export const backgroundColor = "#da7cff";
const axisColor = "#fff";
const tickLabelColor = "#fff";
export const labelColor = "#340098";
const gridColor = "#6e0fca";
const margin = {
  top: 40,
  right: 150,
  bottom: 20,
  left: 50
};const tickLabelProps = () => ({
  fill: tickLabelColor,
  fontSize: 12,
  fontFamily: "sans-serif",
  textAnchor: "middle"
});const getMinMax = (vals) => {
  const numericVals = vals.map(coerceNumber);
  return [Math.min(...numericVals), Math.max(...numericVals)];
};function Example({
  width: outerWidth = 800,
  height: outerHeight = 800,
  showControls = true
}) {
  const width = outerWidth - margin.left - margin.right;
  const height = outerHeight - margin.top - margin.bottom;
  const [dataToggle] = useState(true);
  const [animationTrajectory] = useState("center"); const axes = useMemo(() => {
    const linearValues = dataToggle ? [0, 2, 4, 6, 8, 10] : [6, 8, 10, 12]; return [
      {
        scale: scaleLinear({
          domain: getMinMax(linearValues),
          range: [0, width]
        }),
        values: linearValues,
        tickFormat: (v, index, ticks) =>
          index === 0
            ? "first"
            : index === ticks[ticks.length - 1].index
            ? "last"
            : `${v}`,
        label: "linear"
      }
    ];
  }, [dataToggle, width]); if (width < 10) return null; const scalePadding = 40;
  const scaleHeight = height / axes.length - scalePadding; const yScale = scaleLinear({
    domain: [100, 0],
    range: [scaleHeight, 0]
  }); return (
    <>
      <svg width={outerWidth} height={outerHeight}>
        <LinearGradient
          id="visx-axis-gradient"
          from={backgroundColor}
          to={backgroundColor}
          toOpacity={0.5}
        />
        <rect
          x={0}
          y={0}
          width={outerWidth}
          height={outerHeight}
          fill={"url(#visx-axis-gradient)"}
          rx={14}
        />
        <g transform={`translate(${margin.left},${margin.top})`}>
          {axes.map(({ scale, values, label, tickFormat }, i) => (
            <g
              key={`scale-${i}`}
              transform={`translate(0, ${i * (scaleHeight + scalePadding)})`}
            >
              <AnimatedGridRows
                key={`gridrows-${animationTrajectory}`}
                scale={yScale}
                stroke={gridColor}
                width={width}
                numTicks={dataToggle ? 1 : 3}
                animationTrajectory={animationTrajectory}
              />
              <AnimatedGridColumns
                key={`gridcolumns-${animationTrajectory}`}
                scale={scale}
                stroke={gridColor}
                height={scaleHeight}
                numTicks={dataToggle ? 5 : 2}
                animationTrajectory={animationTrajectory}
              />
              <AreaClosed
                data={values.map((x) => [
                  (scale(x) ?? 0) +
                    ("bandwidth" in scale &&
                    typeof scale.bandwidth !== "undefined"
                      ? scale.bandwidth() / 2
                      : 0),
                  yScale(10 + Math.random() * 90)
                ])}
                yScale={yScale}
                curve={curveMonotoneX}
                fill={gridColor}
                fillOpacity={0.2}
              />
              <AnimatedAxis
                key={`axis-${animationTrajectory}`}
                orientation={Orientation.bottom}
                top={scaleHeight}
                scale={scale}
                tickFormat={tickFormat}
                stroke={axisColor}
                tickStroke={axisColor}
                tickLabelProps={tickLabelProps}
                tickValues={
                  label === "log" || label === "time" ? undefined : values
                }
                numTicks={label === "time" ? 6 : undefined}
                label={label}
                labelProps={{
                  x: width + 30,
                  y: -10,
                  fill: labelColor,
                  fontSize: 18,
                  strokeWidth: 0,
                  stroke: "#fff",
                  paintOrder: "stroke",
                  fontFamily: "sans-serif",
                  textAnchor: "start"
                }}
                animationTrajectory={animationTrajectory}
              />
            </g>
          ))}
        </g>
      </svg>
    </>
  );
}export default function App() {
  return (
    <div>
      <Example width="500" height="300" />
    </div>
  );
}
```

我们添加了`backgroundColor`来设置背景颜色。

`axisColor`有轴色。

`tickLabelColor`具有 x 轴刻度标签的颜色。

`labelColor`具有 y 轴标签颜色。

`gridColor`有网格颜色。

我们用`tickLabelProps`功能改变文本样式。

为了添加图表内容，我们添加了`axes`数组。

我们从`dataToggle`对象计算图形的值。

`linearValues`有线性刻度值。

我们用`yScale`对象来添加 y 轴值。

在我们返回的 JSX 中，我们添加了 SVG，`rect` rectangle 元素，然后在`axes.map`回调中，我们返回图形。

图中有一个包含 SVG 组的`g`元素。

`AnimatedGridRows`具有动画网格行。

我们将`yScale`值传递给它来显示 y 值。

`stroke`设定冲程。

`width`设置宽度。

`AnimatedGridColumns`有网格列。

`numTicks`让我们设置 x 轴上的刻度数。

我们用类似的代码用`AnimatedGridColumns`组件来添加列。

`AreaClosed`有填充线图。

`AnimatedAxis`具有动画 x 轴。

我们用`labelProps`设置标签样式。

`tickValues`有刻度值。

# 结论

我们可以使用 Visx 在 React 应用程序中添加填充线图。