# 引导程序 5 —导航条

> 原文：<https://blog.devgenius.io/bootstrap-5-navbars-5353dfd8e151?source=collection_archive---------2----------------------->

![](img/4d683e5393a724b5027d69c89def51a5.png)

[马库斯·温克勒](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何用 Bootstrap 5 添加导航条。

# 导航条

导航条让我们可以在应用程序中添加导航。

它反应灵敏，支持品牌、导航等等。

我们必须用`.navbar-expands-*`类包装`.navbar`来进行响应折叠。

`*`可以替换为`sm`、`md`、`lg`、`xl`和`xxl`中的一种。

也可以添加配色方案类。

Spacing 和 flex 实用程序类可用于控制导航栏中的间距和对齐方式。

导航条是默认响应的。

但是，这可以被覆盖。

# 支持的内容

有各种各样的内容可以添加到导航栏中。

`.navbar-brand`有品牌。

`.navbar-nav`有 nav。

`.navbar-toggle`让我们切换折叠的导航。

`.navbar-text`用于添加垂直居中的文本。

`.collapse-navbar-collapse`用于通过父断点对导航栏内容进行分组和隐藏。

为了补充所有这些，我们可以写:

```
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">App</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Link</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" data-toggle="dropdown">
            Dropdown
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">action 1</a></li>
            <li><a class="dropdown-item" href="#">action 2</a></li>
            <li>
              <hr class="dropdown-divider">
            </li>
            <li><a class="dropdown-item" href="#">action 3</a></li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled" href="#" tabindex="-1">disabled</a>
        </li>
      </ul>
      <form class="d-flex">
        <input class="form-control mr-2" type="search" placeholder="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>
```

在上面的例子中我们有所有这些类。

nav 元素有一个`navbar-expand-lg`类，当它遇到`lg`断点或更大的断点时，它会展开。

`navbar-light`使内容变淡。

`bg-light`使背景光变亮。

我们有一个带有`collapse`和`navbar-collapse`类的 div，当屏幕变窄时，它可以让导航条折叠起来。

此外，我们通过添加一个带有`dropdown`类的`li`元素来添加一个下拉列表。

`dropdown-menu`有菜单。

`dropdown-toggle`有拨动开关。

# 品牌

`.navbar-brand`可应用于大多数元素。

但是，我们自己可能要调整定位了。

例如，我们可以写:

```
<nav class="navbar navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">App</a>
  </div>
</nav>
```

添加带有品牌的导航栏。

我们有一个品牌项目的链接。

我们可以把它改成一个跨度:

```
<nav class="navbar navbar-light bg-light">
  <div class="container-fluid">
    <span class="navbar-brand mb-0 h1">App</span>
  </div>
</nav>
```

我们用`mb-0`类将下边距改为 0。

我们添加了`h1`类来改变文本的标题样式。

我们还可以添加图像作为品牌，而不是添加文本:

```
<nav class="navbar navbar-light bg-light">
  <div class="container">
    <a class="navbar-brand" href="#">
      <img src="http://placekitten.com/200/200" width="30" height="30" alt="cat" loading="lazy">
    </a>
  </div>
</nav>
```

我们添加一个具有给定宽度和高度的图像。

`loading`设置为`lazy`使其在显示时加载。

我们可以在图片旁边添加文字。

例如，我们可以写:

```
<nav class="navbar navbar-light bg-light">
  <div class="container">
    <a class="navbar-brand" href="#">
      <img src="http://placekitten.com/200/200" width="30" height="30" class="d-inline-block align-top" alt="cat" loading="lazy">
      Cat
    </a>
  </div>
</nav>
```

我们用`d-inline-block`类使图像内联显示，这样它将显示在文本旁边。

`align-top`使其与顶部对齐。

![](img/752619dca51951e00718a79f98d7ac36.png)

安娜·佩尔泽在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以添加包含各种内容的导航栏。

它可以包含表单、导航栏、品牌和下拉列表。