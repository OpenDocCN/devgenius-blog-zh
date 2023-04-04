# 材料用户界面—徽章和筹码

> 原文：<https://blog.devgenius.io/material-ui-badges-and-chips-8eebfa4ec7ff?source=collection_archive---------3----------------------->

![](img/a0aef63be5fd8add7d570e4c6b46e5e4.png)

照片由 [Nagaraju amrabad](https://unsplash.com/@ursanr?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何自定义徽章和添加材料 UI 芯片。

# 徽章的最大价值

我们可以设置徽章的最大值。

它有能力让我们这样做。

例如，我们可以写:

```
import React from "react";
import Badge from "[@material](http://twitter.com/material)-ui/core/Badge";
import MailIcon from "[@material](http://twitter.com/material)-ui/icons/Mail";export default function App() {
  return (
    <>
      <Badge badgeContent={1000} max={999} color="secondary">
        <MailIcon />
      </Badge>
    </>
  );
}
```

我们添加了带有徽章的`Mailicon`和`max`道具。

设置为 999。所以当`badgeContent`比那个大的时候，就会显示 999+。

# 徽章重叠

我们可以改变徽章与图标的重叠方式。

例如，我们可以写:

```
import React from "react";
import clsx from "clsx";
import MailIcon from "[@material](http://twitter.com/material)-ui/icons/Mail";
import Badge from "[@material](http://twitter.com/material)-ui/core/Badge";export default function App() {
  return (
    <div>
      <Badge color="secondary" overlap="circle" badgeContent=" " variant="dot">
        <MailIcon />
      </Badge>
    </div>
  );
}
```

我们添加了`overlap` ptop，并将其设置为`circle`，使其适用于圆形。

# 圆点徽章

要使徽章显示为圆点，我们可以使用设置为`dot`的`variant`。

例如，我们可以写:

```
import React from "react";
import clsx from "clsx";
import MailIcon from "[@material](http://twitter.com/material)-ui/icons/Mail";
import Badge from "[@material](http://twitter.com/material)-ui/core/Badge";export default function App() {
  return (
    <div>
      <Badge color="secondary" variant="dot">
        <MailIcon />
      </Badge>
    </div>
  );
}
```

当`variant`设置为`dot`时，我们可以看到邮件图标上方的圆点。

# 徽章对齐

要更改徽章的对齐方式，我们可以使用`anchorOrigin`支柱来设置徽章的位置。

例如，我们可以写:

```
import React from "react";
import clsx from "clsx";
import MailIcon from "[@material](http://twitter.com/material)-ui/icons/Mail";
import Badge from "[@material](http://twitter.com/material)-ui/core/Badge";export default function App() {
  return (
    <div>
      <Badge
        color="secondary"
        variant="dot"
        anchorOrigin={{
          vertical: "top",
          horizontal: "left"
        }}
      >
        <MailIcon />
      </Badge>
    </div>
  );
}
```

将徽章放在图标的左上方。

# 芯片

芯片是代表输入、属性或动作的小元素。

我们可以通过使用`Chip`组件来添加它们:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";export default function App() {
  return (
    <div>
      <Chip label="chip" />
    </div>
  );
}
```

我们添加一个带有`Chip`组件的芯片。

`label`文本显示为内容。

# 轮廓薯片

我们可以添加一个轮廓为`variant`设置为`outlined`的芯片。

例如，我们可以写:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";export default function App() {
  return (
    <div>
      <Chip label="chip" variant="outlined" />
    </div>
  );
}
```

现在我们看到一个轮廓，而不是灰色背景。

# 芯片阵列

我们可以把一组芯片排列成一个数组。

例如，我们可以写:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";export default function App() {
  const [chipData] = React.useState([
    { key: 0, label: "james" },
    { key: 1, label: "mary" },
    { key: 2, label: "alex" },
    { key: 3, label: "john" },
    { key: 4, label: "david" }
  ]); return (
    <div>
      {chipData.map(c => (
        <Chip label={c.label} key={c.key} />
      ))}
    </div>
  );
}
```

把它们放在一个数组里。

我们可以让用户用`onDelete`道具删除芯片:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";export default function App() {
  const [chipData, setChipData] = React.useState([
    { key: 0, label: "james" },
    { key: 1, label: "mary" },
    { key: 2, label: "alex" },
    { key: 3, label: "john" },
    { key: 4, label: "david" }
  ]); const handleDelete = chipToDelete => () => {
    setChipData(chips => chips.filter(chip => chip.key !== chipToDelete.key));
  }; return (
    <div>
      {chipData.map(c => (
        <Chip label={c.label} key={c.key} onDelete={handleDelete(c)} />
      ))}
    </div>
  );
}
```

我们添加了`onDelete` prop，它接受用我们想要删除的`chips`条目调用的`handleDelete`函数。

该函数返回一个将芯片数据设置为新状态的函数，但不包含我们想要删除的条目。

将显示“x”图标，让我们移除芯片。

![](img/2d2b65971fe3848ac212f073be6b3019.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Element5 数码](https://unsplash.com/@element5digital?utm_source=medium&utm_medium=referral)拍摄

# 结论

我们可以添加芯片向用户显示一小块数据。

它们可以被删除。