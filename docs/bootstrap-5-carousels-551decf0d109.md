# Bootstrap 5 —旋转式传送带

> 原文：<https://blog.devgenius.io/bootstrap-5-carousels-551decf0d109?source=collection_archive---------13----------------------->

![](img/df2118f64994325b7178f9194d97f525.png)

照片由 [Max Bender](https://unsplash.com/@maxwbender?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何用 Bootstrap 5 添加传送带。

# 旋转木马

转盘是一个幻灯片组件，用于在元素间循环。

幻灯片可以有图像或文本。

如果浏览器支持页面可见性 API，那么当用户看不到屏幕时，轮播将停止滑动。

# 仅幻灯片

我们可以添加一个带有 div 的 carousel，div 包含`.carousel`和`,slide`类。

例如，我们可以写:

```
<div class="carousel slide" data-ride="carousel">
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div>
    <div class="carousel-item">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div>
    <div class="carousel-item">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div>
  </div>
</div>
```

我们有带`carousel`和`slide`类的 div。

此外，我们添加了值为`carousel`的`.data-ride`属性来添加旋转木马。

此外，我们有一个包含`carousel-inner`类的 div 来保存项目。

为了添加条目，我们使用`carousel-item`类添加 div。

`active`使其成为活动幻灯片。

# 带控件

我们可以添加一个带有上一个和下一个控件的转盘。

我们通过在 slides div 之外添加`a`元素来添加它们。

例如，我们写道:

```
<div id="carousel" class="carousel slide" data-ride="carousel">
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div> <div class="carousel-item">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div> <div class="carousel-item">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div>
  </div> <a class="carousel-control-prev" href="#carousel" data-slide="prev">
    <span class="carousel-control-prev-icon"></span>
    <span class="sr-only">Previous</span>
  </a> <a class="carousel-control-next" href="#carousel" data-slide="next">
    <span class="carousel-control-next-icon"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
```

我们有 2 个`a`元素，其中`href`设置为转盘的选择器。

此外，我们有`data-slide`属性来表示我们有什么样的控件。

在它里面，我们有可以点击的图标和屏幕阅读器的文本。

# 带指标

除了幻灯片和控件之外，我们还可以添加幻灯片指示器。

为此，我们用`.carousel-indicators`类添加了一个`ol`。

然后在`li`标签中，我们将`data-target`属性设置为转盘的选择器。

例如，我们可以写:

```
<div id="carousel" class="carousel slide" data-ride="carousel">
  <ol class="carousel-indicators">
    <li data-target="#carousel" data-slide-to="0" class="active"></li>
    <li data-target="#carousel" data-slide-to="1"></li>
    <li data-target="#carousel" data-slide-to="2"></li>
  </ol> <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div> <div class="carousel-item">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div> <div class="carousel-item">
      <img src="http://placekitten.com/200/200" class="d-block w-100" alt="cat">
    </div>
  </div> <a class="carousel-control-prev" href="#carousel" data-slide="prev">
    <span class="carousel-control-prev-icon"></span>
    <span class="sr-only">Previous</span>
  </a> <a class="carousel-control-next" href="#carousel" data-slide="next">
    <span class="carousel-control-next-icon"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
```

我们有带`li` s 的`ol`元素。

`data-target`属性被设置为`#carousel`以显示正确的滑动指示器。

`data-slide-to`有幻灯片要滑动到的索引。

# 带字幕

此外，我们可以给幻灯片添加标题。

例如，我们可以写:

```
<div id="carousel" class="carousel slide" data-ride="carousel">
  <ol class="carousel-indicators">
    <li data-target="#carousel" data-slide-to="0" class="active"></li>
    <li data-target="#carousel" data-slide-to="1"></li>
    <li data-target="#carousel" data-slide-to="2"></li>
  </ol> <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="http://placekitten.com/600/600" class="d-block w-100" alt="cat">
      <div class="carousel-caption d-none d-md-block">
        <h5>First slide label</h5>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque magna dui, mattis ornare massa a, dapibus iaculis est. Lorem ipsum dolor sit amet, consectetur adipiscing elit. </p>
      </div>
    </div> <div class="carousel-item">
      <img src="http://placekitten.com/600/600" class="d-block w-100" alt="cat">
      <div class="carousel-caption d-none d-md-block">
        <h5>Second slide label</h5>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque magna dui, mattis ornare massa a, dapibus iaculis est. Lorem ipsum dolor sit amet, consectetur adipiscing elit. </p>
      </div>
    </div> <div class="carousel-item">
      <img src="http://placekitten.com/600/600" class="d-block w-100" alt="cat">
      <div class="carousel-caption d-none d-md-block">
        <h5>Third slide label</h5>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque magna dui, mattis ornare massa a, dapibus iaculis est. Lorem ipsum dolor sit amet, consectetur adipiscing elit. .</p>
      </div>
    </div>
  </div> <a class="carousel-control-prev" href="#carousel" role="button" data-slide="prev">
    <span class="carousel-control-prev-icon"></span>
    <span class="sr-only">Previous</span>
  </a> <a class="carousel-control-next" href="#carousel" data-slide="next">
    <span class="carousel-control-next-icon"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
```

给我们的幻灯片添加标题。

我们用`.carousel-caption`类添加了 div 来添加幻灯片标题。

![](img/58ba0fc05d19fe033485ce8344b4dfbb.png)

由 [ckturistando](https://unsplash.com/@ckturistando?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以添加带有幻灯片、控件和标题的传送带。