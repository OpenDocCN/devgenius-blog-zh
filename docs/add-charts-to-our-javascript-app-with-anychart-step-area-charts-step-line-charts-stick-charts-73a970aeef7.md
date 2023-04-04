# 使用 Anychart 将图表添加到我们的 JavaScript 应用程序中——阶梯面积图、阶梯折线图、棒图和日线图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-step-area-charts-step-line-charts-stick-charts-73a970aeef7?source=collection_archive---------13----------------------->

![](img/978e676056394edcc07edae0126795c3.png)

照片由 [Jukan Tateisi](https://unsplash.com/@tateisimikito?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 阶梯面积图

我们可以用 Anychart 创建一个阶跃面积图。

要添加一个，我们编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

然后我们编写以下 JavaScript 代码:

```
const data = [{
    x: "1995",
    value: 0.10
  },
  {
    x: "1996",
    value: 0.10
  },
  {
    x: "1997",
    value: 0.12
  },
  {
    x: "1998",
    value: 0.13
  },
  {
    x: "1999",
    value: 0.15
  },
];const chart = anychart.area();
const series = chart.stepArea(data);
series.stepDirection("forward");
chart.container("container");
chart.draw();
```

我们添加了带有脚本标签的 Anychart 基础包。

然后我们添加一个 div 作为图表的容器。

接下来，我们在每个条目中添加带有`x`和`value`属性的`data`数组。

`x`有 x 轴值。

`value`有 y 轴数值。

`anychart.area`创建面积图。

`chart.stepArea`用我们的数据创建步长面积图。

`series.stepDirection`设置步骤的方向。

`chart.container`设置图表容器元素的 ID。

`chart.draw`绘制图表。

# 阶跃线图

我们可以用类似的方法创建分档折线图。

例如，我们可以写:

```
const data = [{
    x: "1995",
    value: 0.10
  },
  {
    x: "1996",
    value: 0.10
  },
  {
    x: "1997",
    value: 0.12
  },
  {
    x: "1998",
    value: 0.13
  },
  {
    x: "1999",
    value: 0.15
  },
];const chart = anychart.stepLine();
const series = chart.stepLine(data);
series.stepDirection("forward");
chart.container("container");
chart.draw();
```

数据与前面的示例相同。

`anychart.stepLine`创建步线图。

`chart.stepLine`设置数据。

代码的其余部分与步骤面积图示例相同。

# 棒状图

我们可以很容易地用任何图表创建一个棒。

为了补充它，我们写道:

```
const data = [{
    x: "1995",
    value: 0.10
  },
  {
    x: "1996",
    value: 0.10
  },
  {
    x: "1997",
    value: 0.12
  },
  {
    x: "1998",
    value: 0.13
  },
  {
    x: "1999",
    value: 0.15
  },
];const chart = anychart.stick();
const series = chart.stick(data);
chart.container("container");
chart.draw();
```

我们调用`anychart.stick`来创建棒图。

`chart.stick`用我们的数据创建棒图。

代码的其余部分与其他示例相同。

# 旭日图

我们可以用任何图表创建一个旭日图。

为此，我们写道:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-sunburst.min.js"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

添加 Anychart base 和 sunburst 图表包以及容器 div。

然后我们写道:

```
const data = [{
  name: "Company A",
  children: [{
      name: "Technical",
      children: [{
          name: "Architects"
        },
        {
          name: "Developers"
        },
      ]
    },
    {
      name: "Sales",
      children: [{
          name: "Analysts"
        },
        {
          name: "Executives"
        }
      ]
    },
    {
      name: "HR"
    },
  ]
}];const chart = anychart.sunburst(data, "as-tree");
chart.container("container");
chart.draw();
```

添加数据和图表。

`data`具有`name`和`children`属性，分别添加段名和子段内容。

然后我们调用`anychart.sunburst`用`data`创建旭日图。

`'as-tree'`让我们以树形结构显示图表中的数据。

代码的其余部分与前面的示例相同。

# 结论

我们可以用 Anycharts 轻松地添加阶梯面积图、阶梯折线图、棒图和日线图。