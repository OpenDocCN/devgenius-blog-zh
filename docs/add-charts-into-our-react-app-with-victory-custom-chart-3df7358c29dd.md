# 使用 Victory 将图表添加到我们的 React 应用程序中—自定义图表

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-custom-chart-3df7358c29dd?source=collection_archive---------1----------------------->

![](img/51764be142843828e12295261356aee9.png)

照片由[普里西拉·杜·普里兹](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 自定义图表

我们可以通过一起添加多个组件来创建自定义图表。

例如，我们可以写:

```
import React from "react";
import { VictoryAxis, VictoryLabel, VictoryLine } from "victory";const getDataSetOne = () => {
  return [
    { x: new Date(2000, 1, 1), y: 12 },
    { x: new Date(2001, 6, 1), y: 10 },
    { x: new Date(2002, 12, 1), y: 11 },
    { x: new Date(2003, 1, 1), y: 5 },
    { x: new Date(2004, 1, 1), y: 4 }
  ];
};const getDataSetTwo = () => {
  return [
    { x: new Date(2000, 1, 1), y: 5 },
    { x: new Date(2001, 1, 1), y: 6 },
    { x: new Date(2002, 1, 1), y: 4 },
    { x: new Date(2003, 1, 1), y: 10 },
    { x: new Date(2004, 1, 1), y: 12 }
  ];
};const getTickValues = () => {
  return [
    new Date(2000, 1, 1),
    new Date(2001, 1, 1),
    new Date(2002, 1, 1),
    new Date(2003, 1, 1),
    new Date(2004, 1, 1)
  ];
};const dataSetOne = getDataSetOne();
const dataSetTwo = getDataSetTwo();
const tickValues = getTickValues();export default function App() {
  return (
    <svg viewBox="0 0 450 350">
      <VictoryLabel
        x={25}
        y={55}
        text={"Economy \n % change on a year earlier"}
      />
      <VictoryLabel x={200} y={55} text={"Dinosaur exports\n $bn"} /><g transform={"translate(0, 40)"}>
        <VictoryAxis
          scale="time"
          standalone={false}
          tickValues={tickValues}
          tickFormat={(x) => x.getFullYear()}
        />
        <VictoryAxis
          dependentAxis
          domain={[-10, 15]}
          offsetX={50}
          orientation="left"
          standalone={false}
        />
        <VictoryLine
          data={[
            { x: new Date(2000, 1, 1), y: 0 },
            { x: new Date(2004, 6, 1), y: 0 }
          ]}
          domain={{
            x: [new Date(2000, 1, 1), new Date(2004, 1, 1)],
            y: [-10, 15]
          }}
          scale={{ x: "time", y: "linear" }}
          standalone={false}
        />
        <VictoryLine
          data={dataSetOne}
          domain={{
            x: [new Date(2000, 1, 1), new Date(2004, 1, 1)],
            y: [-10, 15]
          }}
          interpolation="monotoneX"
          scale={{ x: "time", y: "linear" }}
          standalone={false}
        />
        <VictoryAxis
          dependentAxis
          domain={[0, 50]}
          orientation="right"
          standalone={false}
        />
        <VictoryLine
          data={dataSetTwo}
          domain={{
            x: [new Date(2000, 1, 1), new Date(2004, 1, 1)],
            y: [0, 50]
          }}
          interpolation="monotoneX"
          scale={{ x: "time", y: "linear" }}
          standalone={false}
        />
      </g>
    </svg>
  );
}
```

我们得到了函数线的数据。

然后我们添加带有`VictoryLabel`组件的标签。

我们设置`x`和`y`道具来设置它们的位置。

然后，我们将图表组件放在`g`元素中。

`VictoryAxis`渲染 x 轴。

我们将`scale`设置为`'time'`来呈现轴上的日期。

同样，我们设置`offsetX`来移动轴。

第一个`VictoryLine`已经渲染出一条水平线。

我们设置`data`道具来设置数据。

此外，我们设置了`x`和`y`域来设置要在轴上显示的值的范围。

由于`x`和`y`的刻度不同，因此`scale`单独设置。

我们将`standalone`设置为`false`，这样我们可以向图表中添加更多内容。

然后再添加一个`VictoryLine`来渲染来自`dataSetOne`的数据。

我们再加一个`VictoryAxis`在右边放一个 y 轴。我们设定了自己的领域。

然后添加另一个`VictoryLine`来渲染来自`dataSetTwo`的数据。

现在我们得到了一个有 3 条线的折线图，y 轴在每一边，x 轴在底部，标签在图表的顶部。

# 结论

我们可以在 React 应用程序中添加带有 Victory 组件的自定义图表。