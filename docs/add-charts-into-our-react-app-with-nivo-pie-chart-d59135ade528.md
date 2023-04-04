# 使用 Nivo 将图表添加到我们的 React 应用程序中—饼图

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-nivo-pie-chart-d59135ade528?source=collection_archive---------4----------------------->

![](img/e2e34071600bab3eaf7b3b7549f6ec0c.png)

照片由[泰西亚·谢斯托帕尔](https://unsplash.com/@taisiia_shestopal?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Nivo 将图表添加到 React 应用程序中。

# 圆形分格统计图表

Nivo 附带代码，允许我们在 React 应用程序中添加饼状图。

为了安装所需的软件包，我们运行:

```
npm i @nivo/pie
```

然后我们可以通过编写以下内容来添加图表:

```
import React from "react";
import { ResponsivePie } from "@nivo/pie";const data = [
  {
    id: "c",
    label: "c",
    value: 120,
    color: "hsl(271, 70%, 50%)"
  },
  {
    id: "stylus",
    label: "stylus",
    value: 174,
    color: "hsl(292, 70%, 50%)"
  },
  {
    id: "go",
    label: "go",
    value: 225,
    color: "hsl(47, 70%, 50%)"
  }
];const MyResponsivePie = ({ data }) => (
  <ResponsivePie
    data={data}
    margin={{ top: 40, right: 80, bottom: 80, left: 80 }}
    innerRadius={0.5}
    padAngle={0.7}
    cornerRadius={3}
    colors={{ scheme: "nivo" }}
    borderWidth={1}
    borderColor={{ from: "color", modifiers: [["darker", 0.2]] }}
    radialLabelsSkipAngle={10}
    radialLabelsTextColor="#333333"
    radialLabelsLinkColor={{ from: "color" }}
    sliceLabelsSkipAngle={10}
    sliceLabelsTextColor="#333333"
    defs={[
      {
        id: "dots",
        type: "patternDots",
        background: "inherit",
        color: "rgba(255, 255, 255, 0.3)",
        size: 4,
        padding: 1,
        stagger: true
      },
      {
        id: "lines",
        type: "patternLines",
        background: "inherit",
        color: "rgba(255, 255, 255, 0.3)",
        rotation: -45,
        lineWidth: 6,
        spacing: 10
      }
    ]}
    fill={[
      {
        match: {
          id: "ruby"
        },
        id: "dots"
      },
      {
        match: {
          id: "c"
        },
        id: "dots"
      },
      {
        match: {
          id: "go"
        },
        id: "dots"
      },
      {
        match: {
          id: "python"
        },
        id: "dots"
      },
      {
        match: {
          id: "scala"
        },
        id: "lines"
      },
      {
        match: {
          id: "lisp"
        },
        id: "lines"
      },
      {
        match: {
          id: "elixir"
        },
        id: "lines"
      },
      {
        match: {
          id: "javascript"
        },
        id: "lines"
      }
    ]}
    legends={[
      {
        anchor: "bottom",
        direction: "row",
        justify: false,
        translateX: 0,
        translateY: 56,
        itemsSpacing: 0,
        itemWidth: 100,
        itemHeight: 18,
        itemTextColor: "#999",
        itemDirection: "left-to-right",
        itemOpacity: 1,
        symbolSize: 18,
        symbolShape: "circle",
        effects: [
          {
            on: "hover",
            style: {
              itemTextColor: "#000"
            }
          }
        ]
      }
    ]}
  />
);export default function App() {
  return (
    <div style={{ width: 400, height: 300 }}>
      <MyResponsivePie data={data} />
    </div>
  );
}
```

我们用饼图数据定义了`data`数组。

`label`有饼图块标签。

`value`有饼图块大小值。

然后将`ResponsivePie`组件添加到我们的图表中。

`margin`有边际。

`data`有饼状图数据。

`innerRadius`具有内径尺寸。

`colors`拥有饼图切片的配色方案。

`borderWidth`有边框宽度。

`borderColor`有边框配色方案。

`radialLabelsSkipAngle`定义标签隐藏时的角度阈值。

如果它小于给定的大小，那么标签是隐藏的。

`radialLabelsTextColor`具有饼图的文本颜色。

`radialLabelsLinkColor`有标签链接颜色。

`sliceLabelsSkipAngle`具有跳过渲染切片标签的角度阈值。

如果它小于给定的阈值，那么切片标签被隐藏。

`sliceLabelsTextColor`有文字颜色。

`defs`有切片的图形定义。

`color`有花纹颜色。

`type`有模式类型。

`fill`具有切片和图例的填充颜色。

`legends`有图例设置。

`itemSpacing`、`itemWidth`、`itemHeight`、`itemTextColor`和`itemDirection`具有项目尺寸、间距和颜色。

`effects`将鼠标悬停在图例项上时会产生效果。

然后在`App`中，我们设置宽度和高度来渲染饼状图。

# 结论

我们可以使用 Nivo 轻松地将饼图添加到 React 应用程序中。