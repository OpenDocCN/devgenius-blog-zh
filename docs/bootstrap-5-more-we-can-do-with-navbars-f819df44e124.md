# bootstrap 5——我们可以用导航条做更多的事情

> 原文：<https://blog.devgenius.io/bootstrap-5-more-we-can-do-with-navbars-f819df44e124?source=collection_archive---------9----------------------->

![](img/ea8fd17dae1fa75c344cf83bb5d0587d.png)

克里斯·劳顿在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 添加 navbar 内容。

# 航行

我们可以在导航栏上添加导航链接。

为此，我们添加了一个包含`collapse`和`navbar-collapse`类的 div。

例如，我们可以写:

```
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link active" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Profile</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Setting</a>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

我们有一个包含`collapse`和`navbar-collapse`类的 div，用作导航链接的容器。

然后我们通过添加一个带有`navbar-toggler-icon`类的 span 来添加切换图标。

我们也有一个`navbar-toggler`类的按钮来添加导航条切换。

然后在`navbar-nav`中，我们用`.nav-item`类创建了 li 中的导航链接。

这将一起创建一个反应灵敏的导航条，当屏幕变窄时，导航条可以折叠，当屏幕变宽时，导航条可以展开。

我们也可以在导航栏中添加下拉菜单。

例如，我们可以写:

```
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">App</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNavDropdown">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link active"  href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Profile</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Settings</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" role="button" data-toggle="dropdown" >
            Dropdown link
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">action</a></li>
            <li><a class="dropdown-item" href="#">action 2</a></li>
            <li><a class="dropdown-item" href="#">action 3</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

我们通过用`.nav-item`和`dropdown`类创建一个`li`元素来添加下拉菜单。

这样，它会适合菜单。

此外，我们还有显示菜单的`.dropdown-menu`类。

`.dropdown-item`有下拉菜单项。

# 形式

我们也可以在导航栏上添加表单。

例如，我们可以写:

```
<nav class="navbar navbar-light bg-light">
  <div class="container-fluid">
    <form class="d-flex">
      <input class="form-control mr-2" type="search" placeholder="Search">
      <button class="btn btn-outline-success" type="submit">Search</button>
    </form>
  </div>
</nav>
```

将窗体添加到导航栏中。

我们有一个带有`d-flex`类的表单来显示 flex。

输入有`mr-2`添加一些右边距。

除了表格之外，我们还可以添加其他项目。

例如，我们可以写:

```
<nav class="navbar navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand">App</a>
    <form class="d-flex">
      <input class="form-control mr-2" type="search" placeholder="Search">
      <button class="btn btn-outline-success" type="submit">Search</button>
    </form>
  </div>
</nav>
```

我们用`navbar-brand`类添加了一个`a`元素，以在左侧添加品牌。

输入具有`form-control`类，因此它具有应用于它的引导样式。

输入组也可以使用导航条。

例如，我们可以写:

```
<nav class="navbar navbar-light bg-light">
  <form class="container-fluid">
    <div class="input-group">
      <span class="input-group-text">@</span>
      <input type="text" class="form-control" placeholder="Search">
    </div>
  </form>
</nav>
```

我们将`input-group`类应用于 div。

我们用`input-group-text`来添加文本。

此外，我们可以在导航栏上添加按钮。

例如，我们可以写:

```
<nav class="navbar navbar-light bg-light">
  <form class="container-fluid justify-content-start">
    <button class="btn btn-outline-success mr-2" type="button">big button</button>
    <button class="btn btn-sm btn-outline-secondary" type="button">small button</button>
  </form>
</nav>
```

在`form`元素中我们有不同大小的按钮。

该表单有`container-fluid`和`justify-content-start`来布置按钮。

`justify-content-start`使它们向左对齐。

左边的按钮有一个`mr-2`类，用来给按钮添加一些右边距。

![](img/c2643f99bcac828b70f9a65e88428b54.png)

照片由 [Davor Nisevic](https://unsplash.com/@davornisevic?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以在导航栏中添加表单、图片、下拉菜单和按钮。