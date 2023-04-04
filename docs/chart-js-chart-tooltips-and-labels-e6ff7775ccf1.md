# Chart.js 图表工具提示和标签

> 原文：<https://blog.devgenius.io/chart-js-chart-tooltips-and-labels-e6ff7775ccf1?source=collection_archive---------2----------------------->

![](img/6d4381d129e76d51be7db13b8746b062.png)

照片由[路易斯·汉瑟@shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 工具提示

我们可以用`option.tooltips`属性改变工具提示。

它们包括许多选项，如颜色、半径、宽度、文本方向、对齐方式等等。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      label: '# of Votes',
      data: [12.35748, 19, 3],
      backgroundColor: [
        'rgba(255, 99, 132, 0.2)',
        'rgba(54, 162, 235, 0.2)',
        'rgba(255, 206, 86, 0.2)',
      ],
      borderColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)',
      ],
      borderWidth: 1
    }]
  },
  options: {
    tooltips: {
      callbacks: {
        label: function(tooltipItem, data) {
          let label = data.datasets[tooltipItem.datasetIndex].label || ''; if (label) {
            label += ': ';
          }
          label += Math.round(tooltipItem.yLabel * 100) / 100;
          return label;
        }
      }
    },
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

将数字四舍五入到两位数。

我们有带有 y 轴值的`tooltipItem.yLabel`属性。

现在我们会看到，当我们将鼠标悬停在红色条上时，它的工具提示会显示一个带有两个十进制数字的数字。

# 标签颜色回调

我们也可以改变标签颜色回调。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      label: '# of Votes',
      data: [12.35748, 19, 3],
      backgroundColor: [
        'rgba(255, 99, 132, 0.2)',
        'rgba(54, 162, 235, 0.2)',
        'rgba(255, 206, 86, 0.2)',
      ],
      borderColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)',
      ],
      borderWidth: 1
    }]
  },
  options: {
    tooltips: {
      callbacks: {
        labelColor(tooltipItem, chart) {
          return {
            borderColor: 'rgb(255, 0, 0)',
            backgroundColor: 'rgb(255, 0, 0)'
          };
        },
        labelTextColor(tooltipItem, chart) {
          return 'lightgray';
        }
      }
    },
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

我们用带有`labelColor`方法的`options.tooltips.callbacks`属性返回带有`borderColor`和`backgroundColor`属性的对象。

`borderColor`具有工具提示的边框颜色。

而`backgroundColor`有工具提示的背景色。

另外，`labelTextColor`是一个返回工具提示标签文本颜色的方法。

`tooltipItem`对象有许多属性。

它们包括带有标签字符串的`label`属性。

`value`有价值。

`xLabel`和`yLabel`具有 x 和 y 标签值。

`datasetIndex`具有该项所来自的数据集的索引。

`index`拥有数据集中数据项的索引。

`x`和`y`是匹配点的 x 和 y 位置。

# 外部(自定义)工具提示

我们可以用`custom`方法定制我们的工具提示:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      label: '# of Votes',
      data: [12.35748, 19, 3],
      backgroundColor: [
        'rgba(255, 99, 132, 0.2)',
        'rgba(54, 162, 235, 0.2)',
        'rgba(255, 206, 86, 0.2)',
      ],
      borderColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)',
      ],
      borderWidth: 1
    }]
  },
  options: {
    tooltips: {
      enabled: false,
      custom(tooltipModel) {
        var tooltipEl = document.getElementById('chartjs-tooltip');
        if (!tooltipEl) {
          tooltipEl = document.createElement('div');
          tooltipEl.id = 'chartjs-tooltip';
          document.body.appendChild(tooltipEl);
        } if (tooltipModel.body) {
          tooltipEl.innerHTML = tooltipModel.body[0].lines
        } }
    },
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

我们创建一个工具提示元素，然后将`innerHTML`设置为`body[0].lines`属性的值。

现在，我们应该看到图表下方显示的标签值。

# 结论

有许多方法可以自定义图表的标签。