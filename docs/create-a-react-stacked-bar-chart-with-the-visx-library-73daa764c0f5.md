# 用 Visx 库创建一个反应堆积条形图

> 原文：<https://blog.devgenius.io/create-a-react-stacked-bar-chart-with-the-visx-library-73daa764c0f5?source=collection_archive---------2----------------------->

![](img/6076a84af43646a9d79ec748033b1f0a.png)

[法·巴尔沃萨](https://unsplash.com/@fan11?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将堆积条形图添加到 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/axis @visx/grid @visx/group @visx/legend @visx/mock-data @visx/responsive @visx/scale @visx/shape @visx/tooltip
```

安装软件包。

# 创建图表

我们可以通过添加模块提供的项目来创建图表。

我们使用来自`@visx/mock-data`模块的数据。

要添加堆叠条形图，我们编写:

```
import React from "react";
import { BarStack } from "@visx/shape";
import { Group } from "@visx/group";
import { Grid } from "@visx/grid";
import { AxisBottom } from "@visx/axis";
import cityTemperature from "@visx/mock-data/lib/mocks/cityTemperature";
import { scaleBand, scaleLinear, scaleOrdinal } from "@visx/scale";
import { timeParse, timeFormat } from "d3-time-format";
import { useTooltip, useTooltipInPortal, defaultStyles } from "@visx/tooltip";
import { LegendOrdinal } from "@visx/legend";const purple1 = "#6c5efb";
const purple2 = "#c998ff";
export const purple3 = "#a44afe";
export const background = "#eaedff";
const defaultMargin = { top: 40, right: 0, bottom: 0, left: 0 };
const tooltipStyles = {
  ...defaultStyles,
  minWidth: 60,
  backgroundColor: "rgba(0,0,0,0.9)",
  color: "white"
};const data = cityTemperature.slice(0, 12);
const keys = Object.keys(data[0]).filter((d) => d !== "date");const temperatureTotals = data.reduce((allTotals, currentDate) => {
  const totalTemperature = keys.reduce((dailyTotal, k) => {
    dailyTotal += Number(currentDate[k]);
    return dailyTotal;
  }, 0);
  if (Array.isArray(allTotals)) {
    allTotals.push(totalTemperature);
    return allTotals;
  }
  return [];
});const parseDate = timeParse("%Y-%m-%d");
const format = timeFormat("%b %d");
const formatDate = (date) => format(parseDate(date));const getDate = (d) => d.date;const dateScale = scaleBand({
  domain: data.map(getDate),
  padding: 0.2
});
const temperatureScale = scaleLinear({
  domain: [0, Math.max(...temperatureTotals)],
  nice: true
});
const colorScale = scaleOrdinal({
  domain: keys,
  range: [purple1, purple2, purple3]
});let tooltipTimeout;function Example({ width, height, events = false, margin = defaultMargin }) {
  const {
    tooltipOpen,
    tooltipLeft,
    tooltipTop,
    tooltipData,
    hideTooltip,
    showTooltip
  } = useTooltip(); const { containerRef, TooltipInPortal } = useTooltipInPortal(); if (width < 10) return null;
  const xMax = width;
  const yMax = height - margin.top - 100; dateScale.rangeRound([0, xMax]);
  temperatureScale.range([yMax, 0]); return width < 10 ? null : (
    <div style={{ position: "relative" }}>
      <svg ref={containerRef} width={width} height={height}>
        <rect
          x={0}
          y={0}
          width={width}
          height={height}
          fill={background}
          rx={14}
        />
        <Grid
          top={margin.top}
          left={margin.left}
          xScale={dateScale}
          yScale={temperatureScale}
          width={xMax}
          height={yMax}
          stroke="black"
          strokeOpacity={0.1}
          xOffset={dateScale.bandwidth() / 2}
        />
        <Group top={margin.top}>
          <BarStack
            data={data}
            keys={keys}
            x={getDate}
            xScale={dateScale}
            yScale={temperatureScale}
            color={colorScale}
          >
            {(barStacks) =>
              barStacks.map((barStack) =>
                barStack.bars.map((bar) => (
                  <rect
                    key={`bar-stack-${barStack.index}-${bar.index}`}
                    x={bar.x}
                    y={bar.y}
                    height={bar.height}
                    width={bar.width}
                    fill={bar.color}
                    onClick={() => {
                      if (events) alert(`clicked: ${JSON.stringify(bar)}`);
                    }}
                    onMouseLeave={() => {
                      tooltipTimeout = window.setTimeout(() => {
                        hideTooltip();
                      }, 300);
                    }}
                    onMouseMove={(event) => {
                      if (tooltipTimeout) clearTimeout(tooltipTimeout);
                      const top = event.clientY - margin.top - bar.height;
                      const left = bar.x + bar.width / 2;
                      showTooltip({
                        tooltipData: bar,
                        tooltipTop: top,
                        tooltipLeft: left
                      });
                    }}
                  />
                ))
              )
            }
          </BarStack>
        </Group>
        <AxisBottom
          top={yMax + margin.top}
          scale={dateScale}
          tickFormat={formatDate}
          stroke={purple3}
          tickStroke={purple3}
          tickLabelProps={() => ({
            fill: purple3,
            fontSize: 11,
            textAnchor: "middle"
          })}
        />
      </svg>
      <div
        style={{
          position: "absolute",
          top: margin.top / 2 - 10,
          width: "100%",
          display: "flex",
          justifyContent: "center",
          fontSize: "14px"
        }}
      >
        <LegendOrdinal
          scale={colorScale}
          direction="row"
          labelMargin="0 15px 0 0"
        />
      </div> {tooltipOpen && tooltipData && (
        <TooltipInPortal
          key={Math.random()} // update tooltip bounds each render
          top={tooltipTop}
          left={tooltipLeft}
          style={tooltipStyles}
        >
          <div style={{ color: colorScale(tooltipData.key) }}>
            <strong>{tooltipData.key}</strong>
          </div>
          <div>{tooltipData.bar.data[tooltipData.key]}℉</div>
          <div>
            <small>{formatDate(getDate(tooltipData.bar.data))}</small>
          </div>
        </TooltipInPortal>
      )}
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

我们为条形的颜色创建了`purple`、`purple2`、`purple3`变量。

`background`是图表的背景色。

`tooltipStyles`拥有工具提示的样式。

条形数据被设置为`data`变量的值。

`keys`具有 x 轴刻度的值。

我们通过将每天的温度值相加来计算`temnperatureTotals`。

然后我们为 x 轴刻度创建`dateScale`。

而`temperature`刻度是针对 y 轴刻度的。

我们将`temperatureScale`的最大值设置为`temperatureTotals`的最大值，这是 y 轴的最高值。

`colorScale`有颜色值。

然后我们创建`Example`组件来保存堆叠条形图。

`useTooltip`钩子返回一个具有各种方法和状态的对象，用于创建和设置工具提示值。

然后，我们计算`xMax`和`yMax`值，以创建 x 轴和 y 轴刻度。

接下来，我们添加`svg`元素来将所有的图表部分组合在一起。

`Grid`组件在背景中显示网格。

`Group`组件有`BarStack`组件，该组件有堆叠的条。

堆叠条由`rect`元素创建。

我们有`onMouseMove`处理程序来调用`showTooltip`来显示带有条形值的工具提示。

`onMouseLeave`处理程序让我们在离开工具条时关闭工具提示。

`AxisBottom`组件使用样式和颜色呈现 x 轴。

# 结论

我们可以使用 Visx 提供的模块将堆积条形图添加到 React 应用程序中。