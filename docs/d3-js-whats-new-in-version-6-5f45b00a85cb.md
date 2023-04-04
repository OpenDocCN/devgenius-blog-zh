# D3 . js——版本 6 中有什么新内容？

> 原文：<https://blog.devgenius.io/d3-js-whats-new-in-version-6-5f45b00a85cb?source=collection_archive---------2----------------------->

![](img/950b51064b344fb96ffb61f67ef7c175.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 中的数据驱动文档，简称 D3.js，是一个非常受欢迎的库，可以使用 [canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) 或 [SVG](https://developer.mozilla.org/en-US/docs/Web/SVG) 为你的网页添加很酷的数据可视化。这是一个非常强大的工具，可以做任何你可以想象的事情，比如绘制图表或可视化数据。甚至有一些高级接口在 D3 的低级控制之上，例如 [Vega，它使用自己特定格式的 JSON 来更容易地创建视觉效果](https://vega.github.io/)，以及 [Altair，它是一个库](https://altair-viz.github.io/)，允许你用 Python 创建 Vega 视觉效果。

昨天，D3 发布了一个新的重大更新，版本 6.0。在这次更新中有大量的新特性，其中许多是不向后兼容的！我在这里描述其中的几个。

# ECMA 剧本

D3 的最新版本最终符合 ES6/ES2015 标准！这意味着为了浏览器的向后兼容性，它将需要一个 [transpiler](https://babeljs.io/) 。

# 再见，D3 .事件

在 D3 的以前版本中，为了处理任何 DOM 操作，事件监听器必须与一个全局对象`d3.event`对话，这个全局对象是一个静态字段，它返回关于当前被调用事件的所有必要信息。这需要一点时间来适应，并且在 DOM 事件和 D3 之间增加了一层不必要的隔离。

在 D3 6 中，D3.event 已被完全删除。所有事件监听器现在都接收到*事件本身—* 一个更加直观和干净的实现，感觉更像普通的 JS。

[受此影响的听众有](https://observablehq.com/d/f91cccf0cad5e9cb#events)`d3.selection``d3.transition``d3.brush``d3.drag``d3.zoom`。传递给回调函数的第一个参数现在是事件对象本身。

# 统一点击

D3 允许你读取光标输入的各种方式(之前的`d3.mouse`、`d3.touch`、`d3.touches`和`d3.clientPoint`，)已经被[整合成一个单一的便捷功能-](https://observablehq.com/d/f91cccf0cad5e9cb#pointer) 、[(以及多点触摸的](https://observablehq.com/d/f91cccf0cad5e9cb#pointer)、`[d3.pointers](https://observablehq.com/d/f91cccf0cad5e9cb#pointer)`、[)。)](https://observablehq.com/d/f91cccf0cad5e9cb#pointer)

熟悉的功能的另一个更加简化和强大的实现。

# 更快的三角测量

[为了将一个平面分割成最接近其上每个物体的区域](https://en.wikipedia.org/wiki/Voronoi_diagram)，最常用的有效算法是一种叫做财富算法的[“扫描线”技术。](https://en.wikipedia.org/wiki/Fortune's_algorithm)

例如，这种技术在地理图中非常有用。它们对于实际的 UI 目的也很方便，例如使悬停在图中的点上以获得更多信息变得容易(因为 voronoi 分区是图形的直观明显的可视部分)或者甚至是在线图中放置标签(使用 voronoi 分区将确保各种标签彼此不受阻碍)。)

在 D3 的旧版本中，有一个名为`d3-voronoi`的内置 api 来完成这个任务。[在 D3 6 中，已经被](https://observablehq.com/d/f91cccf0cad5e9cb#delaunay) `[d3-delaunay](https://observablehq.com/d/f91cccf0cad5e9cb#delaunay)` [](https://observablehq.com/d/f91cccf0cad5e9cb#delaunay)取代——一种计算 Voronoi 划分的优化算法，比旧版本快 5–10 倍！

奇妙的新 D3 文档包括了对其工作原理的可视化解释，我强烈推荐你去看看。这些可视化的例子不仅使理解 D3 下涉及的更复杂的主题变得容易，它们也是 D3 如何简单有效地将数据可视化付诸实践的完美例子！

在新版本中有更多的功能，但我希望这是对新 D3 更新的一个很好的介绍！

[](https://observablehq.com/d/f91cccf0cad5e9cb) [## D3 6.0 迁移指南

### D3 6.0 迁移指南刷、拖、缩放事件新的事件系统适用于所有的监听器，不仅仅是在…

observablehq.com](https://observablehq.com/d/f91cccf0cad5e9cb) [](https://github.com/d3/d3/blob/master/CHANGES.md) [## d3/d3

### 2020 年 8 月 26 日上映。本文件仅涵盖主要变更。对于小的和补丁的变化，请参阅版本…

github.com](https://github.com/d3/d3/blob/master/CHANGES.md) [](https://observablehq.com/@d3/gallery) [## 走廊

### 画廊寻找一个好的 D3 例子？这里有几个(好吧，...)来细读。动画 D3 的数据连接，插值器，和…

observablehq.com](https://observablehq.com/@d3/gallery)