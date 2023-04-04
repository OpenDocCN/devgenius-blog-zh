# 引导程序 5 —进度条

> 原文：<https://blog.devgenius.io/bootstrap-5-progress-bar-e204859350a3?source=collection_archive---------12----------------------->

![](img/04affefb91e871f8eb24bf4fb155d020.png)

照片由[金青铜](https://unsplash.com/@26yashm12?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何用 Bootstrap 5 添加进度条。

# 进度条

Bootstrap 5 自带进度条样式。

我们可以在应用程序中使用它们。

为了使用它，我们使用`progress`类作为包装器来指示进度的最大值。

`progress-bar`类表示到目前为止的进度。

它需要内联样式、实用程序类或自定义 CSS 来设置宽度。

例如，我们可以写:

```
<div class="progress">
  <div class="progress-bar"></div>
</div>
<div class="progress">
  <div class="progress-bar" style="width: 20%"></div>
</div>
<div class="progress">
  <div class="progress-bar" style="width: 40%"></div>
</div>
<div class="progress">
  <div class="progress-bar" style="width: 60%"></div>
</div>
<div class="progress">
  <div class="progress-bar" style="width: 80%"></div>
</div>
<div class="progress">
  <div class="progress-bar" style="width: 100%"></div>
</div>
```

我们添加了不同数量的进度条，其中填充了`progress`和`progress-bar`类。

`progress-bar`显示一个进度栏。我们设置`width`样式来调整填充的量。

或者，我们可以使用宽度实用程序类来设置宽度。

例如，我们可以写:

```
<div class="progress">
  <div class="progress-bar"></div>
</div>
<div class="progress">
  <div class="progress-bar w-25"></div>
</div>
<div class="progress">
  <div class="progress-bar w-50"></div>
</div>
<div class="progress">
  <div class="progress-bar w-75"></div>
</div>
<div class="progress">
  <div class="progress-bar w-100"></div>
</div>
```

我们使用`w-*`类来设置进度条的宽度。

# 标签

我们可以在进度条中添加标签。

例如，我们可以写:

```
<div class="progress">
  <div class="progress-bar"></div>
</div>
<div class="progress">
  <div class="progress-bar w-25">25%</div>
</div>
<div class="progress">
  <div class="progress-bar w-50">50%</div>
</div>
<div class="progress">
  <div class="progress-bar w-75">75%</div>
</div>
<div class="progress">
  <div class="progress-bar w-100">100%</div>
</div>
```

在栏内用文本显示进度。

# 高度

我们可以用`progress`类在元素上设置`height`样式。

例如，我们可以写:

```
<div class="progress" style="height: 20px;">
  <div class="progress-bar"></div>
</div>
<div class="progress" style="height: 20px;">
  <div class="progress-bar w-25">25%</div>
</div>
<div class="progress" style="height: 20px;">
  <div class="progress-bar w-50">50%</div>
</div>
<div class="progress" style="height: 20px;">
  <div class="progress-bar w-75">75%</div>
</div>
<div class="progress" style="height: 20px;">
  <div class="progress-bar w-100">100%</div>
</div>
```

现在每个条的高度都是 20px 高。

# 背景

我们可以使用 Bootstrap 5 类来改变背景样式。

例如，我们可以写:

```
<div class="progress">
  <div class="progress-bar bg-success"></div>
</div>
<div class="progress">
  <div class="progress-bar w-25 bg-info"></div>
</div>
<div class="progress">
  <div class="progress-bar w-50 bg-warning"></div>
</div>
<div class="progress">
  <div class="progress-bar w-75 bg-danger"></div>
</div>
<div class="progress">
  <div class="progress-bar w-100 bg-secondary"></div>
</div>
```

改变风格。

# 多个条形

我们可以在一个进度条容器中有多个进度条。

例如，我们可以写:

```
<div class="progress">
  <div class="progress-bar" style="width: 15%"></div>
  <div class="progress-bar bg-success" style="width: 35%"></div>
  <div class="progress-bar bg-info" style="width: 20%"></div>
</div>
```

我们设置`width`样式来设置条形的宽度。

它们会自动并排放置。

# 有斑纹的

进度条可以有条纹。

为了添加它们，我们添加了`progress-bar-striped`类:

```
<div class="progress">
  <div class="progress-bar progress-bar-striped" style="width: 10%"></div>
</div><div class="progress">
  <div class="progress-bar progress-bar-striped bg-success" style="width: 20%"></div>
</div><div class="progress">
  <div class="progress-bar progress-bar-striped bg-info" style="width: 40%"></div>
</div><div class="progress">
  <div class="progress-bar progress-bar-striped bg-warning" style="width: 60%" a></div>
</div><div class="progress">
  <div class="progress-bar progress-bar-striped bg-secondary" style="width: 80%" a></div>
</div><div class="progress">
  <div class="progress-bar progress-bar-striped bg-danger" style="width: 100%"></div>
</div>
```

我们添加了不同风格的进度条。

它们都有`progress-bar-striped`类将条纹添加到进度条的填充部分。

# 动画条纹

条纹可以是动画的。

例如，我们可以写:

```
<div class="progress">
  <div class="progress-bar progress-bar-striped progress-bar-animated" style="width: 75%"></div>
</div>
```

用`progress-bar-animated`和`progress-bar-striped`类制作条纹动画。

![](img/96e378a164dc785d55f2abc7204e5f36.png)

Nathalie SPEHNER 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以添加不同长度和样式的进度条。

它们里面也可以有标签。