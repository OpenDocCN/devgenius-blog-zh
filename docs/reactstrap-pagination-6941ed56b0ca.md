# Reactstrap —分页

> 原文：<https://blog.devgenius.io/reactstrap-pagination-6941ed56b0ca?source=collection_archive---------4----------------------->

![](img/db0826a37c2b298466e23f4686aa7d05.png)

照片由[真诚媒体](https://unsplash.com/@sincerelymedia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Reactstrap 是为 React 制作的一个版本引导程序。

这是一组具有 Boostrap 陷阱样式的 React 组件。

在本文中，我们将研究如何使用 Reactstrap 添加分页栏。

# 页码

我们可以用`Pagination`、`PaginationLink`和`PaginationItem`组件轻松地添加一个分页栏。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Pagination, PaginationItem, PaginationLink } from "reactstrap";export default function App() {
  return (
    <div>
      <Pagination>
        <PaginationItem>
          <PaginationLink first href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink previous href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">1</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">2</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">3</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">4</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">5</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink next href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink last href="#" />
        </PaginationItem>
      </Pagination>
    </div>
  );
}
```

添加`Pagination`组件。

它是项目和链接的包装器。

`PaginationItem`有分页项。

而`PaginationLink`有链接。

`first`道具显示双左箭头。

`previous`道具显示左箭头。

`next`道具显示右箭头。

而`last`道具显示双右箭头。

`href`有链接的 URL。

# 禁用和活动状态

我们可以添加`disabled`属性来禁用分页项。

我们可以添加`active`道具来突出显示分页项。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Pagination, PaginationItem, PaginationLink } from "reactstrap";export default function App() {
  return (
    <div>
      <Pagination>
        <PaginationItem>
          <PaginationLink first href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink previous href="#" />
        </PaginationItem>
        <PaginationItem disabled>
          <PaginationLink href="#">1</PaginationLink>
        </PaginationItem>
        <PaginationItem active>
          <PaginationLink href="#">2</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">3</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">4</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">5</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink next href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink last href="#" />
        </PaginationItem>
      </Pagination>
    </div>
  );
}
```

我们给`PaginationItem`添加了`disabled`和`active`道具来增加效果。

一旦它被禁用，我们就不能点击它。

# 胶料

分页栏的大小可以更改。

我们只需用`size`道具改变它:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Pagination, PaginationItem, PaginationLink } from "reactstrap";export default function App() {
  return (
    <div>
      <Pagination size="lg">
        <PaginationItem>
          <PaginationLink first href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink previous href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">1</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">2</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">3</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">4</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">5</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink next href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink last href="#" />
        </PaginationItem>
      </Pagination>
    </div>
  );
}
```

我们将`size`设置为`lg`以使项目变大。

将尺寸设置为`sm`会使其变小:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Pagination, PaginationItem, PaginationLink } from "reactstrap";export default function App() {
  return (
    <div>
      <Pagination size="sm">
        <PaginationItem>
          <PaginationLink first href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink previous href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">1</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">2</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">3</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">4</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink href="#">5</PaginationLink>
        </PaginationItem>
        <PaginationItem>
          <PaginationLink next href="#" />
        </PaginationItem>
        <PaginationItem>
          <PaginationLink last href="#" />
        </PaginationItem>
      </Pagination>
    </div>
  );
}
```

![](img/c353ef7c7177ee26888b8021891640f0.png)

照片由[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用`Pagination`、`PaginationItem`和`PaginationLink`组件添加一个分页栏。