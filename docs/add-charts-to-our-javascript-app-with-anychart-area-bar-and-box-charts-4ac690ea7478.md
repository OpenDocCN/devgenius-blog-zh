# 使用任意图表(面积图、条形图和折线图)将图表添加到我们的 JavaScript 应用程序

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-area-bar-and-box-charts-4ac690ea7478?source=collection_archive---------13----------------------->

![](img/54fb682ecc592e76646dd40800205210.png)

[卢克·切塞尔](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Anychart 是一个易于使用的库，它允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 对比图

我们可以使用 Anychart 添加面积图。

要添加一个，我们编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
<div id="container" style="width: 500px; height: 400px;"></div>
```

以及下面的 JavaScript 代码:

```
const data = [
  ["January", 100],
  ["February", 120],
  ["March", 180],
  ["April", 110],
  ["May", 90]
];chart = anychart.area();
const series = chart.area(data);
chart.container("container");
chart.draw();
```

我们使用脚本标记添加 Anychart 基本包。

div 是呈现图表的容器。

JavaScript 代码具有带数据的`data`数组。

每个条目的第一个值是 x 轴值。

第二个值是 y 轴值。

然后我们调用`anychart,area`创建面积图。

`chart.area`拿数据。

`chart.container`设置要在其中呈现图表的容器的`id`值。

`chart.draw`绘制图表。

我们可以使用`normal`、`hovered`和`selected`方法更改系列填充颜色:

```
const data = [
  ["January", 100],
  ["February", 120],
  ["March", 180],
  ["April", 110],
  ["May", 90]
];chart = anychart.area();
const series = chart.area(data);
series.normal().fill("#00cc99", 0.3);
series.hovered().fill("#00cc99", 0.1);
series.selected().fill("#00cc99", 0.5);
chart.container("container");
chart.draw();
```

然后，我们看到当图表系列是盘旋或选定的颜色。

# 条形图

我们可以使用 Anychart 添加条形图。

要添加条形图，我们称之为`chart.bar`方法:

```
const data = [
  ["January", 100],
  ["February", 120],
  ["March", 180],
  ["April", 110],
  ["May", 90]
];chart = anychart.bar();
const series = chart.bar(data);
chart.container("container");
chart.draw();
```

`anychart.bar`创建图表。

`chart.bar`为柱状图添加数据。

`chart.container`设置要在其中呈现欺骗的容器元素的 ID。

`chart.draw`绘制图表。

HTML 与前面的示例相同。

我们也可以用同样的方法设置填充颜色。

# 方框图

为了创建一个方框图，我们写道:

```
const data = [{
    x: "1",
    low: 1000,
    q1: 1050,
    median: 1200,
    q3: 1800,
    high: 2000,
    outliers: [800, 2500, 3200]
  },
  {
    x: "2",
    low: 2500,
    q1: 3000,
    median: 3800,
    q3: 3900,
    high: 4000
  },
  {
    x: "3",
    low: 2000,
    q1: 2300,
    median: 2500,
    q3: 2900,
    high: 3000
  },
  {
    x: "4",
    low: 4000,
    q1: 5000,
    median: 6500,
    q3: 6500,
    high: 7000,
    outliers: [8930]
  },
  {
    x: "5",
    low: 8000,
    q1: 8400,
    median: 8500,
    q3: 8800,
    high: 9000,
    outliers: [6950, 3000]
  }
];const chart = anychart.box();
const series = chart.box(data);
chart.container("container");
chart.draw();
```

`low`为低值。

`q1`具有第 1 分位数值。

`median`具有中值。

`q3`为第 3 分位数。

`high`具有最高值。

`outliers`具有异常值。

然后，我们使用`anychart.box`方法创建方框图。

`chart.box`采用`data`渲染图表。

# 结论

我们可以使用 Anychart 轻松创建面积图、条形图和折线图。