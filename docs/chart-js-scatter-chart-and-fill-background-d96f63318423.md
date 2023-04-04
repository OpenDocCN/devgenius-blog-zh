# Chart.js 散点图和填充背景

> 原文：<https://blog.devgenius.io/chart-js-scatter-chart-and-fill-background-d96f63318423?source=collection_archive---------1----------------------->

![](img/79235f17b8a48dda1fcd50b15b623a6f.png)

照片由 [Erol Ahmed](https://unsplash.com/@erol?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 散点图

我们可以用 Chart.js 创建一个散点图

这是一个折线图，x 轴改为线性轴。

例如，我们可以通过编写以下内容来创建一个:

```
var ctx = document.getElementById('myChart').getContext('2d');
new Chart(ctx, {
  type: 'scatter',
  data: {
    datasets: [{
      label: 'Scatter Dataset',
      backgroundColor: 'green',
      data: [{
        x: -10,
        y: 0
      }, {
        x: 0,
        y: 10
      }, {
        x: 10,
        y: 5
      }]
    }]
  },
  options: {
    scales: {
      xAxes: [{
        type: 'linear',
        position: 'bottom'
      }]
    }
  }
});
```

我们有一个带有对象数组的`data`属性，这些对象有`x`和`y`属性。

`x`是图表上的 x 坐标，`y`是 y 坐标。

其他属性与折线图相同。

我们可以去[https://www . chart js . org/docs/latest/charts/line . html # dataset-properties](https://www.chartjs.org/docs/latest/charts/line.html#dataset-properties)获取选项。

# 面积图

折线图和雷达图支持`fill`选项，而不是可用于在 2 个数据集和边界之间创建区域的数据集对象。

我们可以用`fill`属性添加填充。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: ['Monday', 'Tuesday', 'Wednesday'],
    datasets: [{
      label: '# of Votes',
      data: [12, 19, 3],
      borderColor: 'green',
      fill: false,
      borderWidth: 1
    }]
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    }
  }
});
```

在`fill`属性设置为`false`的情况下禁用填充。

此外，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: ['Monday', 'Tuesday', 'Wednesday'],
    datasets: [{
        label: '# of foo',
        data: [12, 19, 3],
        borderColor: 'green',
        fill: '+2',
        borderWidth: 1
      },
      {
        label: '# of bar',
        data: [1, 4, 3],
        borderColor: 'red',
        fill: false,
        borderWidth: 1
      },
      {
        label: '# of baz',
        data: [2, 9, 13],
        borderColor: 'blue',
        fill: false,
        borderWidth: 1
      }]
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    }
  }
});
```

使用`'+2'`选项在数据集 1 和数据集 3 之间添加填充。

我们会看到绿色和蓝色线之间的填充。

线条颜色用`borderWidth`属性设置。

我们还可以将其设置为负数，以便在后面的数据集和前面添加的数据集之间进行填充。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: ['Monday', 'Tuesday', 'Wednesday'],
    datasets: [{
        label: '# of foo',
        data: [12, 19, 3],
        borderColor: 'green',
        fill: false,
        borderWidth: 1
      },
      {
        label: '# of bar',
        data: [1, 4, 3],
        borderColor: 'red',
        fill: false,
        borderWidth: 1
      },
      {
        label: '# of baz',
        data: [2, 9, 13],
        borderColor: 'blue',
        fill: '-2',
        borderWidth: 1
      }]
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    }
  }
});
```

我们也可以将`fill`设置为`'origin'`来填充到原点。

还有一个`propagate`选项，使填充区域递归扩展到由隐藏数据集目标的`fill`值定义的可见目标:

```
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: ['Monday', 'Tuesday', 'Wednesday'],
    datasets: [{
        label: '# of foo',
        data: [12, 19, 3],
        borderColor: 'green',
        fill: false,
        borderWidth: 1
      },
      {
        label: '# of bar',
        data: [1, 4, 3],
        borderColor: 'red',
        fill: false,
        borderWidth: 1
      },
      {
        label: '# of baz',
        data: [2, 9, 13],
        borderColor: 'blue',
        fill: '-2',
        borderWidth: 1
      }]
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    },
    plugins: {
      filler: {
        propagate: true
      }
    }
  }
});
```

这样，如果数据集被隐藏，那么填充将传播到显示的最近的数据集。

# 结论

我们可以创建散点图和折线图，并用 Chart.js 填充背景