# 使用 Visx 库为直线和曲线添加注释

> 原文：<https://blog.devgenius.io/add-annotations-to-lines-and-curves-with-the-visx-library-cf8ce57886c5?source=collection_archive---------3----------------------->

![](img/242cde2d29ac0fc2b6144ce6bb13e2c3.png)

本·赫尔希在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将曲线添加到 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/annotation @visx/mock-data @visx/responsive @visx/scale @visx/shape
```

安装软件包。

# 创建带注释的直线/曲线

我们可以通过添加模块提供的项目来创建图表。

我们使用来自`@visx/mock-data`模块的数据。

为了创建它，我们写道:

```
import React, { useMemo, useState } from "react";
import { Label, Connector, CircleSubject, LineSubject } from "@visx/annotation";
import { LinePath } from "@visx/shape";
import { bisector, extent } from "d3-array";
import { scaleLinear, scaleTime } from "@visx/scale";
import appleStock from "@visx/mock-data/lib/mocks/appleStock";
import { Annotation } from "@visx/annotation";export const orange = "#ff7e67";
export const greens = ["#ecf4f3", "#68b0ab", "#006a71"];
const data = appleStock.slice(-100);
const getDate = (d) => new Date(d.date).valueOf();
const getStockValue = (d) => d.close;
const annotateDatum = data[Math.floor(data.length / 2) + 4];
const approxTooltipHeight = 70;function findNearestDatum({ value, scale, accessor, data }) {
  const bisect = bisector(accessor).left;
  const nearestValue = scale.invert(value);
  const nearestValueIndex = bisect(data, nearestValue, 1);
  const d0 = data[nearestValueIndex - 1];
  const d1 = data[nearestValueIndex];
  let nearestDatum = d0;
  if (d1 && accessor(d1)) {
    nearestDatum =
      nearestValue - accessor(d0) > accessor(d1) - nearestValue ? d1 : d0;
  }
  return nearestDatum;
}function Example({ width, height, compact = false }) {
  const xScale = useMemo(
    () =>
      scaleTime({
        domain: extent(data, (d) => getDate(d)),
        range: [0, width]
      }),
    [width]
  );
  const yScale = useMemo(
    () =>
      scaleLinear({
        domain: extent(data, (d) => getStockValue(d)),
        range: [height - 100, 100]
      }),
    [height]
  ); const [editLabelPosition] = useState(false);
  const [editSubjectPosition] = useState(false);
  const [title] = useState("Title");
  const [subtitle] = useState(
    compact ? "Subtitle" : "Long Subtitle"
  );
  const [connectorType] = useState("elbow");
  const [subjectType] = useState("circle");
  const [showAnchorLine] = useState(true);
  const [verticalAnchor] = useState("auto");
  const [horizontalAnchor] = useState("auto");
  const [labelWidth] = useState(compact ? 100 : 175);
  const [annotationPosition, setAnnotationPosition] = useState({
    x: xScale(getDate(annotateDatum)) ?? 0,
    y: yScale(getStockValue(annotateDatum)) ?? 0,
    dx: compact ? -50 : -100,
    dy: compact ? -30 : -50
  }); return (
    <svg width={width} height={height}>
      <rect width={width} height={height} fill={greens[0]} />
      <LinePath
        stroke={greens[2]}
        strokeWidth={2}
        data={data}
        x={(d) => xScale(getDate(d)) ?? 0}
        y={(d) => yScale(getStockValue(d)) ?? 0}
      />
      <Annotation
        width={width}
        height={height}
        x={annotationPosition.x}
        y={annotationPosition.y}
        dx={annotationPosition.dx}
        dy={annotationPosition.dy}
        canEditLabel={editLabelPosition}
        canEditSubject={editSubjectPosition}
        onDragEnd={({ event, ...nextPosition }) => {
          const nearestDatum = findNearestDatum({
            accessor:
              subjectType === "horizontal-line" ? getStockValue : getDate,
            data,
            scale: subjectType === "horizontal-line" ? yScale : xScale,
            value:
              subjectType === "horizontal-line"
                ? nextPosition.y
                : nextPosition.x
          });
          const x = xScale(getDate(nearestDatum)) ?? 0;
          const y = yScale(getStockValue(nearestDatum)) ?? 0; const shouldFlipDx =
            (nextPosition.dx > 0 && x + nextPosition.dx + labelWidth > width) ||
            (nextPosition.dx < 0 && x + nextPosition.dx - labelWidth <= 0);
          const shouldFlipDy =
            (nextPosition.dy > 0 &&
              height - (y + nextPosition.dy) < approxTooltipHeight) ||
            (nextPosition.dy < 0 &&
              y + nextPosition.dy - approxTooltipHeight <= 0);
          setAnnotationPosition({
            x,
            y,
            dx: (shouldFlipDx ? -1 : 1) * nextPosition.dx,
            dy: (shouldFlipDy ? -1 : 1) * nextPosition.dy
          });
        }}
      >
        <Connector stroke={orange} type={connectorType} />
        <Label
          backgroundFill="white"
          showAnchorLine={showAnchorLine}
          anchorLineStroke={greens[2]}
          backgroundProps={{ stroke: greens[1] }}
          fontColor={greens[2]}
          horizontalAnchor={horizontalAnchor}
          subtitle={subtitle}
          title={title}
          verticalAnchor={verticalAnchor}
          width={labelWidth}
        />
        {subjectType === "circle" && <CircleSubject stroke={orange} />}
        {subjectType !== "circle" && (
          <LineSubject
            orientation={
              subjectType === "vertical-line" ? "vertical" : "horizontal"
            }
            stroke={orange}
            min={0}
            max={subjectType === "vertical-line" ? height : width}
          />
        )}
      </Annotation>
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

我们创建了`orange`和`green`变量来分别设置标记和线条的颜色。

`data`有该行的数据。

`getDate`和`getStockValue`分别是 x 轴和 y 轴数据的获取函数。

`findNearestDatum`功能让我们找到将注释框拖动到的点。

在`Example`组件中，我们创建了`xScale`和`yScale`对象来创建折线图的刻度。

`Annotation`组件有该行的注释。

我们设置`x`和`y`位置来放置线的注释。

`width`和`height`设置宽度和高度。

`dx`和`dy`设置位置偏移。

`canEditLabel`让我们设置是否可以用布尔值编辑标签。

`canEditSubject`让我们设置是否可以用布尔值编辑主题。

当我们拖动注释框时,`onDragEnd`处理程序改变注释位置。

我们添加`Connection`和`Label`来为注释框添加标记和标签。

现在我们应该看到一个框显示在该行上，并带有由`subtitle`道具设置的文本。

# 结论

我们可以用 Visx 库在我们的代码行中添加一个注释框。