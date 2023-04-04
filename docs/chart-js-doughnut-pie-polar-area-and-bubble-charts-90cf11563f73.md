# Chart.js 圆环图/饼图、极区图和气泡图

> 原文：<https://blog.devgenius.io/chart-js-doughnut-pie-polar-area-and-bubble-charts-90cf11563f73?source=collection_archive---------8----------------------->

![](img/351f095f1031091c5cba47d60483fd14.png)

由[paweczerwiński](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 甜甜圈和饼图

我们可以创建一个将`type`设置为`'doughnut'`的圆环图。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
new Chart(ctx, {
  type: 'doughnut',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      backgroundColor: ['red', 'blue', 'yellow'],
      data: [10, 20, 30]
    }]
  },
});
```

创建三种颜色的圆环图。

`data`是一个数字数组。

数据集属性包括`backgroundColor`来改变背景颜色。

`borderAlign`改变边框的对齐方式。

`borderColor`改变边框颜色。

`borderWidth`改变边框宽度。

`hoverBackgroundColor`当鼠标悬停在线段上时改变颜色。

`hoverBorder`当鼠标悬停在线段上时，改变线段的颜色。

`weight`改变重量。

选项的完整列表在 https://www.chartjs.org/docs/latest/charts/doughnut.html 的 T21。

# 极区

极区图类似于饼图，但每个部分都有相同的角度。

根据值的不同，线段的半径也不同。

要创建一个，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
new Chart(ctx, {
  type: 'polarArea',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      backgroundColor: ['red', 'blue', 'yellow'],
      data: [10, 20, 30]
    }]
  },
});
```

我们创建了一个极坐标面积图，其中`type`设置为`'polarArea'`。

其他都和甜甜圈图一样。

我们可以用`borderAlign`属性改变边框对齐方式。

如果是`'center'`，那么圆弧的边界会重叠。

如果是`'inner'`，那么保证边框不会重叠。

我们可以用`Chart.defaults.polarArea`属性改变极区图的全局默认选项。

选项的完整列表在 https://www.chartjs.org/docs/latest/charts/polar.html 上。

# 泡泡图

气泡图用于同时显示三维数据。

气泡的位置由前两个尺寸以及相应的 x 轴和 y 轴决定。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
new Chart(ctx, {
  type: 'bubble',
  data: {
    datasets: [{
        label: 'John',
        data: [{
          x: 3,
          y: 7,
          r: 10
        }],
        backgroundColor: "green",
        hoverBackgroundColor: "green"
      },
      {
        label: 'Paul',
        data: [{
          x: 6,
          y: 2,
          r: 10
        }],
        backgroundColor: "green",
        hoverBackgroundColor: "green"
      },
      {
        label: 'George',
        data: [{
          x: 2,
          y: 6,
          r: 10
        }],
        backgroundColor: "green",
        hoverBackgroundColor: "green"
      },
    ]
  }
});
```

我们有一个带有`datasets`属性的数组。

每个条目都有一个带有属性为`x`、`y`和`r`的对象的`data`属性。

`x`和`y`是 x 坐标。

而`r`是以像素为单位的气泡半径。

`r`未被图表缩放。它是画布中绘制的气泡的 eaw 半径，以像素为单位。

我们可以更改许多其他选项，如边框、背景、悬停背景颜色、悬停半径、标签等。

选项的完整列表在[https://www.chartjs.org/docs/latest/charts/bubble.html](https://www.chartjs.org/docs/latest/charts/bubble.html)

# 结论

我们可以用 Chart.js 创建圆环图、饼图、极区图和气泡图