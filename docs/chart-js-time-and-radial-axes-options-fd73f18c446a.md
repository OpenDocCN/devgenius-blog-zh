# Chart.js 时间轴和径向轴选项

> 原文：<https://blog.devgenius.io/chart-js-time-and-radial-axes-options-fd73f18c446a?source=collection_archive---------1----------------------->

![](img/c0cf554a702ebcb4be9948472d7a193e.png)

安德鲁·庞斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 时间轴刻度界限

我们可以改变`bounds`属性来控制缩放边界策略。

可以是`'data'`也可以是`'ticks'`。

`'data'`确保数据完全可见。

`'ticks'`确保蜱完全可见。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [{
        x: new Date(2020, 1, 1),
        y: 1
      }, {
        t: new Date(2020, 4, 1),
        y: 3
      }, {
        t: new Date(2020, 7, 1),
        y: 5
      }, {
        t: new Date(2020, 10, 1),
        y: 7
      }]
    }],
  },
  options: {
    scales: {
      xAxes: [{
        type: 'time',
        bounds: 'data'
      }]
    }
  }
});
```

根据可用数据显示刻度，使其可见。

# 蜱源

`ticks.source`属性控制刻度的生成。

该值可以是`'auto'`、`'data'`或`'labels'`。

`'auto'`根据刻度大小和时间选项生成最佳分笔成交点。

`'data'`从数据生成分笔成交点。

`'labels'`仅从用户给定的标签生成分笔成交点。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [{
        x: new Date(2020, 1, 1),
        y: 1
      }, {
        t: new Date(2020, 4, 1),
        y: 3
      }, {
        t: new Date(2020, 7, 1),
        y: 5
      }, {
        t: new Date(2020, 10, 1),
        y: 7
      }]
    }],
  },
  options: {
    scales: {
      xAxes: [{
        type: 'time',
        ticks: {
          source: 'auto'
        }
      }]
    }
  }
});
```

让 Chart.js 生成最佳分笔成交点。

# 线性径向轴

径向轴用于指定雷达图和极区图类型。

线性标尺用于绘制数字数据图表。

线性插值用于确定数值相对于轴中心的位置。

我们可以使用`ticks.suggestedMin`和`ticks.suggestedMax`属性更改轴范围设置。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'radar',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [0, 20, 40, 50]
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scale: {
      ticks: {
        suggestedMin: 50,
        suggestedMax: 100
      }
    }
  }
});
```

更改建议的最小值和最大值。

这样，我们可以将比例值更改为不同于默认值。

# 径向轴步长

我们也可以改变步长。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'radar',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [0, 20, 40, 50]
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scale: {
      ticks: {
        max: 60,
        min: 0,
        stepSize: 0.5
      }
    }
  }
});
```

我们为具有这些属性的轴设置`max`和`min`值。

`stepSize`改变径向轴的刻度大小。

# 结论

我们可以把时间轴改成我们想要的。

同样，我们可以改变径向轴线。