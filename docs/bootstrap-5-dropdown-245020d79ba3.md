# Bootstrap 5 —动态下拉菜单

> 原文：<https://blog.devgenius.io/bootstrap-5-dropdown-245020d79ba3?source=collection_archive---------4----------------------->

![](img/51a356732e4977cd0d8031d1c8dcfbee.png)

图片由 [Ella Olsson](https://unsplash.com/@ellaolsson?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 JavaScript 操作下拉菜单，以及如何使用 Bootstrap 5 添加内容。

# 文本

我们可以在下拉菜单中添加任何文本。

例如，我们可以写:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown" data-display="static">
    button
  </button>
  <div class="dropdown-menu p-4 text-muted">
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin non libero congue lectus pharetra dictum quis ut eros. Nunc ac eleifend risus. In vitae arcu tincidunt, eleifend velit id, cursus nibh. Vestibulum hendrerit urna dictum, vulputate quam et, faucibus sapien. Suspendisse in mauris non diam facilisis aliquet eget ac nisi. Donec nec elit vel ex pellentesque pellentesque. Sed commodo tellus at enim vulputate ornare. Pellentesque vel elit ut libero hendrerit dictum a in dolor. Suspendisse cursus justo nulla, ac malesuada orci pretium quis. Sed sed nunc in lacus hendrerit consequat.
  </div>
</div>
```

向下拉菜单中添加带填充的文本。

# 形式

表单也可以放在菜单里面。

例如，我们可以写:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown" data-display="static">
    button
  </button>
  <div class="dropdown-menu p-1 text-muted">
    <form class="px-4 py-3">
      <div class="mb-3">
        <label for="email" class="form-label">Email</label>
        <input type="email" class="form-control" id="email" placeholder="email">
      </div>
      <div class="mb-3">
        <label for="password" class="form-label">Password</label>
        <input type="password" class="form-control" id="password" placeholder="Password">
      </div>
      <div class="mb-3">
        <div class="form-check">
          <input type="checkbox" class="form-check-input" id="check">
          <label class="form-check-label" for="check">
            Remember me
          </label>
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Sign in</button>
    </form>
  </div>
</div>
```

在菜单中添加表单。

我们只需将表单直接放入下拉菜单容器中。

# 下拉选项

我们可以用`data-offset`属性改变下拉列表的位置。

例如，我们可以写:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown" data-offset="10,20">
    button
  </button>
  <ul class="dropdown-menu dropdown-menu-lg-left">
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>      
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

将菜单移动到远离切换按钮的位置。

这些数字是我们想要移动菜单的 x 和 y 的像素距离。

我们可以使用`data-reference`属性将其更改为父属性:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown" data-reference="parent">
    button
  </button>
  <ul class="dropdown-menu dropdown-menu-lg-left">
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>      
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

# Java Script 语言

我们可以通过 JavaScript 调用下拉菜单。

例如，给定以下下拉列表:

```
<div class="btn-group">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-toggle="dropdown">
    button
  </button>
  <ul class="dropdown-menu dropdown-menu-lg-left">
    <li><button class="dropdown-item" type="button">foo</button></li>
    <li><button class="dropdown-item" type="button">bar</button></li>      
    <li><button class="dropdown-item" type="button">baz</button></li>
  </ul>
</div>
```

我们可以通过书写来显示菜单:

```
const dropdownToggleEl = document.querySelector('.dropdown-toggle')
const dropdownList = new bootstrap.Dropdown(dropdownToggleEl);
dropdownList.show();
```

我们通过选择带有`.dropdown-toggle`类的元素得到下拉菜单。

然后我们将它传递给`bootstrap.Dropdown`构造函数。

然后我们在返回的对象上调用`show`来打开菜单。

其他方法包括`toggle`切换菜单。

`hide`隐藏菜单。

`update`更新下拉列表的位置。

`dispose`破坏下拉菜单。

获取下拉列表实例。

# 事件

dropdown 元素也发出一些事件。

当下拉菜单显示时，它发出`show.bs.dropdown`事件。

显示时会发出`shown.bs.dropdown`。

`hide.bs.dropdown`隐藏时发出。

`hidden.bs.dropdown`隐藏时发出。

我们可以用`addEventListener`的方法来听:

```
const myDropdown = document.getElementById('myDropdown')
myDropdown.addEventListener('show.bs.dropdown', () => {
  // do something...
})
```

![](img/565320270ce71b54ddc3add4325aaada.png)

照片由[赫尔墨斯·里维拉](https://unsplash.com/@hermez777?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以在下拉菜单中添加表单和文本。

此外，我们可以监听它发出的事件，并用 JavaScript 操纵它。