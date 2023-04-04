# 用 Visx 库添加径向线

> 原文：<https://blog.devgenius.io/add-radial-line-with-the-visx-library-c932010020f8?source=collection_archive---------7----------------------->

![](img/3589809a803c3eaf6700be3d9b4a3db1.png)

照片由[陈海](https://unsplash.com/@jokerhoi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在这篇文章中，我们将看看如何使用它来添加辐射线到我们的反应应用程序。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/curve @visx/gradient @visx/group @visx/mock-data @visx/responsive @visx/scale @visx/shape
```

安装软件包。

# 创建径向线

我们可以通过添加模块提供的项目来创建图表。

我们使用来自`@visx/mock-data`模块的数据。

然后，我们可以通过编写以下内容来创建径向线图:

```
import React, { useRef, useState, useEffect } from "react";
import { Group } from "@visx/group";
import { LineRadial } from "@visx/shape";
import { scaleTime, scaleLog } from "@visx/scale";
import { curveBasisOpen } from "@visx/curve";
import appleStock from "@visx/mock-data/lib/mocks/appleStock";
import { LinearGradient } from "@visx/gradient";
import { animated, useSpring } from "react-spring";const green = "#e5fd3d";
export const blue = "#aeeef8";
const darkgreen = "#dff84d";
export const background = "#744cca";
const darkbackground = "#603FA8";
const springConfig = {
  tension: 20
};// utils
function extent(data, value) {
  const values = data.map(value);
  return [Math.min(...values), Math.max(...values)];
}
// accessors
const date = (d) => new Date(d.date).valueOf();
const close = (d) => d.close;// scales
const xScale = scaleTime({
  range: [0, Math.PI * 2],
  domain: extent(appleStock, date)
});
const yScale = scaleLog({
  domain: extent(appleStock, close)
});const angle = (d) => xScale(date(d)) ?? 0;
const radius = (d) => yScale(close(d)) ?? 0;const firstPoint = appleStock[0];
const lastPoint = appleStock[appleStock.length - 1];const Example = ({ width, height, animate = true }) => {
  const lineRef = useRef(null);
  const [lineLength, setLineLength] = useState(0);
  const [shouldAnimate, setShouldAnimate] = useState(false); const spring = useSpring({
    frame: shouldAnimate ? 0 : 1,
    config: springConfig,
    onRest: () => setShouldAnimate(false)
  }); const effectDependency = lineRef.current;
  useEffect(() => {
    if (lineRef.current) {
      setLineLength(lineRef.current.getTotalLength());
    }
  }, [effectDependency]);if (width < 10) return null;
  yScale.range([0, height / 2 - 20]);const yScaleTicks = yScale.ticks();
  const handlePress = () => setShouldAnimate(true); return (
    <>
      {animate && (
        <>
          <button
            type="button"
            onClick={handlePress}
            onTouchStart={handlePress}
          >
            Animate
          </button>
          <br />
        </>
      )}
      <svg
        width={width}
        height={height}
        onClick={() => setShouldAnimate(!shouldAnimate)}
      >
        <LinearGradient from={green} to={blue} id="line-gradient" />
        <rect width={width} height={height} fill={background} rx={14} />
        <Group top={height / 2} left={width / 2}>
          {yScaleTicks.map((tick, i) => (
            <circle
              key={`radial-grid-${i}`}
              r={yScale(tick)}
              stroke={blue}
              strokeWidth={1}
              fill={blue}
              fillOpacity={1 / (i + 1) - (1 / i) * 0.2}
              strokeOpacity={0.2}
            />
          ))}
          {yScaleTicks.map((tick, i) => (
            <text
              key={`radial-grid-${i}`}
              y={-(yScale(tick) ?? 0)}
              dy="-.33em"
              fontSize={8}
              fill={blue}
              textAnchor="middle"
            >
              {tick}
            </text>
          ))} <LineRadial angle={angle} radius={radius} curve={curveBasisOpen}>
            {({ path }) => {
              const d = path(appleStock) || "";
              return (
                <>
                  <animated.path
                    d={d}
                    ref={lineRef}
                    strokeWidth={2}
                    strokeOpacity={0.8}
                    strokeLinecap="round"
                    fill="none"
                    stroke={animate ? darkbackground : "url(#line-gradient)"}
                  />
                  {shouldAnimate && (
                    <animated.path
                      d={d}
                      strokeWidth={2}
                      strokeOpacity={0.8}
                      strokeLinecap="round"
                      fill="none"
                      stroke="url(#line-gradient)"
                      strokeDashoffset={spring.frame.interpolate(
                        (v) => v * lineLength
                      )}
                      strokeDasharray={lineLength}
                    />
                  )}
                </>
              );
            }}
          </LineRadial>
          {[firstPoint, lastPoint].map((d, i) => {
            const cx = ((xScale(date(d)) ?? 0) * Math.PI) / 180;
            const cy = -(yScale(close(d)) ?? 0);
            return (
              <circle
                key={`line-cap-${i}`}
                cx={cx}
                cy={cy}
                fill={darkgreen}
                r={3}
              />
            );
          })}
        </Group>
      </svg>
    </>
  );
};export default function App() {
  return (
    <div className="App">
      <Example width={500} height={300} />
    </div>
  );
}
```

我们用`green`、`blue`和`background`变量设置线条和背景颜色。

`darkBackground`用于深色背景。

`extent`函数返回一个包含数据最小值和最大值的数字数组。

`date`和`close`是数据访问器，分别返回时间和值的数据。

从`domain`和`range`创建`xScale`和`yScale`值。

`xScale`既有域又有范围，这样可以创建径向线。

我们还需要`angle`和`radius`函数来计算径向线的点。

我们有`shouldAnimate`状态来设置何时动画填充线条。

在`Group`组件中，我们有`circle`元素来创建同心圆。

`text`元素包含每个环值的文本。

`LineRadial`组件根据从`angle`和`radius`函数返回的值呈现该行，我们将它们作为道具传入。

render prop 返回从`d`路径字符串创建的`animated.path`组件。

第二个`animated.path`组件具有通过动画制作`stroke`和`strokeDashOffset`生成的填充线。

下面的`circle`分别添加到线条的起点和终点，作为线条终点的标记。

# 结论

我们可以通过 Visx 库轻松地在 React 应用程序中添加一条径向线。