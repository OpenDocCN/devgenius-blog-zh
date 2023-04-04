# 用 Visx 库创建一个 React 和弦图

> 原文：<https://blog.devgenius.io/create-a-react-chord-diagram-with-the-visx-library-45664861cedc?source=collection_archive---------7----------------------->

![](img/8cf20a5221836f4d340d0359c0bde9cb.png)

由 [Markus Gjengaar](https://unsplash.com/@markus_gjengaar?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将和弦图添加到 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/chord @visx/gradient @visx/group @visx/responsive @visx/scale @visx/shape
```

安装软件包。

# 创建图表

我们可以通过添加模块提供的项目来创建图表。

我们使用来自`@visx/mock-data`模块的数据。

为了创建和弦图，我们写下:

```
import React from "react";
import { Arc } from "@visx/shape";
import { Group } from "@visx/group";
import { Chord, Ribbon } from "@visx/chord";
import { scaleOrdinal } from "@visx/scale";
import { LinearGradient } from "@visx/gradient";const pink = "#ff2fab";
const orange = "#ffc62e";
const purple = "#dc04ff";
const purple2 = "#7324ff";
const red = "#d04376";
const green = "#52f091";
const blue = "#04a6ff";
const lime = "#00ddc6";
const bg = "#e4e3d8";const dataMatrix = [
  [11975, 5871, 8916, 2868],
  [1951, 10048, 2060, 6171],
  [8010, 16145, 8090, 8045],
  [1013, 990, 940, 6907]
];function descending(a, b) {
  return b < a ? -1 : b > a ? 1 : b >= a ? 0 : NaN;
}const color = scaleOrdinal({
  domain: [0, 1, 2, 3],
  range: [
    "url(#gpinkorange)",
    "url(#gpurplered)",
    "url(#gpurplegreen)",
    "url(#gbluelime)"
  ]
});function Example({ width, height, centerSize = 20, events = false }) {
  height -= 77;
  const outerRadius = Math.min(width, height) * 0.5 - (centerSize + 10);
  const innerRadius = outerRadius - centerSize;return width < 10 ? null : (
    <div className="chords">
      <svg width={width} height={height}>
        <LinearGradient
          id="gpinkorange"
          from={pink}
          to={orange}
          vertical={false}
        />
        <LinearGradient
          id="gpurplered"
          from={purple}
          to={red}
          vertical={false}
        />
        <LinearGradient
          id="gpurplegreen"
          from={purple2}
          to={green}
          vertical={false}
        />
        <LinearGradient id="gbluelime" from={blue} to={lime} vertical={false} />
        <rect width={width} height={height} fill={bg} rx={14} />
        <Group top={height / 2} left={width / 2}>
          <Chord matrix={dataMatrix} padAngle={0.05} sortSubgroups={descending}>
            {({ chords }) => (
              <g>
                {chords.groups.map((group, i) => (
                  <Arc
                    key={`key-${i}`}
                    data={group}
                    innerRadius={innerRadius}
                    outerRadius={outerRadius}
                    fill={color(i)}
                    onClick={() => {
                      if (events) alert(`${JSON.stringify(group)}`);
                    }}
                  />
                ))}
                {chords.map((chord, i) => {
                  return (
                    <Ribbon
                      key={`ribbon-${i}`}
                      chord={chord}
                      radius={innerRadius}
                      fill={color(chord.target.index)}
                      fillOpacity={0.75}
                      onClick={() => {
                        if (events) alert(`${JSON.stringify(chord)}`);
                      }}
                    />
                  );
                })}
              </g>
            )}
          </Chord>
        </Group>
      </svg>
      <style jsx>{`
        .chords {
          display: flex;
          flex-direction: column;
          user-select: none;
        }
        svg {
          margin: 1rem 0;
          cursor: pointer;
        }
        .deets {
          display: flex;
          flex-direction: row;
          font-size: 12px;
        }
        .deets > div {
          margin: 0.25rem;
        }
      `}</style>
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

我们添加带有颜色代码的变量来设置和弦的颜色。

`dataMatrix`变量包含和弦的数据。

`descending`函数根据`a`和`b`的值返回一个数字，将排序改为降序。

接下来，我们创建`color`变量来添加和弦的颜色范围。

在`Example`组件中，我们添加了和弦图。

我们添加`LinearGradient`组件来为和弦添加渐变。

然后我们在`Group`组件中添加`Chord`组件来添加和弦。

我们通过渲染`Arc`组件来创建和弦组。

我们用`Ribbon`组件渲染和弦本身。

最后，我们有一些风格来定位和弦图。

# 结论

我们可以使用 Visx 库提供的 React 组件轻松创建和弦图。