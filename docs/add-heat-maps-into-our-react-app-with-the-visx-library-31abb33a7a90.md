# 使用 Visx 库将热图添加到我们的 React 应用中

> 原文：<https://blog.devgenius.io/add-heat-maps-into-our-react-app-with-the-visx-library-31abb33a7a90?source=collection_archive---------8----------------------->

![](img/34ce600ef3cc42460ea858b95d5aa89d.png)

克里斯蒂安·斯特克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将热图添加到 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/group @visx/heatmap @visx/mock-data @visx/responsive @visx/scale
```

安装软件包。

# 添加热图

我们可以通过编写以下内容来添加热图:

```
import React from "react";
import { Group } from "@visx/group";
import genBins from "@visx/mock-data/lib/generators/genBins";
import { scaleLinear } from "@visx/scale";
import { HeatmapCircle, HeatmapRect } from "@visx/heatmap";const hot1 = "#77312f";
const hot2 = "#f33d15";
const cool1 = "#122549";
const cool2 = "#b4fbde";
const background = "#28272c";const binData = genBins(/* length = */ 16, /* height = */ 16);function max(data, value) {
  return Math.max(...data.map(value));
}function min(data, value) {
  return Math.min(...data.map(value));
}
const bins = (d) => d.bins;
const count = (d) => d.count;const colorMax = max(binData, (d) => max(bins(d), count));
const bucketSizeMax = max(binData, (d) => bins(d).length);// scales
const xScale = scaleLinear({
  domain: [0, binData.length]
});
const yScale = scaleLinear({
  domain: [0, bucketSizeMax]
});
const circleColorScale = scaleLinear({
  range: [hot1, hot2],
  domain: [0, colorMax]
});
const rectColorScale = scaleLinear({
  range: [cool1, cool2],
  domain: [0, colorMax]
});
const opacityScale = scaleLinear({
  range: [0.1, 1],
  domain: [0, colorMax]
});
const defaultMargin = { top: 10, left: 20, right: 20, bottom: 110 };const Example = ({
  width,
  height,
  events = false,
  margin = defaultMargin,
  separation = 20
}) => {
  const size =
    width > margin.left + margin.right
      ? width - margin.left - margin.right - separation
      : width;
  const xMax = size / 2;
  const yMax = height - margin.bottom - margin.top;
  const binWidth = xMax / binData.length;
  const binHeight = yMax / bucketSizeMax;
  const radius = min([binWidth, binHeight], (d) => d) / 2;xScale.range([0, xMax]);
  yScale.range([yMax, 0]);return width < 10 ? null : (
    <svg width={width} height={height}>
      <rect
        x={0}
        y={0}
        width={width}
        height={height}
        rx={14}
        fill={background}
      />
      <Group top={margin.top} left={margin.left}>
        <HeatmapCircle
          data={binData}
          xScale={(d) => xScale(d) ?? 0}
          yScale={(d) => yScale(d) ?? 0}
          colorScale={circleColorScale}
          opacityScale={opacityScale}
          radius={radius}
          gap={2}
        >
          {(heatmap) =>
            heatmap.map((heatmapBins) =>
              heatmapBins.map((bin) => (
                <circle
                  key={`heatmap-circle-${bin.row}-${bin.column}`}
                  className="visx-heatmap-circle"
                  cx={bin.cx}
                  cy={bin.cy}
                  r={bin.r}
                  fill={bin.color}
                  fillOpacity={bin.opacity}
                  onClick={() => {
                    if (!events) return;
                    const { row, column } = bin;
                    alert(JSON.stringify({ row, column, bin: bin.bin }));
                  }}
                />
              ))
            )
          }
        </HeatmapCircle>
      </Group>
      <Group top={margin.top} left={xMax + margin.left + separation}>
        <HeatmapRect
          data={binData}
          xScale={(d) => xScale(d) ?? 0}
          yScale={(d) => yScale(d) ?? 0}
          colorScale={rectColorScale}
          opacityScale={opacityScale}
          binWidth={binWidth}
          binHeight={binWidth}
          gap={2}
        >
          {(heatmap) =>
            heatmap.map((heatmapBins) =>
              heatmapBins.map((bin) => (
                <rect
                  key={`heatmap-rect-${bin.row}-${bin.column}`}
                  className="visx-heatmap-rect"
                  width={bin.width}
                  height={bin.height}
                  x={bin.x}
                  y={bin.y}
                  fill={bin.color}
                  fillOpacity={bin.opacity}
                  onClick={() => {
                    if (!events) return;
                    const { row, column } = bin;
                    alert(JSON.stringify({ row, column, bin: bin.bin }));
                  }}
                />
              ))
            )
          }
        </HeatmapRect>
      </Group>
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

我们在代码的顶部定义了热图的颜色代码。

`binData`有热图的数据。

`max`和`min`返回数据的最大值和最小值。

`bins`和`count`是数据的获取方法。

我们用`colorMax`和`bucketSizeMax`函数得到颜色和桶尺寸的最大值。

接下来，我们计算热图的`xScale`和`yScale`，以获得热图值的范围。

`circleColorScale`和`rectColorScale`是热图圆形或矩形的色标。

`opacityStyle`具有圆形或矩形的不透明度。

在`Example`组件中，我们获得了热图的宽度。

`binWidth`和`binHeight`具有热图中箱子的宽度和高度。

`radius`获取热图中圆圈的半径。

我们将所有变量传递给`HeatmapCircle`组件，以创建一个带圆圈的热图。

而`HeatmapRect`组件做同样的事情来创建一个带有矩形的热图。

这些图块在每个图块的渲染属性中进行渲染。我们根据热图类型渲染`circle`或`rect`。

# 结论

我们可以使用 Visx 库在 React 应用程序中创建包含不同类型图块的热图。