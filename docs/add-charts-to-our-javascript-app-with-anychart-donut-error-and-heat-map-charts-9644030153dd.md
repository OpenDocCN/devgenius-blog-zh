# 使用 Anychart 向我们的 JavaScript 应用程序添加图表—圆环图、误差图和热图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-donut-error-and-heat-map-charts-9644030153dd?source=collection_archive---------5----------------------->

![](img/cb5281c38fa5525543ca75ec51dcb2f7.png)

照片由[杆长](https://unsplash.com/@rodlong?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 圆环图

我们可以用 Anychart 在我们的 web 应用程序中添加一个甜甜圈图。

例如，我们可以编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

然后我们可以添加以下 JavaScript:

```
const data = [{
    x: "A",
    value: 30
  },
  {
    x: "B",
    value: 53
  },
  {
    x: "C",
    value: 34
  },
];const chart = anychart.pie(data);
chart.innerRadius("30%");
chart.container("container");
chart.draw();
```

我们添加带有脚本标签的基础 Anychart 包。

然后我们添加 div 来呈现图表。

在 JavaScript 代码中，我们用`data`数组为图表添加数据。

`anychart.pie`让我们创建圆环图。

`chart.innerRadius`让我们添加一个半径为给定百分比的角色。

`chart.container`设置要在其中呈现图表的容器元素的 ID。

`chart.draw`绘制海图。

# 误差图表

我们可以用`series.error`方法创建一个误差图。

例如，我们可以写:

```
const data = [
  ["January", 10000],
  ["February", 12000],
  ["March", 13000],
];const chart = anychart.column();
const series = chart.column(data);
series.error("10%");
chart.container("container");
chart.draw();
```

我们的图表有了`data`。

然后我们用`anychart.column`创建柱形图。

我们用`series.error`方法给它添加误差线。

这将使误差线显示在列上。

我们可以在面积图中添加误差线。条形图、折线图、散点图、棒图等等。

# 漏斗图

我们可以使用以下 HTML 添加漏斗图:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-pyramid-funnel.min.js"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

我们需要`funnel-pyramid`包来添加图表。

HTML 的其余部分与其他示例相同。

然后，我们通过书写来添加漏斗图:

```
const data = [
  ["apple", 2320],
  ["orange", 940],
  ["grape", 490],
];const chart = anychart.funnel(data);
chart.container("container");
chart.draw();
```

我们调用`anychart.funnel`来创建漏斗图。

并调用`chart.container`来设置用于呈现图表的容器的 ID。

# 热图图表

为了创建热图图表，我们编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-heatmap.min.js"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

我们需要添加`heatmap`包来创建热图。

然后我们编写以下 JavaScript 代码:

```
const data = [{
    x: "2010",
    y: "A",
    heat: 15
  },
  {
    x: "2011",
    y: "A",
    heat: 17
  },
  {
    x: "2012",
    y: "A",
    heat: 21
  },
  {
    x: "2010",
    y: "B",
    heat: 34
  },
  {
    x: "2011",
    y: "B",
    heat: 33
  },
  {
    x: "2012",
    y: "B",
    heat: 32
  },
];const chart = anychart.heatMap(data);
chart.container("container");
chart.draw();
```

`data`包含带有`x`和`y`标签的对象。`heat`是给定`x`和`y`位置的值。

`anychart.heatMap`让我们创建图表。

# 结论

我们可以使用 Anychart 轻松添加圆环图、误差图和热图。