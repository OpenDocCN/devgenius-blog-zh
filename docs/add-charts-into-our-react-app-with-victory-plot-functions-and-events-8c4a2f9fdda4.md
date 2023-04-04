# 将图表添加到我们的 Victory React 应用程序中—绘制函数和事件

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-plot-functions-and-events-8c4a2f9fdda4?source=collection_archive---------8----------------------->

![](img/6cc03a8364c3adf4e210698cbb35f668.png)

照片由[J E W E L M I T CH E L L L](https://unsplash.com/@preciousjfm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 绘图功能

我们可以轻松地在我们的 React 应用程序中用 Victory 绘制函数。

例如，我们可以写:

```
import React from "react";
import { VictoryChart, VictoryLine } from "victory";export default function App() {
  return (
    <VictoryChart>
      <VictoryLine
        samples={50}
        style={{ data: { stroke: "red", strokeWidth: 4 } }}
        y={(data) => Math.sin(2 * Math.PI * data.x)}
      />
    </VictoryChart>
  );
}
```

我们设置`samples`属性来设置要绘制的点数。

然后`y`被设置为一个函数，其中包含我们想要绘制的值。

我们用`data.x`得到 x 轴的值，并用它做我们想做的事情。

# 组件事件

我们可以为图表中的组件添加事件处理程序。

例如，我们可以写:

```
import React from "react";
import { VictoryBar } from "victory";export default function App() {
  return (
    <VictoryBar
      data={[
        { x: 1, y: 2, label: "A" },
        { x: 2, y: 4, label: "B" },
        { x: 3, y: 7, label: "C" },
        { x: 4, y: 3, label: "D" },
        { x: 5, y: 5, label: "E" }
      ]}
      events={[
        {
          target: "data",
          eventHandlers: {
            onClick: () => {
              return [
                {
                  target: "labels",
                  mutation: (props) => {
                    return props.text === "clicked"
                      ? null
                      : { text: "clicked" };
                  }
                }
              ];
            }
          }
        }
      ]}
    />
  );
}
```

将`events`道具添加到我们的`VictoryBar`组件中。

在其中，我们添加了`eventHandlers.onClick`函数来为每个栏添加点击处理程序。

然后在`mutation`方法中，当我们点击工具条时，我们切换“点击的”文本。

# 嵌套组件事件

我们可以从图表的其他部分触发图表的一个部分的变化。

例如，我们可以写:

```
import React from "react";
import { VictoryArea, VictoryBar, VictoryChart, VictoryStack } from "victory";export default function App() {
  return (
    <VictoryChart
      events={[
        {
          childName: ["area-1", "area-2"],
          target: "data",
          eventHandlers: {
            onClick: () => {
              return [
                {
                  childName: "area-4",
                  mutation: (props) => {
                    const fill = props.style.fill;
                    return fill === "green"
                      ? null
                      : { style: { fill: "green" } };
                  }
                }
              ];
            }
          }
        }
      ]}
    >
      <VictoryStack>
        <VictoryArea
          name="area-1"
          data={[
            { x: "a", y: 2 },
            { x: "b", y: 3 },
            { x: "c", y: 5 },
            { x: "d", y: 4 }
          ]}
        />
        <VictoryArea
          name="area-2"
          data={[
            { x: "a", y: 1 },
            { x: "b", y: 4 },
            { x: "c", y: 5 },
            { x: "d", y: 7 }
          ]}
        />
        <VictoryArea
          name="area-3"
          data={[
            { x: "a", y: 3 },
            { x: "b", y: 2 },
            { x: "c", y: 6 },
            { x: "d", y: 2 }
          ]}
        />
        <VictoryArea
          name="area-4"
          data={[
            { x: "a", y: 2 },
            { x: "b", y: 3 },
            { x: "c", y: 3 },
            { x: "d", y: 4 }
          ]}
        />
      </VictoryStack>
    </VictoryChart>
  );
}
```

我们有 4 个`VictoryArea`组件来显示由这些点形成的填充区域。

然后我们给`VictoryChart`加上`events`道具。

`childName`具有触发`mutation`功能的组件的`name`值。

然后在`onClick`方法中，我们返回一个包含对象的数组，指定哪个组件发生了变化以及它们是如何变化的。

`childName`具有发生变化的部件的`name`。

`mutation`有代码改变`onClick`返回的数组中`childName`指定的组件。

我们用`name`和`area-4`拨动`VictoryArea`上`'green'`和`null`之间的`fill`。

# 结论

我们可以在 React Victory 的图表中绘制函数并监听图表组件中的事件。