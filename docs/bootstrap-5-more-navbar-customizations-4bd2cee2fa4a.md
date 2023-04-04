# Bootstrap 5 —更多导航栏定制

> 原文：<https://blog.devgenius.io/bootstrap-5-more-navbar-customizations-4bd2cee2fa4a?source=collection_archive---------4----------------------->

![](img/1181f4bfcbfeae5fb06ca53a77fff5ab.png)

照片由[波格丹一世·托多兰](https://unsplash.com/@todoranb_26?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 定制导航条。

# 文本

我们可以在导航条上添加文本。

例如，我们可以写:

```
<nav class="navbar navbar-light bg-light">
  <div class="container-fluid">
    <span class="navbar-text">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit.
    </span>
  </div>
</nav>
```

添加文本。

我们添加了`navbar-text`类来设置文本的样式以适应导航条。

此外，我们可以随意混合搭配其他实用程序类:

```
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">App</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarText" aria-controls="navbarText" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarText">
      <ul class="navbar-nav mr-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Profile</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Settings</a>
        </li>
      </ul>
      <span class="navbar-text">
        more text
      </span>
    </div>
  </div>
</nav>
```

我们有一个带有品牌和导航项目的导航条。

我们添加了`mr-auto`来给我们的 nav 增加右边距。

我们有`mb-2`和`mb-lg-0`类来根据屏幕的宽度改变下边距。

如果它很大，那么就没有下边距。

# 配色方案

navbar 的配色方案也可以改变。

我们可以添加`navbar-dark`和`bg-dark`类来使导航条变暗。

`navbar-dark`和`bg-primary`类使导航条变成蓝色。

我们也可以应用我们自己的风格。

例如，我们可以写:

```
<nav class="navbar navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">App</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarText" aria-controls="navbarText" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarText">
      <ul class="navbar-nav mr-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Profile</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Settings</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

添加黑色导航条。

这些类应用于根 navbar 元素。

我们也可以应用自己的导航栏颜色:

```
<nav class="navbar navbar-light" style="background-color: lightblue">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">App</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarText" aria-controls="navbarText" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarText">
      <ul class="navbar-nav mr-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Profile</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Settings</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

我们添加了`style`属性来做到这一点。

# 容器

我们可以添加一个`container`类来使导航栏在页面上居中。

例如，我们可以写:

```
<div class="container">
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">App</a>
    </div>
  </nav>
</div>
```

然后我们会在页面周围留一些空白。

此外，我们可以使用任何响应容器来改变导航栏的宽度:

```
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-md">
    <a class="navbar-brand" href="#">App</a>
  </div>
</nav>
```

我们添加了`navbar-expand-lg`类，使其在屏幕到达`lg`断点或更宽时展开。

# 安置

导航条的位置也可以改变。

例如，我们可以写:

```
<nav class="navbar fixed-top navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Fixed Navbar</a>
  </div>
</nav>
```

然后我们有了`fixed-top`类让导航条粘在顶部。

为了保持导航条在屏幕底部，我们可以写:

```
<nav class="navbar fixed-bottom navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Fixed Navbar</a>
  </div>
</nav>
```

`fixed-bottom`类将导航条放在底部。

我们也可以使用`sticky-top`类使它粘在顶部:

```
<nav class="navbar sticky-top navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Fixed Navbar</a>
  </div>
</nav>
```

![](img/4d5c885330dae83beb2e8135e85bc1ff.png)

由 [Andrew Schultz](https://unsplash.com/@andrewschultz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用不同的类来放置我们的导航条和它的内容。