# 反应引导—列出组

> 原文：<https://blog.devgenius.io/react-bootstrap-list-groups-58ca1eeeb1fb?source=collection_archive---------12----------------------->

![](img/cf1e0176a0d2063db1f0c0ee53867b3e.png)

由 [Max Kleinen](https://unsplash.com/@hirmin?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 向 React 应用程序添加列表组。

# 列表组

列表组用于显示一系列内容。

我们可以用它们来展示很多东西。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  return (
    <div>
      <ListGroup>
        <ListGroup.Item>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi
          efficitur massa tellus, non ultrices augue sagittis ornare. Sed
          ultrices ligula in est tempus tincidunt. Nam id consequat lacus, et
          mattis turpis.
        </ListGroup.Item>
        <ListGroup.Item>
          In euismod egestas dictum. Curabitur vel ipsum in eros commodo egestas
          sed sed augue. In hac habitasse platea dictumst. In pretium massa in
          urna tincidunt vulputate. Duis eget tristique nisi, auctor pharetra
          ex. Duis rhoncus, lectus vitae consequat blandit, lectus sem rhoncus
          lorem, sit amet viverra tortor est in mi.
        </ListGroup.Item>
        <ListGroup.Item>
          Ut pretium sollicitudin vehicula. In hac habitasse platea dictumst.
          Morbi fringilla mauris sit amet quam tempor, vulputate consequat dolor
          tempor. Duis magna quam, molestie et velit a, cursus porttitor mi.
          Fusce consectetur faucibus elit ut dictum. Proin quis elit vel diam
          consectetur accumsan eu in neque. Suspendisse eget magna non tortor
          tincidunt porttitor in sit amet ligula. Aenean turpis arcu, suscipit
          eleifend neque ac.
        </ListGroup.Item>
      </ListGroup>
    </div>
  );
}
```

添加包含文本列表的列表组。

# 活动项目

我们可以设置`active`道具来高亮显示选中的项目。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  return (
    <div>
      <ListGroup>
        <ListGroup.Item active>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi
          efficitur massa tellus, non ultrices augue sagittis ornare. Sed
          ultrices ligula in est tempus tincidunt. Nam id consequat lacus, et
          mattis turpis.
        </ListGroup.Item>
        <ListGroup.Item>
          In euismod egestas dictum. Curabitur vel ipsum in eros commodo egestas
          sed sed augue. In hac habitasse platea dictumst. In pretium massa in
          urna tincidunt vulputate. Duis eget tristique nisi, auctor pharetra
          ex. Duis rhoncus, lectus vitae consequat blandit, lectus sem rhoncus
          lorem, sit amet viverra tortor est in mi.
        </ListGroup.Item>
        <ListGroup.Item>
          Ut pretium sollicitudin vehicula. In hac habitasse platea dictumst.
          Morbi fringilla mauris sit amet quam tempor, vulputate consequat dolor
          tempor.
          eleifend neque ac.
        </ListGroup.Item>
      </ListGroup>
    </div>
  );
}
```

因为我们在第一个`ListGroup.Item`上有了`active`道具，所以第一个项目被高亮显示。

# 禁用的项目

我们可以添加`disabled`道具来阻止对`ListGroup.Item`的行动

`onClick`将被禁用。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  return (
    <div>
      <ListGroup>
        <ListGroup.Item disabled>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi
          efficitur massa tellus, non ultrices augue sagittis ornare. Sed
          ultrices ligula in est tempus tincidunt. Nam id consequat lacus, et
          mattis turpis.
        </ListGroup.Item>
        <ListGroup.Item>
          In euismod egestas dictum. Curabitur vel ipsum in eros commodo egestas
          sed sed augue. In hac habitasse platea dictumst. In pretium massa in
          urna tincidunt vulputate. Duis eget tristique nisi, auctor pharetra
          ex. Duis rhoncus, lectus vitae consequat blandit, lectus sem rhoncus
          lorem, sit amet viverra tortor est in mi.
        </ListGroup.Item>
        <ListGroup.Item>
          Ut pretium sollicitudin vehicula. In hac habitasse platea dictumst.
          Morbi fringilla mauris sit amet quam tempor, vulputate consequat dolor
          tempor. Duis magna quam, molestie et velit a, cursus porttitor mi.
          Fusce consectetur faucibus elit ut dictum.
        </ListGroup.Item>
      </ListGroup>
    </div>
  );
}
```

禁用列表组项目。

# 可操作项目

道具让我们将列表组项目呈现为链接或按钮。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  const onClick = () => {
    alert("clicked");
  };return (
    <div>
      <ListGroup>
        <ListGroup.Item action href="#foo">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi
          efficitur massa tellus, non ultrices augue sagittis ornare. Sed
          ultrices ligula in est tempus tincidunt. Nam id consequat lacus, et
          mattis turpis.
        </ListGroup.Item>
        <ListGroup.Item action href="#bar">
          In euismod egestas dictum. Curabitur vel ipsum in eros commodo egestas
          sed sed augue. In hac habitasse platea dictumst. In pretium massa in
          urna tincidunt vulputate. Duis eget tristique nisi, auctor pharetra
          ex. Duis rhoncus, lectus vitae consequat blandit, lectus sem rhoncus
          lorem, sit amet viverra tortor est in mi.
        </ListGroup.Item>
        <ListGroup.Item action onClick={onClick}>
          Ut pretium sollicitudin vehicula. In hac habitasse platea dictumst.
          Morbi fringilla mauris sit amet quam tempor, vulputate consequat dolor
          tempor. Duis magna quam, molestie et velit a, cursus porttitor mi.
          Fusce consectetur faucibus elit ut dictum.
        </ListGroup.Item>
      </ListGroup>
    </div>
  );
}
```

我们有带`href`或`onClick`道具的`action`道具，当我们点击链接时，可以导航到另一个页面。

或者，如果我们有`onClick`，我们可以运行一个点击处理程序。

![](img/1addd511cca89daa4384d45dc11e6d47.png)

[主持人李](https://unsplash.com/@anchorlee?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们可以添加一个列表组，其中包含可以做各种事情的项目。

它们可以被禁用或呈现为链接或按钮。