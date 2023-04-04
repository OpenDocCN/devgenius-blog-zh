# 引导 5-列出组

> 原文：<https://blog.devgenius.io/bootstrap-5-list-groups-40186476525d?source=collection_archive---------18----------------------->

![](img/04a74b639f835ac26ae873f0133ffaee.png)

由 [Hermes Rivera](https://unsplash.com/@hermez777?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何使用 Bootstrap 5 添加列表组。

# 列表组

我们可以使用列表组来显示列表中的项目。

要添加一个，我们只需用`.list-group`类创建一个`ul`。

例如，我们可以写:

```
<ul class="list-group">
  <li class="list-group-item">Lorem ipsum dolor sit amet</li>
  <li class="list-group-item">consectetur adipiscing elit</li>
  <li class="list-group-item">Proin non libero congue lectus</li>
  <li class="list-group-item">pharetra dictum quis ut eros</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

`.list-group`使列表显示时不带布尔值，用线条分隔项目。

`.list-group-item`是物品类。

# 活动项目

我们可以用`.active`类将一个项目设置为活动的。

例如，我们可以写:

```
<ul class="list-group">
  <li class="list-group-item active">Lorem ipsum dolor sit amet</li>
  <li class="list-group-item">consectetur adipiscing elit</li>
  <li class="list-group-item">Proin non libero congue lectus</li>
  <li class="list-group-item">pharetra dictum quis ut eros</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

# 禁用的项目

要添加禁用项目，我们可以添加`disabled`类:

```
<ul class="list-group">
  <li class="list-group-item disabled">Lorem ipsum dolor sit amet</li>
  <li class="list-group-item">consectetur adipiscing elit</li>
  <li class="list-group-item">Proin non libero congue lectus</li>
  <li class="list-group-item">pharetra dictum quis ut eros</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

# 链接和按钮

列表组项目可以是链接。

要添加它们，我们可以将容器改为一个 div，这样我们就可以直接添加`a`元素:

```
<div class="list-group">
  <a href="#" class="list-group-item list-group-item-action active">
    Lorem ipsum dolor sit amet,
  </a>
  <a href="#" class="list-group-item list-group-item-action">consectetur adipiscing elit</a>
  <a href="#" class="list-group-item list-group-item-action">Proin non libero congue lectus</a>
  <a href="#" class="list-group-item list-group-item-action">pharetra dictum quis ut eros</a>
  <a href="#" class="list-group-item list-group-item-action disabled" tabindex="-1">Vestibulum at eros</a>
</div>
```

我们添加了一个包含`a`元素的 div。

课程是一样的。

我们添加了`list-group-item-action`使它们可以点击。

# 脸红

要移除边框，我们可以添加`list-group-flush`类。

例如，我们可以写:

```
<ul class="list-group list-group-flush">
  <li class="list-group-item">Lorem ipsum dolor sit amet</li>
  <li class="list-group-item">consectetur adipiscing elit</li>
  <li class="list-group-item">Proin non libero congue lectus</li>
  <li class="list-group-item">pharetra dictum quis ut eros</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

现在我们不会看到外部边界，但每个条目的底部边界会保留。

# 水平的

我们可以添加`.list-group-horizontal`类来改变列表组项目的布局。

还有响应式变体，即`.list-group-horizontal-*`类家族。

其中`*`可以替换为`sm`、`md`、`lg`、`xl`或`xxl`等断点。

如果我们想要 equal with group 项，那么我们可以将`.flex-fill`类添加到每个列表组项中。

例如，我们可以写:

```
<ul class="list-group list-group-horizontal">
  <li class="list-group-item">Lorem ipsum dolor sit amet</li>
  <li class="list-group-item">consectetur adipiscing elit</li>
  <li class="list-group-item">Morbi leo risus</li>
</ul><ul class="list-group list-group-horizontal-sm">
  <li class="list-group-item">Lorem ipsum dolor sit amet</li>
  <li class="list-group-item">consectetur adipiscing elit</li>
  <li class="list-group-item">Morbi leo risus</li>
</ul><ul class="list-group list-group-horizontal-md">
  <li class="list-group-item">Lorem ipsum dolor sit amet</li>
  <li class="list-group-item">consectetur adipiscing elit</li>
  <li class="list-group-item">Morbi leo risus</li>
</ul>
```

我们在每个列表上都有`list-group-horizontal-md`类，这样当屏幕达到`md`断点或更宽时，它们会并排显示。

# 上下文类别

我们可以在列表中添加各种样式变体。

我们添加了`.list-group-item-*`类家族来设计项目的样式。

例如，我们可以写:

```
<ul class="list-group">
  <li class="list-group-item">no style</li>
  <li class="list-group-item list-group-item-primary">primary</li>
  <li class="list-group-item list-group-item-secondary">secondary</li>
  <li class="list-group-item list-group-item-success">success</li>
  <li class="list-group-item list-group-item-danger">danger</li>
  <li class="list-group-item list-group-item-warning">warning</li>
  <li class="list-group-item list-group-item-info">info</li>
  <li class="list-group-item list-group-item-light">light</li>
  <li class="list-group-item list-group-item-dark">dark</li>
</ul>
```

我们在每一项中添加用于样式化的类。

![](img/f78cc29cbb22633d5bcfe869c741c270.png)

由 [Raphael Nogueira](https://unsplash.com/@phaelnogueira?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用列表组来显示列表中的项目。