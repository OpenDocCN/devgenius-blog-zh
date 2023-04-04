# 引导程序 5 —列表和图像

> 原文：<https://blog.devgenius.io/bootstrap-5-lists-and-images-324a1ed6f35c?source=collection_archive---------16----------------------->

![](img/ef1ba815dd19f423bfb6ee6adecd274f.png)

照片由[孙平](https://unsplash.com/@vladington?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何使用 Bootstrap 5 设计列表和图像的样式。

# 列表

Bootstrap 5 删除列表项的默认`list-style`和左边距。

这只适用于直接子列表项。

必须将该类添加到嵌套列表中。

例如，我们可以连接:

```
<ul class="list-unstyled">
  <li>Lorem ipsum dolor sit amet</li>
  <li>Duis id nunc dignissim</li>
  <li>Nulla volutpat aliquam velit
    <ul>
      <li>Phasellus iaculis neque</li>
      <li> Suspendisse potenti. Aliquam erat volutpat.</li>
    </ul>
  </li>
  <li>Pellentesque habitant morbi tristique senectus</li>
  <li>Aenean sit amet erat nunc</li>
  <li>Eget porttitor lorem</li>
</ul>
```

添加嵌套列表。

我们添加了`list-unstyled`类来添加一个无样式列表。

# 在一条直线上的

我们可以删除一个列表的项目符号，并用`.list-inline`和`.list-inline-item`类的组合来应用一些光线`margin`:

```
<ul class="list-inline">
  <li class="list-inline-item">Lorem ipsum dolor sit amet, consectetur adipiscing elit.</li>
  <li class="list-inline-item">Pellentesque quis nunc ultricies enim ullamcorper pretium at cursus tellus.</li>
  <li class="list-inline-item"> Ut convallis quis lacus in volutpat. Pellentesque volutpat dui et enim mattis</li>
</ul>
```

我们添加了这些类，以使项目与边距内联显示。

# 描述列表对齐

为了制作一个带有描述的列表，我们可以使用带有宽度样式的`dl`、`dt`和`dd`标签。

例如，我们可以写:

```
<dl class="row">
  <dt class="col-sm-3">Description lists</dt>
  <dd class="col-sm-9">A description list is perfect for defining terms.</dd>

  <dt class="col-sm-3">Euismod</dt>
  <dd class="col-sm-9">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
    <p>ellentesque quis nunc ultricies enim ullamcorper pretium at cursus tellus. Curabitur sit amet leo arcu. Integer vitae tincidunt odio. Duis id nunc dignissim.</p>
  </dd>

  <dt class="col-sm-3">Maecenas imperdiet sapien augue, ac malesuada metus ultrices et.</dt>
  <dd class="col-sm-9">Aliquam erat volutpat. Quisque rutrum commodo felis imperdiet fermentum. Integer hendrerit dictum augue dapibus semper. In ac nisi consectetur.</dd>

  <dt class="col-sm-3 text-truncate">Truncated term is truncated</dt>
  <dd class="col-sm-9">Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.</dd>

  <dt class="col-sm-3">Nesting</dt>
  <dd class="col-sm-9">
    <dl class="row">
      <dt class="col-sm-4">Nested definition list</dt>
      <dd class="col-sm-8">Aenean dolor augue, vulputate non mattis eu, lacinia viverra ex.</dd>
    </dl>
  </dd>
</dl>
```

我们有一个带有`dt`和`dd`标签的列表，带有列类来设置它们的宽度。

# 形象

我们可以用 img 元素和`.img-fluid`类添加响应图像。

例如，我们可以写:

```
<img src="http://placekitten.com/200/200" class="img-fluid" alt="cat">
```

# 图像缩略图

要添加缩略图，我们使用`.img-thumbnail`类:

```
<img src="http://placekitten.com/200/200" class="img-thumbnail" alt="cat">
```

# 对齐图像

我们可以将图像与`float-left`或`float-right`类对齐:

```
<img src="http://placekitten.com/200/200" class="rounded float-left" alt="cat">
<img src="http://placekitten.com/200/200" class="rounded float-right" alt="cat">
```

要使它水平居中对齐，我们可以写:

```
<img src="http://placekitten.com/200/200" class="rounded mx-auto d-block" alt="cat">
```

我们使用了`mx-auto`类来使图像居中。

同样，我们可以使用`text-center`类来做同样的事情:

```
<div class="text-center">
  <img src="http://placekitten.com/200/200" class="rounded" alt="cat">
</div>
```

# 画

`picture`元素可用于指定多个图像源。

所以我们可以写:

```
​<picture>
  <img src="http://placekitten.com/200/200" class="img-fluid img-thumbnail" alt="cat 1">
  <img src="http://placekitten.com/201/201" class="img-fluid img-thumbnail" alt="cat 2">
</picture>
```

我们将`img`元素放在`picture`标签中。

我们必须在`img`标签上添加`img-*`类，而不是`picture`标签。

![](img/831f34800ef4ff4a609ec1912187c674.png)

奥拉因卡·巴巴罗拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用 Bootstrap 5 设计列表和图像的样式。

这是我们在课上学到的。