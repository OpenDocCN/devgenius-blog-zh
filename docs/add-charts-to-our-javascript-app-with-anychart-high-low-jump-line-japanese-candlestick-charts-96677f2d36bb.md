# 使用 Anychart 将图表添加到我们的 JavaScript 应用程序中——高低点、跳线、日本烛台图表

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-high-low-jump-line-japanese-candlestick-charts-96677f2d36bb?source=collection_archive---------6----------------------->

![](img/50b6aae8bfe89693990623b8f74c70f5.png)

照片由[赛义德·阿里](https://unsplash.com/@syedmohdali121?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 高低点图

我们可以使用 Anychart 轻松创建高低点图。

要添加一个，我们写:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

然后我们写道:

```
const data = [
  ["January", 1000, 5000],
  ["February", 500, 2000],
  ["March", 3000, 6000],
  ["April", 4000, 19680],
  ["May", 6000, 58581]
];chart = anychart.hilo();
const series = chart.hilo(data);
chart.title("HiLo Chart");
chart.container("container");
chart.draw();
```

我们为基础包添加脚本标签。

然后我们添加容器 div 来呈现图表。

`data`有一组数据。

每个条目的第一个值是 x 轴值。

第二个值是低值。

第三个值是高值。

然后我们用`anychart.hilo`创建高低点图。

`chart.hilo`根据数据创建系列。

`chart.title`设置标题。

`chart.container`设置用于呈现图表的容器的 ID。

`chart.draw`绘制图表。

# 日本烛台图表

为了创建一个日本烛台图表，我们写:

```
const data = [
  [Date.UTC(2007, 07, 23), 23.55, 23.88, 23.38, 23.62],
  [Date.UTC(2007, 07, 24), 22.65, 23.7, 22.65, 23.36],
  [Date.UTC(2007, 07, 25), 22.75, 23.7, 22.69, 23.44],
  [Date.UTC(2007, 07, 26), 23.2, 23.39, 22.87, 22.92],
  [Date.UTC(2007, 07, 27), 23.98, 24.49, 23.47, 23.49],
];const chart = anychart.candlestick();
const series = chart.candlestick(data);
chart.container("container");
chart.draw();
```

HTML 与前面的例子相同。

我们在第一个`data`数组条目中有 x 轴的日期值。

那么其他值分别是开盘价、最高价、最低价和收盘价。

然后我们用`anychart.candlestuck`创建日本蜡烛图。

`chart.candlestick`取数据。

其余与上例相同。

# 跳跃线图

我们可以通过编写以下内容来创建跳线图:

```
const data = [{
    x: "January",
    value: 10000
  },
  {
    x: "February",
    value: 12000
  },
  {
    x: "March",
    value: 18000
  }
];const chart = anychart.jumpLine();
const series = chart.jumpLine(data);
chart.container("container");
chart.draw();
```

我们有带有 x 轴值的属性的`data`数组，

并且`value`具有 y 轴值。

`anychart.jumpLine`创建跳线图。

`chart.jumpLine`取数据。

其余的和前面的例子一样。

# 折线图

我们可以用基本的 Anychart 包添加一个折线图。

要添加一个，请写:

```
const data = [
  ["January", 10000],
  ["February", 12000],
  ["March", 18000],
];const chart = anychart.line();
const series = chart.line(data);
chart.container("container");
chart.draw();
```

在每个数组条目中，我们有分别带有 x 轴和 y 轴值的`data`数组。

然后我们调用`anychart.line`来创建折线图。

`chart.line`为折线图设置数据。

其余的和前面的例子一样。

# 结论

我们可以使用 Anychart 轻松添加高低点、跳线、日式烛台和折线图。