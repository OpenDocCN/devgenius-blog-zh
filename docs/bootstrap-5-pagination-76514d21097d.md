# 引导程序 5 —分页

> 原文：<https://blog.devgenius.io/bootstrap-5-pagination-76514d21097d?source=collection_archive---------10----------------------->

![](img/375224581e620aff6e06f7f497facbef.png)

[王帅文](https://unsplash.com/@ryan_zy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会发生变化。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何用 Bootstrap 5 添加分页按钮。

# 页码

如果有一系列内容跨越多个页面，我们可以显示分页按钮，让用户导航到不同的页面。

例如，我们可以写:

```
<nav>
  <ul class="pagination">
    <li class="page-item"><a class="page-link" href="#">Previous</a></li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item"><a class="page-link" href="#">2</a></li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item"><a class="page-link" href="#">Next</a></li>
  </ul>
</nav>
```

我们有一个内部带有`ul`的导航元素。

`li`有`.page-item`类给它们添加边框。

每个分页链接都有`.page-link`类来设置它们的样式。

# 使用图标

我们可以给`a`标签添加图标。

例如，我们可以写:

```
<nav>
  <ul class="pagination">
    <li class="page-item">
      <a class="page-link" href="#">
        <span>&laquo;</span>
      </a>
    </li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item"><a class="page-link" href="#">2</a></li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item">
      <a class="page-link" href="#">
        <span>&raquo;</span>
      </a>
    </li>
  </ul>
</nav>
```

我们有第一个和最后一个分页链接，里面有图标而不是普通文本。

# 禁用和活动状态

我们可以将链接的状态设置为禁用或活动。

例如，我们可以写:

```
<nav>
  <ul class="pagination">
    <li class="page-item disabled">
      <a class="page-link" href="#" tabindex="-1">Previous</a>
    </li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item active">
      <a class="page-link" href="#">2</a>
    </li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
```

我们禁用了之前与`disabled`类的链接。

我们用`active`类将 2 链接设置为活动的。

我们可以将活动或禁用的锚点替换为跨度。

可以在上一个或下一个箭头中省略锚点。

这些都删除了点击功能，并防止键盘焦点，同时保留正确的风格。

例如，我们可以写:

```
<nav>
  <ul class="pagination">
    <li class="page-item disabled">
      <span class="page-link">Previous</span>
    </li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item active">
      <span class="page-link">
        2        
      </span>
    </li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
```

我们用之前的跨度和 2 个按钮替换锚点。

现在我们无法点击或关注它们。

# 胶料

分页栏的大小可以随`pagination-lg`或`pagination-sm`类而改变。

例如，我们可以写:

```
<nav>
  <ul class="pagination pagination-lg">
    <li class="page-item disabled">
      <span class="page-link">Previous</span>
    </li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item active" aria-current="page">
      <span class="page-link">
        2        
      </span>
    </li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
```

使其变大，并且:

```
<nav>
  <ul class="pagination pagination-sm">
    <li class="page-item disabled">
      <span class="page-link">Previous</span>
    </li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item active" aria-current="page">
      <span class="page-link">
        2        
      </span>
    </li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
```

让它变小。

# 对齐

分页栏的对齐方式可以改变。

例如，我们可以写:

```
<nav>
  <ul class="pagination justify-content-center">
    <li class="page-item disabled">
      <span class="page-link">Previous</span>
    </li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item active" aria-current="page">
      <span class="page-link">
        2        
      </span>
    </li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
```

将分页栏居中。

![](img/70de42c4fe2b7c03c3b416a2afd393bf.png)

由 [Ramesh Casper](https://unsplash.com/@rameshcasper?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以添加一个分页栏来显示导航到不同页面的链接。