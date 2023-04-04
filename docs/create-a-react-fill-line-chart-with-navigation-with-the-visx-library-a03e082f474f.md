# 使用 Visx 库创建带导航的 React 填充折线图

> 原文：<https://blog.devgenius.io/create-a-react-fill-line-chart-with-navigation-with-the-visx-library-a03e082f474f?source=collection_archive---------5----------------------->

![](img/46fe1157350cf4a060d049c16aa79316.png)

维多利亚博物馆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加带导航的填充折线图。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/axis @visx/brush @visx/curve @visx/gradient @visx/group @visx/mock-data @visx/pattern @visx/responsive @visx/scale @visx/shape
```

安装软件包。

# 创建图表

我们可以通过添加模块提供的项目来创建图表。

我们使用来自`@visx/mock-data`模块的数据。

然后，为了创建底部带有导航图表的填充折线图，我们编写:

```
import React, { useRef, useState, useMemo } from "react";
import { scaleTime, scaleLinear } from "@visx/scale";
import appleStock from "@visx/mock-data/lib/mocks/appleStock";
import { Brush } from "@visx/brush";
import { PatternLines } from "@visx/pattern";
import { LinearGradient } from "@visx/gradient";
import { max, extent } from "d3-array";
import { AxisBottom, AxisLeft } from "@visx/axis";
import { AreaClosed } from "@visx/shape";
import { Group } from "@visx/group";
import { curveMonotoneX } from "@visx/curve";const stock = appleStock.slice(1000);
const brushMargin = { top: 10, bottom: 15, left: 50, right: 20 };
const chartSeparation = 30;
const PATTERN_ID = "brush_pattern";
const GRADIENT_ID = "brush_gradient";
export const accentColor = "#f6acc8";
export const background = "#584153";
export const background2 = "#af8baf";
const selectedBrushStyle = {
  fill: `url(#${PATTERN_ID})`,
  stroke: "white"
};const getDate = (d) => new Date(d.date);
const getStockValue = (d) => d.close;
const axisColor = "#fff";
const axisBottomTickLabelProps = {
  textAnchor: "middle",
  fontFamily: "Arial",
  fontSize: 10,
  fill: axisColor
};
const axisLeftTickLabelProps = {
  dx: "-0.25em",
  dy: "0.25em",
  fontFamily: "Arial",
  fontSize: 10,
  textAnchor: "end",
  fill: axisColor
};function AreaChart({
  data,
  gradientColor,
  width,
  yMax,
  margin,
  xScale,
  yScale,
  hideBottomAxis = false,
  hideLeftAxis = false,
  top,
  left,
  children
}) {
  if (width < 10) return null;
  return (
    <Group left={left || margin.left} top={top || margin.top}>
      <LinearGradient
        id="gradient"
        from={gradientColor}
        fromOpacity={1}
        to={gradientColor}
        toOpacity={0.2}
      />
      <AreaClosed
        data={data}
        x={(d) => xScale(getDate(d)) || 0}
        y={(d) => yScale(getStockValue(d)) || 0}
        yScale={yScale}
        strokeWidth={1}
        stroke="url(#gradient)"
        fill="url(#gradient)"
        curve={curveMonotoneX}
      />
      {!hideBottomAxis && (
        <AxisBottom
          top={yMax}
          scale={xScale}
          numTicks={width > 520 ? 10 : 5}
          stroke={axisColor}
          tickStroke={axisColor}
          tickLabelProps={() => axisBottomTickLabelProps}
        />
      )}
      {!hideLeftAxis && (
        <AxisLeft
          scale={yScale}
          numTicks={5}
          stroke={axisColor}
          tickStroke={axisColor}
          tickLabelProps={() => axisLeftTickLabelProps}
        />
      )}
      {children}
    </Group>
  );
}function BrushChart({
  compact = false,
  width,
  height,
  margin = {
    top: 20,
    left: 50,
    bottom: 20,
    right: 20
  }
}) {
  const brushRef = useRef(null);
  const [filteredStock, setFilteredStock] = useState(stock); const onBrushChange = (domain) => {
    if (!domain) return;
    const { x0, x1, y0, y1 } = domain;
    const stockCopy = stock.filter((s) => {
      const x = getDate(s).getTime();
      const y = getStockValue(s);
      return x > x0 && x < x1 && y > y0 && y < y1;
    });
    setFilteredStock(stockCopy);
  }; const innerHeight = height - margin.top - margin.bottom;
  const topChartBottomMargin = compact
    ? chartSeparation / 2
    : chartSeparation + 10;
  const topChartHeight = 0.8 * innerHeight - topChartBottomMargin;
  const bottomChartHeight = innerHeight - topChartHeight - chartSeparation;
  const xMax = Math.max(width - margin.left - margin.right, 0);
  const yMax = Math.max(topChartHeight, 0);
  const xBrushMax = Math.max(width - brushMargin.left - brushMargin.right, 0);
  const yBrushMax = Math.max(
    bottomChartHeight - brushMargin.top - brushMargin.bottom,
    0
  );
  const dateScale = useMemo(
    () =>
      scaleTime({
        range: [0, xMax],
        domain: extent(filteredStock, getDate)
      }),
    [xMax, filteredStock]
  );
  const stockScale = useMemo(
    () =>
      scaleLinear({
        range: [yMax, 0],
        domain: [0, max(filteredStock, getStockValue) || 0],
        nice: true
      }),
    [yMax, filteredStock]
  );
  const brushDateScale = useMemo(
    () =>
      scaleTime({
        range: [0, xBrushMax],
        domain: extent(stock, getDate)
      }),
    [xBrushMax]
  );
  const brushStockScale = useMemo(
    () =>
      scaleLinear({
        range: [yBrushMax, 0],
        domain: [0, max(stock, getStockValue) || 0],
        nice: true
      }),
    [yBrushMax]
  ); const initialBrushPosition = useMemo(
    () => ({
      start: { x: brushDateScale(getDate(stock[50])) },
      end: { x: brushDateScale(getDate(stock[100])) }
    }),
    [brushDateScale]
  ); const handleClearClick = () => {
    if (brushRef?.current) {
      setFilteredStock(stock);
      brushRef.current.reset();
    }
  }; const handleResetClick = () => {
    if (brushRef?.current) {
      const updater = (prevBrush) => {
        const newExtent = brushRef.current.getExtent(
          initialBrushPosition.start,
          initialBrushPosition.end
        ); const newState = {
          ...prevBrush,
          start: { y: newExtent.y0, x: newExtent.x0 },
          end: { y: newExtent.y1, x: newExtent.x1 },
          extent: newExtent
        }; return newState;
      };
      brushRef.current.updateBrush(updater);
    }
  }; return (
    <div>
      <svg width={width} height={height}>
        <LinearGradient
          id={GRADIENT_ID}
          from={background}
          to={background2}
          rotate={45}
        />
        <rect
          x={0}
          y={0}
          width={width}
          height={height}
          fill={`url(#${GRADIENT_ID})`}
          rx={14}
        />
        <AreaChart
          hideBottomAxis={compact}
          data={filteredStock}
          width={width}
          margin={{ ...margin, bottom: topChartBottomMargin }}
          yMax={yMax}
          xScale={dateScale}
          yScale={stockScale}
          gradientColor={background2}
        />
        <AreaChart
          hideBottomAxis
          hideLeftAxis
          data={stock}
          width={width}
          yMax={yBrushMax}
          xScale={brushDateScale}
          yScale={brushStockScale}
          margin={brushMargin}
          top={topChartHeight + topChartBottomMargin + margin.top}
          gradientColor={background2}
        >
          <PatternLines
            id={PATTERN_ID}
            height={8}
            width={8}
            stroke={accentColor}
            strokeWidth={1}
            orientation={["diagonal"]}
          />
          <Brush
            xScale={brushDateScale}
            yScale={brushStockScale}
            width={xBrushMax}
            height={yBrushMax}
            margin={brushMargin}
            handleSize={8}
            innerRef={brushRef}
            resizeTriggerAreas={["left", "right"]}
            brushDirection="horizontal"
            initialBrushPosition={initialBrushPosition}
            onChange={onBrushChange}
            onClick={() => setFilteredStock(stock)}
            selectedBoxStyle={selectedBrushStyle}
          />
        </AreaChart>
      </svg>
      <button onClick={handleClearClick}>Clear</button>&nbsp;
      <button onClick={handleResetClick}>Reset</button>
    </div>
  );
}export default function App() {
  return (
    <div className="App">
      <BrushChart width="500" height="300" />
    </div>
  );
}
```

我们使用`appleStock`模拟数据来创建图表。

我们只是从`appleStock`数组中获取前 1000 个条目并渲染它们。

`brushMargin`是图表的页边距。

我们为颜色声明其他变量，比如为填充颜色声明`accentColor`。

`background`和`background2`为渐变色。

`getDate`让我们从 x 轴的数据中获取日期。

`getStockValue`返回 y 轴的值。

然后我们用填充的折线图创建`AreaChart`组件。

我们使用`axisBottomTickLabelProps`对象来设置 x 轴的样式。

我们用`axisLeftTickLabelProps`对 y 轴做同样的操作。

在`AreaChart`组件中，我们添加了带有渐变的`LinearGradient`组件，用于填充折线图。

`AreaClosed`在直线和 x 轴之间填充。

`AxisBottom`有 x 轴。`scale`具有 x 轴刻度。`top`设置 x 轴位置。

`AxisLeft`具有 y 轴。

接下来，我们创建`BrushChart`组件来创建一个图表，让我们通过拖动它来导航填充的折线图。

我们有 th `onBrushChange`函数来改变填充的折线图的位置。

然后我们设置了`innerHeight`、`topChartBottomMargin`等。设置 x 轴和 y 轴的边距、高度和最大值。

我们用`dateScale`、`stockScale`、`brushDateScale`和`brushStockScale`来设置画笔图表的比例。

我们将`initialBrushPosition`设置为一个值来设置笔刷图表的初始位置。

当我们点击清除按钮时，我们也有`handleClearClick`来清除画笔。

`handleResetClick`重置刷子位置。

最后，我们通过将 2 个`AreaCharts`放在一起，将填充的折线图放在画笔图上。

第一个用于主折线图，第二个用于导航。

`Brush`组件让我们拖动第二个折线图来导航第一个折线图。

# 结论

我们可以通过在 React 应用程序中使用画笔添加多个折线图来创建带导航的填充折线图。