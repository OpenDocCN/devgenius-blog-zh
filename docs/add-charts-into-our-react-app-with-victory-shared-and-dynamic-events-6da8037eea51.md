# 将图表添加到我们的 React 应用 with Victory —共享的动态事件

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-shared-and-dynamic-events-6da8037eea51?source=collection_archive---------6----------------------->

![](img/f8793cbb6acef141e83cbae33628d277.png)

凯尔·马丁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# VictorySharedEvents

我们可以使用`VictorySharedEvents`组件添加多个图表之间共享的事件。

例如，我们可以写:

```
import React from "react";
import {
  VictoryBar,
  VictoryLabel,
  VictoryPie,
  VictorySharedEvents
} from "victory";export default function App() {
  return (
    <svg viewBox="0 0 450 350">
      <VictorySharedEvents
        events={[
          {
            childName: ["pie", "bar"],
            target: "data",
            eventHandlers: {
              onMouseOver: () => {
                return [
                  {
                    childName: ["pie", "bar"],
                    mutation: (props) => {
                      return {
                        style: Object.assign({}, props.style, {
                          fill: "tomato"
                        })
                      };
                    }
                  }
                ];
              },
              onMouseOut: () => {
                return [
                  {
                    childName: ["pie", "bar"],
                    mutation: () => {
                      return null;
                    }
                  }
                ];
              }
            }
          }
        ]}
      >
        <g transform={"translate(150, 50)"}>
          <VictoryBar
            name="bar"
            width={300}
            standalone={false}
            style={{
              data: { width: 20 },
              labels: { fontSize: 25 }
            }}
            data={[
              { x: "a", y: 2 },
              { x: "b", y: 3 },
              { x: "c", y: 5 }
            ]}
            labels={["a", "b", "c"]}
            labelComponent={<VictoryLabel y={290} />}
          />
        </g>
        <g transform={"translate(0, -75)"}>
          <VictoryPie
            name="pie"
            width={250}
            standalone={false}
            style={{ labels: { fontSize: 25, padding: 10 } }}
            data={[
              { x: "a", y: 1 },
              { x: "b", y: 4 },
              { x: "c", y: 5 }
            ]}
          />
        </g>
      </VictorySharedEvents>
    </svg>
  );
}
```

在`events`属性中，我们添加了一个带有`eventsHandlers`对象的数组，为各种事件添加事件处理程序。

我们用`onMouseOver`为 mouseover 提供事件处理程序，用`onMouseOut`为 mouseout 提供事件处理程序。

`mutation`方法让我们改变我们悬停在其上的段的填充样式。

`childName`具有我们想要更改的图表的`name`值。

类似地，我们有`onMouseOut`事件处理程序来返回`null`以恢复到原始样式。

# 外部事件突变

我们可以通过改变状态来附加事件突变。

例如，我们可以写:

```
import React, { useState } from "react";
import { VictoryBar, VictoryChart } from "victory";const buttonStyle = {
  backgroundColor: "black",
  color: "white",
  padding: "10px",
  marginTop: "10px"
};export default function App() {
  const [externalMutations, setExternalMutations] = useState({});
  const removeMutation = () => {
    setExternalMutations(undefined);
  };const clearClicks = () => {
    setExternalMutations([
      {
        childName: "Bar-1",
        target: ["data"],
        eventKey: "all",
        mutation: () => ({ style: undefined }),
        callback: removeMutation
      }
    ]);
  };return (
    <div>
      <button onClick={clearClicks} style={buttonStyle}>
        Reset
      </button>
      <VictoryChart
        domain={{ x: [0, 5] }}
        externalEventMutations={externalMutations}
        events={[
          {
            target: "data",
            childName: "Bar-1",
            eventHandlers: {
              onClick: () => ({
                target: "data",
                mutation: () => ({ style: { fill: "orange" } })
              })
            }
          }
        ]}
      >
        <VictoryBar
          name="Bar-1"
          style={{ data: { fill: "grey" } }}
          labels={() => "click me!"}
          data={[
            { x: 1, y: 2 },
            { x: 2, y: 4 },
            { x: 3, y: 1 },
            { x: 4, y: 5 }
          ]}
        />
      </VictoryChart>
    </div>
  );
}
```

用`events`道具在`VictoryChart`上添加点击事件处理程序。

当我们单击条形时，填充变为橙色。

重置按钮调用`clearClicks`通过将`externalMutations`设置为`undefined`来清除所有点击处理程序。

`clearClicks`从条中清除所有样式。

我们将`externalEventMutations`设置为`externalMutations`。

# 结论

我们可以动态添加事件处理程序，并使用 React Victory 将共享事件添加到图表中。