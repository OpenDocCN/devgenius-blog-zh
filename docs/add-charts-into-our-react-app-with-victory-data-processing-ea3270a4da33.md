# 将图表添加到我们的 Victory React 应用程序中—数据处理

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-data-processing-ea3270a4da33?source=collection_archive---------4----------------------->

![](img/de7e8416fb5e05665e6ae6a7aacfd109.png)

[杰森·霍根](https://unsplash.com/@jasonhogan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 指定 x 和 y 数据

如果我们有一个对象数组作为数据，我们可以用属性名指定`x`和`y`轴的数据。

例如，我们可以写:

```
import React from "react";
import { VictoryBar, VictoryChart } from "victory";export default function App() {
  return (
    <VictoryChart domainPadding={50}>
      <VictoryBar
        data={[
          { employee: "Jane Doe", salary: 65000 },
          { employee: "John Doe", salary: 62000 }
        ]}
        x="employee"
        y="salary"
      />
    </VictoryChart>
  );
}
```

我们分别用 x 轴和 y 轴的数据将`x`和`y`属性设置为属性名。

如果我们有一个数组的数组，我们可以用我们要为 x 轴和 y 轴显示的数据指定索引:

```
import React from "react";
import { VictoryBar, VictoryChart } from "victory";export default function App() {
  return (
    <VictoryChart domainPadding={50}>
      <VictoryBar
        data={[
          [1, 1],
          [2, 3],
          [3, 1]
        ]}
        x={0}
        y={1}
      />
    </VictoryChart>
  );
}
```

如果我们有一个嵌套对象的数组，我们也可以指定路径字符串或字符串数组，以形成包含我们要显示的数据的属性路径:

```
import React from "react";
import { VictoryBar, VictoryChart } from "victory";export default function App() {
  return (
    <VictoryChart domainPadding={50}>
      <VictoryBar
        data={[
          {
            employee: { firstName: "Jane", lastName: "Doe" },
            salary: { base: 65000, bonus: 2000 }
          },
          {
            employee: { firstName: "John", lastName: "Doe" },
            salary: { base: 62000, bonus: 6000 }
          }
        ]}
        x="employee.firstName"
        y={["salary", "base"]}
      />
    </VictoryChart>
  );
}
```

# 处理数据

我们可以通过为`x`和`y`道具传入函数来处理数据:

```
import React from "react";
import { VictoryAxis, VictoryBar, VictoryChart } from "victory";export default function App() {
  return (
    <VictoryChart domainPadding={{ x: 40 }}>
      <VictoryBar
        data={[
          { experiment: "trial 1", expected: 3.95, actual: 3.21 },
          { experiment: "trial 2", expected: 3.95, actual: 3.38 },
          { experiment: "trial 3", expected: 3.95, actual: 2.05 },
          { experiment: "trial 4", expected: 3.95, actual: 3.71 }
        ]}
        x="experiment"
        y={(d) => (d.actual / d.expected) * 100}
      />
      <VictoryAxis
        label="experiment"
        style={{
          axisLabel: { padding: 30 }
        }}
      />
      <VictoryAxis
        dependentAxis
        label="percent yield"
        style={{
          axisLabel: { padding: 40 }
        }}
      />
    </VictoryChart>
  );
}
```

我们传入一个函数来计算用函数显示的`y`的值。

`d`有我们要计算的数据条目。

# 分类数据

我们可以用`sortKey`道具对数据进行排序:

```
import React from "react";
import { VictoryChart, VictoryLine } from "victory";
import { range } from "lodash";export default function App() {
  return (
    <VictoryChart domainPadding={{ x: 40 }}>
      <VictoryLine
        data={range(0, 2 * Math.PI, 0.01).map((t) => ({ t }))}
        sortKey="t"
        x={({ t }) => Math.sin(3 * t + 2 * Math.PI)}
        y={({ t }) => Math.sin(2 * t)}
      />
    </VictoryChart>
  );
}
```

由于`sortKey`被设置为`t`，我们按`t`值排序。

# 结论

我们可以用各种方式处理数据，用 React Victory 创建我们想要的图表。