# BootstrapVue —布局

> 原文：<https://blog.devgenius.io/bootstrapvue-layouts-fb479c2d043b?source=collection_archive---------22----------------------->

![](img/5a537a4d92d5cc94f36b1901290c1843.png)

照片由[斯雷科·斯克罗比奇](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在这篇文章中，我们将看看如何添加布局。

# 可变宽度内容

我们可以将断点 prop 设置为`'auto'`来使列成为变量。

断点包括`xs`、`sm`、`md`、`lg`和`xl`。

`xs`宽度小于 576px。

`sm`宽度大于等于 576px。

`md`宽度大于等于 768px。

`lg`宽度大于等于 992px。

`xl`宽度大于或等于 1200px。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col col >1</b-col>
        <b-col col sm="auto">2</b-col>
        <b-col col >3</b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

使中间列的宽度可变。

我们添加了`col`和`sm='auto'`来使中间的列宽度可变，其他列填充剩余的空间。

# 响应类

我们可以使用`cols`支柱来调整列宽比例。

该值应介于 1 和 12 之间。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col cols="8">8</b-col>
        <b-col cols="4">4</b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

然后我们让左栏占据 8 / 12 或三分之二的空间。

右栏占了 4 / 12 或三分之一的空间。

# 水平堆叠

我们可以使用`sm`道具创建一个堆叠在小型设备上的网格，并成为中型设备的水平网格。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col sm="8">8</b-col>
        <b-col sm="4">4</b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

然后在窄屏幕上，8 在 4 的上面，而在宽屏幕上，它们是并排的。

# 对齐

我们可以使用`b-row`上的`align-v`支柱来垂直对齐内容。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-container>
      <b-row align-v="center">
        <b-col>1</b-col>
        <b-col>2</b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script><style>
.row {
  height: 200px;
}
</style>
```

然后该行在`b-row`中居中。

`align-v`的其他可能值可以是`'start'`以将行与顶部边缘对齐。

同样，我们可以将它设置为`'end'`来将行与底部边缘对齐。

`'baseline'`将最高的行与顶部边缘对齐。

`'stretch'`将高度拉伸至容器的高度。

# 水平线向

要水平对齐项目，我们可以使用`align-h`道具来完成。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-container>
      <b-row align-h="start">
        <b-col cols="4">1</b-col>
        <b-col cols="4">2</b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们使用了`align-h`道具将项目向左对齐，值为`'start'`

其他值包括`'center'`，将内容居中对齐。

`'end'`将内容向右对齐。

`'around'`横向扩展内容。

`'between'`将内容放在边缘附近。

# 排序列

我们可以使用`order-*`道具来控制内容的视觉顺序。

这些道具反应灵敏。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col order="2">foo</b-col>
        <b-col order="1">bar</b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

因为有了`order`属性，所以‘foo’div 在‘bar’div 的右边。

我们可以写`order-md`、`order-sm`等。仅在遇到这些断点时更改顺序。

![](img/8a269e299e68e430a10b016e118bf808.png)

布鲁克·拉克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

有许多方法可以调整列的大小。

我们可以根据断点改变大小，这样我们就可以有响应的布局。

我们也可以在任何我们想要的地方添加空间。