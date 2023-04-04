# 材质用户界面—响应性和可拖动的对话框

> 原文：<https://blog.devgenius.io/material-ui-responsive-and-draggable-dialogs-96d86efd8152?source=collection_archive---------2----------------------->

![](img/166b34b929b454830fb91cb03d116536.png)

赫尔墨斯·里维拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何用材质 UI 定制对话框。

# 响应全屏对话框

我们可以用`useMediaQuery`钩子创建一个全屏的对话框。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import DialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";
import DialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import DialogContentText from "[@material](http://twitter.com/material)-ui/core/DialogContentText";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import useMediaQuery from "[@material](http://twitter.com/material)-ui/core/useMediaQuery";
import { useTheme } from "[@material](http://twitter.com/material)-ui/core/styles";export default function App() {
  const [open, setOpen] = React.useState(false);
  const theme = useTheme();
  const fullScreen = useMediaQuery(theme.breakpoints.down("sm")); const handleClickOpen = () => {
    setOpen(true);
  }; const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <Button variant="outlined" color="primary" onClick={handleClickOpen}>
        Open dialog
      </Button>
      <Dialog fullScreen={fullScreen} open={open} onClose={handleClose}>
        <DialogTitle>Dialog</DialogTitle>
        <DialogContent>
          <DialogContentText>lorem ipsum</DialogContentText>
        </DialogContent>
        <DialogActions>
          <Button onClick={handleClose} color="primary" autoFocus>
            ok
          </Button>
        </DialogActions>
      </Dialog>
    </div>
  );
}
```

添加一个全屏的响应对话框。

我们使用了`useTheme`钩子来获取主题。

有了它，我们使用`useMediaQuery`让`breakpoint`传入钩子。

这意味着如果屏幕宽度达到 sm 断点或更小，对话框将全屏显示。

否则，它将是一个普通大小的对话框。

# 确认对话框

我们可以创建一个确认对话框，以便在关闭对话框之前进行选择。

要创建一个，我们可以写:

```
import React from "react";
import PropTypes from "prop-types";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import List from "[@material](http://twitter.com/material)-ui/core/List";
import ListItem from "[@material](http://twitter.com/material)-ui/core/ListItem";
import ListItemText from "[@material](http://twitter.com/material)-ui/core/ListItemText";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import DialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import DialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import RadioGroup from "[@material](http://twitter.com/material)-ui/core/RadioGroup";
import Radio from "[@material](http://twitter.com/material)-ui/core/Radio";
import FormControlLabel from "[@material](http://twitter.com/material)-ui/core/FormControlLabel";const options = ["apple", "orange", "grape"];function ConfirmationDialogRaw(props) {
  const { onClose, value: valueProp, open, ...other } = props;
  const [value, setValue] = React.useState(valueProp);
  const radioGroupRef = React.useRef(null); React.useEffect(() => {
    if (!open) {
      setValue(valueProp);
    }
  }, [valueProp, open]); const handleEntering = () => {
    if (radioGroupRef.current != null) {
      radioGroupRef.current.focus();
    }
  }; const handleCancel = () => {
    onClose();
  }; const handleOk = () => {
    onClose(value);
  }; const handleChange = event => {
    setValue(event.target.value);
  }; return (
    <Dialog
      disableBackdropClick
      disableEscapeKeyDown
      maxWidth="xs"
      onEntering={handleEntering}
      open={open}
      {...other}
    >
      <DialogTitle>Fruit</DialogTitle>
      <DialogContent dividers>
        <RadioGroup
          ref={radioGroupRef}
          name="ringtone"
          value={value}
          onChange={handleChange}
        >
          {options.map(option => (
            <FormControlLabel
              value={option}
              key={option}
              control={<Radio />}
              label={option}
            />
          ))}
        </RadioGroup>
      </DialogContent>
      <DialogActions>
        <Button autoFocus onClick={handleCancel} color="primary">
          Cancel
        </Button>
        <Button onClick={handleOk} color="primary">
          Ok
        </Button>
      </DialogActions>
    </Dialog>
  );
}export default function App() {
  const [open, setOpen] = React.useState(false);
  const [value, setValue] = React.useState("Done"); const onClick = () => {
    setOpen(true);
  }; const handleClose = newValue => {
    setOpen(false); if (newValue) {
      setValue(newValue);
    }
  }; return (
    <div>
      <Button secondary={value} onClick={onClick}>
        Choose fruit
      </Button> <ConfirmationDialogRaw
        keepMounted
        open={open}
        onClose={handleClose}
        value={value}
      />
    </div>
  );
}
```

我们添加了`ConfirmationDialogRaw`组件来显示带有各种选项的对话框。

它有一个从`option`数组呈现的选项列表，作为一组单选按钮。

此外，我们在单选按钮下面有一个“确定”和“取消”按钮。

在`App`组件中，我们有`Button`让我们打开对话框。

# 可拖动对话框

我们可以用 react-draggable 库创建一个可拖动的对话框。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import DialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";
import DialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import DialogContentText from "[@material](http://twitter.com/material)-ui/core/DialogContentText";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";
import Draggable from "react-draggable";function PaperComponent(props) {
  return (
    <Draggable
      handle="#draggable-dialog-title"
      cancel={'[class*="MuiDialogContent-root"]'}
    >
      <Paper {...props} />
    </Draggable>
  );
}export default function App() {
  const [open, setOpen] = React.useState(false); const handleClickOpen = () => {
    setOpen(true);
  }; const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <Button variant="outlined" color="primary" onClick={handleClickOpen}>
        Open dialog
      </Button>
      <Dialog open={open} onClose={handleClose} PaperComponent={PaperComponent}>
        <DialogTitle style={{ cursor: "move" }} id="draggable-dialog-title">
          Title
        </DialogTitle>
        <DialogContent>
          <DialogContentText>lorem ipsum</DialogContentText>
        </DialogContent>
        <DialogActions>
          <Button onClick={handleClose} color="primary">
            ok
          </Button>
        </DialogActions>
      </Dialog>
    </div>
  );
}
```

创建一个可拖动的对话框。

我们创造了`PaperComponent`使其可拖动。

react-draggable 中的`Draggable`组件让我们通过设置带有选择器的`handle`属性来指定可拖动的项目。

我们将它传递给`Dialog`的`PaperComponent`道具。

选择器必须在`DialogTitle`中，以使对话框可拖动。

![](img/f1e2ea58a41dfafa099b824aaeb025c4.png)

安娜·佩尔泽在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以让全屏对话框有反应。

它们也可以制成可拖动的。