# 反应陷阱—进度条

> 原文：<https://blog.devgenius.io/reactstrap-progress-bars-bc005449aa58?source=collection_archive---------6----------------------->

![](img/9f2c122399fea5fdb2229fe72b3a5fca.png)

帕特里克·托马索在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Reactstrap 是为 React 制作的一个版本引导程序。

这是一组具有 Boostrap 陷阱样式的 React 组件。

在本文中，我们将看看如何用 Reactstrap 添加一个进度条。

# 进步

我们可以用`Progress`组件添加一个进度条。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Progress } from "reactstrap";export default function App() {
  return (
    <div className="text-center">
      <Progress value={2 * 20} />
      <Progress color="success" value="25" />
      <Progress color="info" value={50} />
      <Progress color="warning" value={75} />
      <Progress color="danger" value="100" />
    </div>
  );
}
```

显示各种进度条。

`value`道具让我们改变填充的条的数量。

`color`有颜色，它们是显示的变体。

# 多段

我们可以创建一个有多个部分的进度条。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Progress } from "reactstrap";export default function App() {
  return (
    <div className="text-center">
      <Progress multi>
        <Progress bar value="15">
          foo
        </Progress>
        <Progress bar color="success" value="30">
          bar
        </Progress>
        <Progress bar color="info" value="25">
          baz
        </Progress>
        <Progress bar color="warning" value="20">
          qux
        </Progress>
        <Progress bar color="danger" value="5">
          !!
        </Progress>
      </Progress>
    </div>
  );
}
```

我们有一个`Progress`组件来包装其他的`Progress`组件。

`multi`道具让我们添加一个有多个分段的进度条。

内部条有`bar`组件，用`color`来指定它们的颜色。

`value`是 100 分中的值。

# 有斑纹的

我们可以用`striped`道具做一个条纹进度条。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Progress } from "reactstrap";export default function App() {
  return (
    <div className="text-center">
      <Progress striped color="info" value={50} />
    </div>
  );
}
```

添加条纹进度条。

这也适用于具有多个分段的进度条:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Progress } from "reactstrap";export default function App() {
  return (
    <div className="text-center">
      <Progress multi>
        <Progress striped bar value="10" />
        <Progress striped bar color="success" value="20" />
        <Progress striped bar color="warning" value="30" />
        <Progress striped bar color="danger" value="20" />
      </Progress>
    </div>
  );
}
```

我们必须将`striped`道具添加到每个横条上。

# 愉快的

我们可以通过使用`animated`道具来制作进度条动画。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Progress } from "reactstrap";export default function App() {
  return (
    <div className="text-center">
      <Progress animated color="info" value={50} />
    </div>
  );
}
```

让它变得生动。

我们可以对多段进度条做同样的事情:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Progress } from "reactstrap";export default function App() {
  return (
    <div className="text-center">
      <Progress multi>
        <Progress animated bar value="10" />
        <Progress animated bar color="success" value="20" />
        <Progress animated bar color="warning" value="30" />
        <Progress animated bar color="danger" value="20" />
      </Progress>
    </div>
  );
}
```

`animated`道具应用于每一段。

![](img/4b9d37817eb3c9a3645bf5347030204b.png)

[安舒 A](https://unsplash.com/@anshu18?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们可以添加一个进度条，有多段，不同的颜色。

它们也可以是条纹或动画。