# 材质用户界面-扩展面板自定义和进度微调器

> 原文：<https://blog.devgenius.io/material-ui-expansion-panels-customization-and-progress-spinner-be2606a6463?source=collection_archive---------8----------------------->

![](img/2caedb7b35a2f587684ac3f0af4ffd29.png)

[Minnie Zhou](https://unsplash.com/@marslady?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何定制扩展面板和添加带有材质 UI 的进度微调器。

# 附加操作

我们可以向扩展面板添加额外的操作。

例如，我们可以写:

```
import React from "react";
import ExpansionPanel from "[@material](http://twitter.com/material)-ui/core/ExpansionPanel";
import ExpansionPanelSummary from "[@material](http://twitter.com/material)-ui/core/ExpansionPanelSummary";
import ExpansionPanelDetails from "[@material](http://twitter.com/material)-ui/core/ExpansionPanelDetails";
import Checkbox from "[@material](http://twitter.com/material)-ui/core/Checkbox";
import FormControlLabel from "[@material](http://twitter.com/material)-ui/core/FormControlLabel";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import ExpandMoreIcon from "[@material](http://twitter.com/material)-ui/icons/ExpandMore";export default function App() {
  return (
    <div>
      <ExpansionPanel>
        <ExpansionPanelSummary
          expandIcon={<ExpandMoreIcon />}
          id="additional-actions1-header"
        >
          <FormControlLabel
            onClick={event => event.stopPropagation()}
            onFocus={event => event.stopPropagation()}
            control={<Checkbox />}
            label="Lorem ipsum dolor sit amet"
          />
        </ExpansionPanelSummary>
        <ExpansionPanelDetails>
          <Typography>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit
          </Typography>
        </ExpansionPanelDetails>
      </ExpansionPanel>
      <ExpansionPanel>
        <ExpansionPanelSummary
          expandIcon={<ExpandMoreIcon />}
          id="additional-actions2-header"
        >
          <FormControlLabel
            onClick={event => event.stopPropagation()}
            onFocus={event => event.stopPropagation()}
            control={<Checkbox />}
            label="Praesent id erat pretium"
          />
        </ExpansionPanelSummary>
        <ExpansionPanelDetails>
          <Typography>
            Praesent id erat pretium, mollis turpis sit amet, vestibulum ipsum.
            Phasellus non mi eu velit lacinia consequat
          </Typography>
        </ExpansionPanelDetails>
      </ExpansionPanel>
    </div>
  );
}
```

在标题中的文本旁边添加带有复选标记的展开面板。

我们嵌套了一个`ExpansionPanelSummary`组件来添加一个带有`FormControlLabel`和`Checkbox`组件的复选框。

我们必须为`FormControlLabel`中的`onClick`和`onFocus`道具添加事件处理程序，这样复选框的点击事件就不会传播到扩展面板摘要中。

# 次要标题和列

我们可以在标题和内容中添加任何我们想要的内容。

所以如果我们愿意，我们可以添加副标题。

为此，我们可以写:

```
import React from "react";
import ExpansionPanel from "[@material](http://twitter.com/material)-ui/core/ExpansionPanel";
import ExpansionPanelDetails from "[@material](http://twitter.com/material)-ui/core/ExpansionPanelDetails";
import ExpansionPanelSummary from "[@material](http://twitter.com/material)-ui/core/ExpansionPanelSummary";
import ExpansionPanelActions from "[@material](http://twitter.com/material)-ui/core/ExpansionPanelActions";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import ExpandMoreIcon from "[@material](http://twitter.com/material)-ui/icons/ExpandMore";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Divider from "[@material](http://twitter.com/material)-ui/core/Divider";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  column: {
    flexBasis: "33.33%"
  }
}));export default function App() {
  const classes = useStyles(); return (
    <div>
      <ExpansionPanel defaultExpanded>
        <ExpansionPanelSummary
          expandIcon={<ExpandMoreIcon />}
          id="panel1c-header"
        >
          <div className={classes.column}>
            <Typography>Fruits</Typography>
          </div>
          <div className={classes.column}>
            <Typography>choose a fruit</Typography>
          </div>
        </ExpansionPanelSummary>
        <ExpansionPanelDetails>
          <div>
            <Typography variant="caption">apple, orange</Typography>
          </div>
        </ExpansionPanelDetails>
        <Divider />
        <ExpansionPanelActions>
          <Button size="small">Cancel</Button>
          <Button size="small" color="primary">
            Save
          </Button>
        </ExpansionPanelActions>
      </ExpansionPanel>
    </div>
  );
}
```

我们添加一些分成几列的文本。

设置`className`是为了让我们可以并排看到东西。

此外，我们使用`ExpansionPanelActions`组件将一些按钮添加到它自己的容器中。

`Divider`显示为`ExpansionPanelDetails`和`ExpansionPanelActions`之间的一条线。

# 进展指标

为了显示正在加载的内容，我们可以在应用程序中添加一个进度指示器。

例如，我们可以使用`CircularProgress`组件来显示加载微调器。

我们可以写:

```
import React from "react";
import CircularProgress from "[@material](http://twitter.com/material)-ui/core/CircularProgress";export default function App() {
  return (
    <div>
      <CircularProgress />
    </div>
  );
}
```

添加圆形微调器。

我们可以用`color`改变颜色:

```
import React from "react";
import CircularProgress from "[@material](http://twitter.com/material)-ui/core/CircularProgress";export default function App() {
  return (
    <div>
      <CircularProgress color="secondary" />
    </div>
  );
}
```

# 圆形确定的

我们可以显示一个循环进度微调器，以便仅在加载内容时显示该微调器。

例如，我们可以写:

```
import React from "react";
import CircularProgress from "[@material](http://twitter.com/material)-ui/core/CircularProgress";export default function App() {
  const [progress, setProgress] = React.useState(0); React.useEffect(() => {
    const timer = setInterval(() => {
      setProgress(prevProgress =>
        prevProgress >= 100 ? 0 : prevProgress + 10
      );
    }, 800); return () => {
      clearInterval(timer);
    };
  }, []); return (
    <div>
      <CircularProgress variant="static" value={progress} />
    </div>
  );
}
```

我们添加了`CircularProgress`组件，并将`variant`设置为`static`，使其成为静态的。

这让我们可以在`progress`状态更新时更新微调器显示。

![](img/93cfc804aedee8a175c5427339f2c7a3.png)

科林·梅纳德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用我们想要的内容定制我们的扩展面板。

此外，我们可以添加一个进度微调器，显示为一个不受控制的组件。

或者我们可以根据事情的进展来制作动画。

一切的造型都可以定制。