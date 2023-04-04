# 材质用户界面——更多工具提示技巧和排版

> 原文：<https://blog.devgenius.io/material-ui-more-tooltip-tricks-and-typography-64c61ad8aff2?source=collection_archive---------2----------------------->

![](img/afef25d2ecc15915780f35c8521cc187.png)

[sheri silver](https://unsplash.com/@sheri_silver?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何自定义工具提示和添加排版与材料用户界面。

# 禁用元素

如果我们想在一个被禁用的元素上显示工具提示，那么我们必须用包装器元素包装这个被禁用的元素。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";export default function App() {
  return (
    <Tooltip title="disabled">
      <span>
        <Button disabled>Disabled Button</Button>
      </span>
    </Tooltip>
  );
}
```

我们用一个 span 将禁用的按钮包裹起来，这样当我们悬停在它上面时就可以看到工具提示。

# 过渡

当我们显示工具提示时，我们可以显示各种过渡。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";
import Fade from "[@material](http://twitter.com/material)-ui/core/Fade";export default function App() {
  return (
    <Tooltip
      TransitionComponent={Fade}
      TransitionProps={{ timeout: 600 }}
      title="tooltip"
    >
      <Button>Fade</Button>
    </Tooltip>
  );
}
```

在显示或隐藏工具提示时添加淡入淡出过渡。

其他效果包括缩放效果:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";
import Zoom from "[@material](http://twitter.com/material)-ui/core/Zoom";export default function App() {
  return (
    <Tooltip
      TransitionComponent={Zoom}
      TransitionProps={{ timeout: 600 }}
      title="tooltip"
    >
      <Button>Fade</Button>
    </Tooltip>
  );
}
```

我们设置`TransitionProps`来改变效果的持续时间。

# 显示和隐藏

我们还可以添加显示和隐藏工具提示的延迟。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";export default function App() {
  return (
    <Tooltip title="tooltip" enterDelay={500} leaveDelay={200}>
      <Button>button</Button>
    </Tooltip>
  );
}
```

当我们显示和隐藏工具提示时，我们会添加一个延迟。

数字以毫秒为单位。

# 排印

我们可以用`Typography`组件改变应用程序的字体。

Roboto 字体不是由材质 UI 自动生成的。

要加载它，我们可以添加以下 CSS:

```
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap" />
```

我们也可以安装`fontsource-roboto`包:

```
npm install fontsource-roboto
```

并写下:

```
import 'fontsource-roboto';
```

# 排版组件

我们可以添加`Typography`组件来呈现各种文本样式。

例如，我们可以写:

```
import React from "react";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";export default function App() {
  return (
    <>
      <Typography variant="h1" component="h2" gutterBottom>
        h1.
      </Typography>
      <Typography variant="h2" gutterBottom>
        h2.
      </Typography>
      <Typography variant="h3" gutterBottom>
        h3.
      </Typography>
      <Typography variant="h4" gutterBottom>
        h4.
      </Typography>
      <Typography variant="h5" gutterBottom>
        h5.
      </Typography>
      <Typography variant="h6" gutterBottom>
        h6.
      </Typography>
    </>
  );
}
```

用它来呈现各种标题。

我们渲染的元素设置在`variant`道具中。

`gutterBottom`在底部增加一些边距。

我们还可以呈现各种正文文本样式:

```
import React from "react";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";export default function App() {
  return (
    <>
      <Typography variant="subtitle1" gutterBottom>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quos
        blanditiis tenetur
      </Typography>
      <Typography variant="subtitle2" gutterBottom>
        Morbi maximus maximus nisl, in tristique augue convallis vel. Praesent
        eget scelerisque ex, a sagittis libero.
      </Typography>
      <Typography variant="body1" gutterBottom>
        Vivamus ultrices nunc a est pulvinar sodales.
      </Typography>
      <Typography variant="body2" gutterBottom>
        Proin tincidunt nunc sed orci suscipit varius. Suspendisse volutpat
        interdum iaculis.
      </Typography>
    </>
  );
}
```

我们将`variant`设置为`subtitle1`、`subtitle2`、`body1`或`body2`来显示各种正文样式。

按钮、标题和上划线文本也有样式:

```
import React from "react";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";export default function App() {
  return (
    <>
      <Typography variant="button" display="block" gutterBottom>
        button text
      </Typography>
      <Typography variant="caption" display="block" gutterBottom>
        caption text
      </Typography>
      <Typography variant="overline" display="block" gutterBottom>
        overline text
      </Typography>
    </>
  );
}
```

# 主题

我们可以使用主题的`typography`键来设计我们的文本。

例如，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  root: {
    ...theme.typography.button,
    backgroundColor: theme.palette.background.paper,
    padding: theme.spacing(1)
  }
}));export default function App() {
  const classes = useStyles(); return <div className={classes.root}>some div</div>;
}
```

我们将`theme.typography.button`样式添加到我们的样式中，这样我们可以在任何地方应用它。

![](img/0fe405f42579a24b6022c1797aa9811f.png)

布鲁克·拉克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以在任何地方使用工具提示。

可以添加排版组件来显示文本。