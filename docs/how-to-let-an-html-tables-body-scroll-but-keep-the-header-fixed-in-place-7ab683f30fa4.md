# 如何让一个 HTML 表格的主体滚动，但保持标题固定在适当的位置？

> 原文：<https://blog.devgenius.io/how-to-let-an-html-tables-body-scroll-but-keep-the-header-fixed-in-place-7ab683f30fa4?source=collection_archive---------1----------------------->

![](img/1d8b667f003e3eb9cfb6f080fd63ca0d.png)

照片由[卢卡·布拉沃](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有时，我们想要添加一个 HTML 表格，它有一个可滚动的主体，但是标题是固定的。

在本文中，我们将看看如何添加一个表，其中表体滚动，但表头保持不动。

# 创建具有固定标题的表格

我们可以用一些 CSS 创建一个有固定标题的 HTML 表格。

例如，如果我们有下表:

```
<table>
  <thead>
    <tr>
      <th>One</th>
      <th>Two</th>      
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
    </tr>
  </tbody>
</table>
```

然后，我们可以通过编写以下 CSS 来设置它的样式:

```
table {
  overflow: scroll;
  display: block;
  height: 120px;
}thead>tr {
  position: absolute;
  display: block;
  padding: 0;
  margin: 0;
  top: 0;
  background-color: white;
}tbody>tr:nth-of-type(1) {
  margin-top: 16px;
}tbody tr {
  display: block;
}td,
th {
  width: 70px;
  border-style: solid;
  border-width: 1px;
  border-color: black;
}
```

我们将`table`元素的高度设置为 120px 来限制它的高度，这样我们就可以使它可滚动。

为了使其可滚动，我们将`overflow` CSS 属性设置为`scroll`。

然后我们将`thead`中的`tr`元素设置到绝对位置，这样它们就能保持在原位。

此外，我们将`thead`中的`tr`元素设置为白色背景，这样当我们滚动它们时，下面的元素将被隐藏。

# 结论

我们可以用一些 CSS 样式制作一个带有可滚动主体和固定标题的 HTML 表格。