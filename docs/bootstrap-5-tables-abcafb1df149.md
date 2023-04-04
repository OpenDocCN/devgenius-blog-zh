# 引导数据库 5 —表格

> 原文：<https://blog.devgenius.io/bootstrap-5-tables-abcafb1df149?source=collection_archive---------24----------------------->

![](img/4de2c46cbcba030f3fbb4aad57cd70ca.png)

照片由[斯蒂芬·孙平](https://unsplash.com/@vladington?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 设计表格样式。

# 桌子

由于表格在其他 UI 组件中的广泛使用，引导表样式是可选的。

Bootstrap 中不会继承表格样式。

因此嵌套表可以是独立于父表的样式。

例如，我们可以使用`table`类来设计表格的样式。

我们可以通过编写以下内容来创建一个简单的表:

```
<table class="table">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>james</td>
      <td>smith</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>mary</td>
      <td>jones</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td colspan="2">Larry</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
```

# 变体

一个表格有许多不同的样式。

我们可以用下面的类来设计它们的样式:

```
<table class="table-primary">...</table>
<table class="table-secondary">...</table>
<table class="table-success">...</table>
<table class="table-danger">...</table>
<table class="table-warning">...</table>
<table class="table-info">...</table>
<table class="table-light">...</table>
<table class="table-dark">...</table>
```

他们也在行上工作:

```
<tr class="table-primary">...</tr>
<tr class="table-secondary">...</tr>
<tr class="table-success">...</tr>
<tr class="table-danger">...</tr>
<tr class="table-warning">...</tr>
<tr class="table-info">...</tr>
<tr class="table-light">...</tr>
<tr class="table-dark">...</tr>
```

他们还处理表格单元格:

```
<tr>
  <td class="table-primary">...</td>
  <td class="table-secondary">...</td>
  <td class="table-success">...</td>
  <td class="table-danger">...</td>
  <td class="table-warning">...</td>
  <td class="table-info">...</td>
  <td class="table-light">...</td>
  <td class="table-dark">...</td>
</tr>
```

# 重音表

我们可以添加`.table-striped`类来为`tbody`中的表格行添加斑马条纹。

例如，我们可以写:

```
<table class="table table-striped">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>james</td>
      <td>smith</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>mary</td>
      <td>jones</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td colspan="2">Larry</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
```

条纹也适用于其他表格变体:

```
<table class="table table-dark table-striped">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>james</td>
      <td>smith</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>mary</td>
      <td>jones</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td colspan="2">Larry</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
```

# 可悬停的行

为了使行可悬浮，我们可以添加`table-hover`类:

```
<table class="table table-hover">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>james</td>
      <td>smith</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>mary</td>
      <td>jones</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td colspan="2">Larry</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
```

现在我们看到我们悬停的行突出显示。

悬停效果可以与条纹相结合:

```
<table class="table table-striped table-hover">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>james</td>
      <td>smith</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>mary</td>
      <td>jones</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td colspan="2">Larry</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
```

# 活动表格

我们可以用`table-active`类突出显示一个表格行或单元格:

```
<table class="table">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td class="table-active">james</td>
      <td>smith</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>mary</td>
      <td>jones</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td colspan="2">Larry</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
```

我们在单元格上添加了`table-active`类，因此它将被高亮显示。

这也适用于其他变体:

```
<table class="table table-success">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td class="table-active">james</td>
      <td>smith</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>mary</td>
      <td>jones</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td colspan="2">Larry</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
```

样式的工作原理是用`--bs-table-bg`属性设置背景。

然后表格上的渐变添加了 `background-image: linear-gradient(var( — bs-table-accent-bg), var( — bs-table-accent-bg));` CSS 属性。

添加`.table-striped`、`.table-hover`或`.table-active`类时，将`--bs-table-accent-bg`设置为半透明颜色，以改变背景颜色。

`--bs-table-accent-bg`为高光生成对比度最高的颜色。

文本和边框颜色以相同的方式生成。

# 表格边框

我们可以用`table-bordered`类添加边框:

```
<table class="table table-bordered">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>james</td>
      <td>smith</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>mary</td>
      <td>jones</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td colspan="2">Larry</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
```

![](img/55c9a7102fec3d5fe65bf90f6107abd3.png)

照片由 [Mgg Vitchakorn](https://unsplash.com/@mggbox?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以添加具有各种效果的表格，如条纹、悬停和各种颜色。