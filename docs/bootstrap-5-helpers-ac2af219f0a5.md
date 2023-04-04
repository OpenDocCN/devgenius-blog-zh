# 引导程序 5 —助手

> 原文：<https://blog.devgenius.io/bootstrap-5-helpers-ac2af219f0a5?source=collection_archive---------6----------------------->

![](img/2ee3f3cb6c3d02ca54814969552be741.png)

赫尔墨斯·里维拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**写这篇文章时，Bootstrap 5 还处于 alpha 版本，可能会有变化。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在这篇文章中，我们将看看一些 Bootstrap 5 助手类。

# Clearfix

Clearfix 让我们可以轻松地清除容器中的浮动内容。

为了添加它，我们将`.clearfix`类添加到父元素中。

它也可以用作混音。

例如，我们可以写:

```
.element {
  @include clearfix;
}
```

我们可以在 HTML 中使用它:

```
<div class="bg-info clearfix">
  <button type="button" class="btn btn-secondary float-left">floated left</button>
  <button type="button" class="btn btn-secondary float-right">floated right</button>
</div>
```

# 彩色链接

我们可以用`.link-*`类添加彩色链接来给链接着色。

例如，我们可以写:

```
<a href="#" class="link-primary">Primary</a>
<a href="#" class="link-secondary">Secondary</a>
<a href="#" class="link-success">Success</a>
<a href="#" class="link-danger">Danger</a>
<a href="#" class="link-warning">Warning</a>
<a href="#" class="link-info">Info</a>
<a href="#" class="link-light">Light</a>
<a href="#" class="link-dark">Dark</a>
```

添加链接。

# 使...嵌入

Bootstrap 5 带有一个组件，可以根据父对象的宽度创建响应视频或幻灯片嵌入。

它们直接应用于`iframe`、`embded`、`video`和`object`。

而且我们也不需要加`frameborder='0'`去掉边框。

例如，我们可以写:

```
<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/IA7REQxohV4" title="YouTube video" allowfullscreen></iframe>
</div>
```

我们有`title`来设置标题。

`allowfullscreen`让我们以全屏模式显示视频。

# 宽比

我们可以改变嵌入的长宽比。

为此，我们添加了一些类:

```
<div class="embed-responsive embed-responsive-21by9">
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/IA7REQxohV4" title="YouTube video" allowfullscreen></iframe>
</div>
```

我们有`embed-responsive-*`类来改变长宽比。

此外，我们可以将嵌入比率更改为其他比率:

```
<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/IA7REQxohV4" title="YouTube video" allowfullscreen></iframe>
</div><div class="embed-responsive embed-responsive-4by3">
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/IA7REQxohV4" title="YouTube video" allowfullscreen></iframe>
</div><div class="embed-responsive embed-responsive-1by1">
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/IA7REQxohV4" title="YouTube video" allowfullscreen></iframe>
</div>
```

我们可以将比例更改为 16 x9、4x3 或 1x1。

# 位置

Bootstrap 5 还附带了改变位置的助手。

例如，我们可以写:

```
<div class="fixed-top">...</div>
```

将 div 的位置固定在顶部。

此外，我们可以写:

```
<div class="fixed-bottom">...</div>
```

把它固定在底部。

我们也可以用`.sticky-top`类来制作`position: sticky`:

```
<div class="sticky-top">...</div>
```

我们可以做出响应:

```
<div class="sticky-sm-top">Stick small or wider</div><div class="sticky-md-top">Stick medium or wider</div><div class="sticky-lg-top">Stick large or wider</div><div class="sticky-xl-top">Stick extra large or wider</div>
```

# 屏幕阅读器

屏幕阅读器有实用程序类。

Bootstrap 5 附带了`sr-only`类，使文本仅对屏幕阅读器可用。

例如，我们可以写:

```
<h2 class="sr-only">screen readers</h2>
```

`.sr-only-focusable`仅当聚焦时显示元素:

```
<a class="sr-only-focusable" href="#content">main content</a>
```

# 拉伸连杆

我们可以用`.stretched-link`类拉伸链接。

例如，我们可以写:

```
<div class="card" style="width: 20rem;">
  <img src="http://placekitten.com/200/200" class="card-img-top" alt="cat">
  <div class="card-body">
    <h5 class="card-title">Card with stretched link</h5>
    <p class="card-text">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent tempus turpis augue, aliquam egestas nunc semper sit amet. Vivamus vel euismod eros. Fusce aliquam vitae nisi quis suscipit. Nullam rutrum convallis dui, a pharetra nulla dapibus eget. In diam justo, finibus in commodo fermentum, maximus sit amet tellus. .</p>
    <a href="#" class="btn btn-primary stretched-link">Go somewhere</a>
  </div>
</div>
```

因为我们在链接上有了`stretch-link`类，它将被拉伸到卡片的宽度。

包含块有`position: relative`，所以我们不需要添加它来拉伸链接。

我们必须给大多数组件添加`position: relative`来使链接伸展。

我们可以这样写:

```
<div class="d-flex position-relative">
  <img src="http://placekitten.com/200/200" class="flex-shrink-0 mr-3" alt="cat">
  <div>
    <h5 class="mt-0">Custom component with stretched link</h5>
    <p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent tempus turpis augue, aliquam egestas nunc semper sit amet. Vivamus vel euismod eros. Fusce aliquam vitae nisi quis suscipit. Nullam rutrum convallis dui, a pharetra nulla dapibus eget. In diam justo, finibus in commodo fermentum, maximus sit amet tellus. </p>
    <a href="#" class="stretched-link">Go</a>
  </div>
</div>
```

我们将`position-relative`类添加到容器 div 中，使链接可以伸展。

![](img/51e270dc66735eeb6428a704d49ee750.png)

照片由 [niko photos](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

Bootstrap 5 为我们提供了各种造型的帮手。