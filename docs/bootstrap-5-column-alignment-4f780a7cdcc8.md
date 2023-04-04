# 引导 5 —列对齐

> 原文：<https://blog.devgenius.io/bootstrap-5-column-alignment-4f780a7cdcc8?source=collection_archive---------3----------------------->

![](img/83a7b07323c6b4eac06337517c4bda89.png)

照片由 [gotdaflow](https://unsplash.com/@gettagottaflow?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 对齐列。

# 嵌套

我们可以用默认网格嵌套我们的内容。

例如，我们可以写:

```
<div class="container">
  <div class="row">
    <div class="col-sm-3">
      Level 1: .col-sm-3
    </div>
    <div class="col-sm-9">
      <div class="row">
        <div class="col-7 col-sm-5">
          Level 2: .col-7 .col-sm-5
        </div>
        <div class="col-6 col-sm-6">
          Level 2: .col-6 .col-sm-6
        </div>
      </div>
    </div>
  </div>
</div>
```

# 列

我们可以使用对齐、排序和偏移选项来更改列。

它基于 flexbox，简化了规模调整。

Bootstrap 具有预定义的类，用于创建快速响应的布局。

例如，我们可以写:

```
<div class="container">
  <div class="row align-items-start">
    <div class="col">
      1 of three columns
    </div>
    <div class="col">
      2 of three columns
    </div>
    <div class="col">
      3 of three columns
    </div>
  </div>
  <div class="row align-items-center">
    <div class="col">
      1 of three columns
    </div>
    <div class="col">
      2 of three columns
    </div>
    <div class="col">
      3of three columns
    </div>
  </div>
  <div class="row align-items-end">
    <div class="col">
      1 of three columns
    </div>
    <div class="col">
      2 of three columns
    </div>
    <div class="col">
      3 of three columns
    </div>
  </div>
</div>
```

我们有`col`类来调整列的大小。

对齐列后，我们有了`align-items`类。

`align-items-start`对齐顶端

`align-items-center`垂直居中各列。

`align-items-end`把柱子放在最下面。

我们可以让一行包含多列，它们有自己的垂直对齐方式。

为此，我们写道:

```
<div class="container">
  <div class="row">
    <div class="col align-self-start">
      One of three columns
    </div>
    <div class="col align-self-center">
      One of three columns
    </div>
    <div class="col align-self-end">
      One of three columns
    </div>
  </div>
</div>
```

我们可以将`align`类放在每一列中来垂直定位它们。

# 水平线向

要水平对齐列，我们可以使用`justify-content`类。

例如，我们可以写:

```
<div class="container">
  <div class="row justify-content-start">
    <div class="col-4">
      One of two columns
    </div>
    <div class="col-4">
      One of two columns
    </div>
  </div>
  <div class="row justify-content-center">
    <div class="col-4">
      One of two columns
    </div>
    <div class="col-4">
      One of two columns
    </div>
  </div>
  <div class="row justify-content-end">
    <div class="col-4">
      One of two columns
    </div>
    <div class="col-4">
      One of two columns
    </div>
  </div>
  <div class="row justify-content-around">
    <div class="col-4">
      One of two columns
    </div>
    <div class="col-4">
      One of two columns
    </div>
  </div>
  <div class="row justify-content-between">
    <div class="col-4">
      One of two columns
    </div>
    <div class="col-4">
      One of two columns
    </div>
  </div>
  <div class="row justify-content-evenly">
    <div class="col-4">
      One of two columns
    </div>
    <div class="col-4">
      One of two columns
    </div>
  </div>
</div>
```

`justify-content-start`左对齐各列。

`justify-content-center`居中对齐各列。

`justify-content-end`右对齐各列。

`justify-content-around`将列与每列之前、之间和之后的空间对齐。

`justify-content-between`两端对齐列。

`justiffy-columns-evenly`以相等的间距对齐各列。

# 列换行

如果列超出了行的宽度，它们将自动换到下一行。

例如，我们可以写:

```
<div class="container">
  <div class="row">
    <div class="col-9">.col-9</div>
    <div class="col-4">.col-4</div>
    <div class="col-6">.col-6</div>
  </div>
</div>
```

我们将看到最后两列位于第一列之后，因为它们溢出了 12 列网格。

# 分栏符

我们可以用一个分栏符强制列显示在下一行。

例如，我们可以写:

```
<div class="container">
  <div class="row">
    <div class="col-6 col-sm-2">.col-6 .col-sm-2</div>
    <div class="col-6 col-sm-4">.col-6 .col-sm-4</div>

    <div class="w-100"></div>

    <div class="col-6 col-sm-4">.col-6 .col-sm-4</div>
    <div class="col-6 col-sm-2">.col-6 .col-sm-2</div>
  </div>
</div>
```

使用带有`w-100`类的 div 强制将最后 2 列放在下一行。

![](img/16b15ae5b0e11ff8fdeb64966dc3efd8.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Moujib Aghrout](https://unsplash.com/@mojaghrout?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以添加分栏符来强制将列移到下一行。

我们还可以嵌套列并垂直或水平对齐它们。