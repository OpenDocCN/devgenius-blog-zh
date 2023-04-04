# 材料用户界面—滑块

> 原文：<https://blog.devgenius.io/material-ui-slider-d08100891180?source=collection_archive---------1----------------------->

![](img/2c4b1bae8e0b8a279933b1b9a4e81647.png)

照片由[布鲁斯·沃林顿](https://unsplash.com/@brucebmax?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何添加带有材质 UI 的滑块。

# 离散滑块

我们可以添加滑块来捕捉离散值。

为此，我们将`marks`属性设置为`true`。

例如，我们可以写:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";export default function App() {
  return (
    <form>
      <Slider marks />
    </form>
  );
}
```

来制作一个不受控制的滑块，它可以捕捉到最接近的 10 位。

# 小步前进

我们可以用`steps`道具改变舞步。

例如，我们可以写:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";export default function App() {
  return (
    <form>
      <Slider step={0.00001} marks />
    </form>
  );
}
```

改变每个离散值之间的增量。

# 自定义标记

我们可以用`marks`道具添加自己的自定义标记。

例如，我们可以写:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";const marks = [
  {
    value: 0,
    label: "0m"
  },
  {
    value: 25,
    label: "25m"
  },
  {
    value: 50,
    label: "50m"
  },
  {
    value: 75,
    label: "75m"
  },
  {
    value: 100,
    label: "100m"
  }
];export default function App() {
  return (
    <form>
      <Slider step={10} marks={marks} valueLabelDisplay="auto" />
    </form>
  );
}
```

我们在每个条目中添加带有`value`和`label`属性的`marks`数组，以显示带有`label`属性中文本的标签。

# 受限值

我们可以通过将`step`设置为`null`来将值限制为`marks`数组中的值。

例如，我们可以写:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";const marks = [
  {
    value: 0,
    label: "0m"
  },
  {
    value: 25,
    label: "25m"
  },
  {
    value: 50,
    label: "50m"
  },
  {
    value: 75,
    label: "75m"
  },
  {
    value: 100,
    label: "100m"
  }
];export default function App() {
  return (
    <form>
      <Slider step={null} marks={marks} valueLabelDisplay="auto" />
    </form>
  );
}
```

现在我们只能选择标记在`marks`数组中的值，因为`null`值被传递到了`step`属性中。

# 使标签始终可见

我们可以通过将`valueLabelDisplay`属性设置为`on`来使标签始终可见。

例如，我们可以写:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";const marks = [
  {
    value: 0,
    label: "0m"
  },
  {
    value: 25,
    label: "25m"
  },
  {
    value: 50,
    label: "50m"
  },
  {
    value: 75,
    label: "75m"
  },
  {
    value: 100,
    label: "100m"
  }
];export default function App() {
  return (
    <form>
      <Slider step={10} valueLabelDisplay="on" marks={marks} />
    </form>
  );
}
```

现在我们看到滑块点上方的值。

# 范围滑块

我们可以允许用户用`Slider`选择一个数值范围。

为此，我们可以将数组的值设置为初始值。

例如，我们可以写:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";export default function App() {
  const [value, setValue] = React.useState([20, 30]); const handleChange = (event, newValue) => {
    setValue(newValue);
  };
  return (
    <form>
      <Slider valueLabelDisplay="auto" value={value} onChange={handleChange} />
    </form>
  );
}
```

制作一个范围滑块。

我们设置一个数组作为`value`状态的初始值，使滑块成为一个范围滑块。

# 垂直滑块

我们可以在`orientation`道具设置为`vertical`的情况下制作一个垂直的滑块。

例如，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";const useStyles = makeStyles({
  root: {
    height: 300
  }
});export default function App() {
  const classes = useStyles(); return (
    <div className={classes.root}>
      <Slider
        orientation="vertical"
        valueLabelDisplay="auto"
        defaultValue={30}
      />
    </div>
  );
}
```

创建垂直滑块。

我们必须设置包装器的高度，以便显示滑块。

# 轨道

我们可以将`track`设置为`false`来移除当我们选择一个值时显示的轨迹。

为此，我们写道:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";export default function App() {
  return (
    <div>
      <Slider track={false} defaultValue={30} />
    </div>
  );
}
```

移除轨道。

我们也可以将其设置为`inverted`来改变轨道，使其与默认轨道相反。

# 非线性标度

默认情况下，比例是线性的，但是我们可以通过将`scale`属性设置为函数，将其更改为非线性比例。

我们可以写:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";export default function App() {
  return (
    <div>
      <Slider scale={x => x ** 5} defaultValue={30} />
    </div>
  );
}
```

来改变比例。

![](img/68e199d406ecf28ce1c92678a5d06261.png)

由[塞拉圣约翰](https://unsplash.com/@sierrastjohn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以添加滑块，让用户从滑块中选择一个数字或一系列数字。