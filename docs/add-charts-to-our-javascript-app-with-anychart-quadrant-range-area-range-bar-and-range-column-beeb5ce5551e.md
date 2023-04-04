# 使用 Anychart 将图表添加到我们的 JavaScript 应用程序中——象限图、范围面积图、范围条形图和范围柱形图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-quadrant-range-area-range-bar-and-range-column-beeb5ce5551e?source=collection_archive---------7----------------------->

![](img/7b771d60db39cf5b916c47b2b2f48585.png)

Marc St 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 象限图

我们可以用 Anychart 添加象限图。

为了添加它，我们编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

然后，我们添加以下 JavaScript 代码:

```
const data = [{
    x: 4,
    value: 42
  },
  {
    x: 13,
    value: 59
  },
  {
    x: 25,
    value: 68
  },
  {
    x: 25,
    value: 63
  },
  {
    x: 44,
    value: 54
  },
];const chart = anychart.quadrant(data);
chart.container("container");
chart.draw();
```

script 标签添加 Anychart 基础包。

那么 div 就是我们呈现图表的容器。

然后我们将图表数据添加到`data`数组中。

`x`有 x 轴值。

`value`有 y 轴数值。

`anychart.quadrant`添加象限图并为该图设置数据。

`chart.container`设置图表容器元素的 ID。

并且`chart.draw`绘制图表。

# 范围面积图

我们可以用 Anychart 添加一个范围面积图。

要添加一个，我们写:

```
const data = [
  ["January", 0.7, 6.1],
  ["February", 0.6, 6.3],
  ["March", 1.9, 8.5],
  ["April", 3.1, 10.8],
  ["May", 5.7, 14.4]
];const chart = anychart.area();
const series = chart.rangeArea(data);
chart.container("container");
chart.draw();
```

`data`数组按 x 轴值、y 轴低值、y 轴高值的顺序排列。

`anychart.area`创建面积图。

`chart.rangeArea`为图表设置数据。

代码的其余部分与前面的示例相同。

# 范围条形图

我们可以用下面的代码添加一个范围条形图:

```
const data = [
  ["January", 0.7, 6.1],
  ["February", 0.6, 6.3],
  ["March", 1.9, 8.5],
  ["April", 3.1, 10.8],
  ["May", 5.7, 14.4]
];const chart = anychart.bar();
const series = chart.rangeBar(data);
chart.container("container");
chart.draw();
```

`data`数组的结构与范围面积图相同。

`anychart.bar`让我们添加一个范围条形图。

`chart.rangeBar`设置范围栏的数据。

代码的其余部分与前面的示例相同。

# 范围柱形图

我们可以用下面的代码添加一个范围柱形图:

```
const data = [
  ["January", 0.7, 6.1],
  ["February", 0.6, 6.3],
  ["March", 1.9, 8.5],
  ["April", 3.1, 10.8],
  ["May", 5.7, 14.4]
];const chart = anychart.column();
const series = chart.rangeColumn(data);
chart.container("container");
chart.draw();
```

`data`阵列的结构与其他范围图表相同。

`anychart.column`创建范围柱形图。

`chart.rangeColumn`设置范围柱形图的数据。

代码的其余部分与其他示例相同。

# 结论

我们可以使用 Anychart 轻松添加象限图、范围面积图、范围条形图和范围柱形图。