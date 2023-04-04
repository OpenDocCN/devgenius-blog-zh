# Chart.js 轴标签和实例方法

> 原文：<https://blog.devgenius.io/chart-js-axis-labels-and-instance-methods-8414df4e849a?source=collection_archive---------6----------------------->

![](img/ba94c0fcfb360fc1175180e755e99ad3.png)

照片由[Lyndse ballow](https://unsplash.com/@lmballew?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 标记轴

标签轴告诉观众他们在看什么。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [0, 20, 40, 50]
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          callback(value, index, values) {
            return `$${value}`;
          }
        }
      }]
    }
  }
});
```

我们在带`callback`的数字前加了一美元。

`value`有 y 轴值。

# 式样

我们可以用各种选项来设计轴的样式。

例如，我们可以改变网格线的颜色、边框、刻度等等。

我们可以通过改变`gridLines`属性来实现。

为此，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [0, 20, 40, 50]
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scales: {
      yAxes: [{
        gridLines: {
          color: 'green'
        }
      }]
    }
  }
});
```

我们将`gridLine`的颜色改为`'green'`。

还有其他选项，如零线宽度、零线颜色、边框虚线等等。

# 刻度配置

我们还可以改变分笔成交点配置。

为此，我们可以更改`ticks`属性。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [0, 20, 40, 50]
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          fontColor: 'green'
        }
      }]
    }
  }
});
```

我们将字体颜色的 y 轴刻度改为`'green'`，使 y 轴标签为绿色。

其他选项包括字体样式、线宽、填充等。

还有小调和大调的选项。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [0, 20, 40, 50]
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          minor: {
            fontColor: 'green'
          },
          major: {
            fontColor: 'red'
          }
        }
      }]
    }
  }
});
```

更改次要和主要刻度选项。

选项的完整列表在 https://www.chartjs.org/docs/latest/axes/styling.html[上。](https://www.chartjs.org/docs/latest/axes/styling.html)

# 图表原型方法

每个`Chart`实例都有自己的实例方法。

它们包括:

*   `destroy` —销毁图表
*   `reset` —将图表重置为初始动画之前的状态。
*   `render(config)` —呈现带有各种选项的配置。
*   `stop` —停止任何当前动画循环
*   `resize` —调整图表画布元素的大小
*   `clear` —清除图表画布
*   `toBase64Image` —返回图表当前状态下的 base64 编码字符串
*   `generateLegend` —返回图表图例的 HTML 字符串
*   `getElementAtEvent(e)` —根据事件对象获取元素。
*   `getElementsAtEvent(e)` —根据事件对象获取元素。
*   `getDatasetAtEvent(e)` —获取所去点下的元素。
*   `getDatasetMeta(index)` —获取与当前索引匹配的数据集，并返回元数据。

# 结论

我们可以在图表上调用实例方法。

此外，标签可以改变我们想要的方式。