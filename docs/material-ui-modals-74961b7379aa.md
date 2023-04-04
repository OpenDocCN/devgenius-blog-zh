# 材料用户界面—模型

> 原文：<https://blog.devgenius.io/material-ui-modals-74961b7379aa?source=collection_archive---------1----------------------->

![](img/d06925a61121874326a19b60f923cc37.png)

照片由 [Piotr Stefański](https://unsplash.com/@mnemotechnikon?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何添加带有材质 UI 的模态。

# 情态的

我们可以使用`Modal`组件以我们的方式显示对话框。

要创建一个简单的，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Modal from "[@material](http://twitter.com/material)-ui/core/Modal";function getModalStyle() {
  const top = 50;
  const left = 50; return {
    top: `${top}%`,
    left: `${left}%`,
    transform: `translate(-${top}%, -${left}%)`
  };
}const useStyles = makeStyles(theme => ({
  paper: {
    position: "absolute",
    width: 300,
    backgroundColor: theme.palette.background.paper,
    padding: 20
  }
}));export default function App() {
  const classes = useStyles();
  const [modalStyle] = React.useState(getModalStyle);
  const [open, setOpen] = React.useState(false); const handleOpen = () => {
    setOpen(true);
  }; const handleClose = () => {
    setOpen(false);
  }; const body = (
    <div style={modalStyle} className={classes.paper}>
      <h2>Modal</h2>
      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. </p>
      <SimpleModal />
    </div>
  ); return (
    <div>
      <button type="button" onClick={handleOpen}>
        Open Modal
      </button>
      <Modal open={open} onClose={handleClose}>
        {body}
      </Modal>
    </div>
  );
}
```

创建一个简单的模型。

我们添加了一些带有`makeStyles`组件和`getModalStyle`功能的样式。

我们使用`makeStyles`来设置背景颜色和内容的位置。

我们将位置更改为“绝对”,以便它显示在所有内容之上。

`getModalStyle`用于定位情态。

# 过渡

我们可以用一个过渡组件来激活模态。

例如，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Modal from "[@material](http://twitter.com/material)-ui/core/Modal";
import Backdrop from "[@material](http://twitter.com/material)-ui/core/Backdrop";
import Fade from "[@material](http://twitter.com/material)-ui/core/Fade";const useStyles = makeStyles(theme => ({
  modal: {
    display: "flex",
    alignItems: "center",
    justifyContent: "center"
  },
  paper: {
    backgroundColor: theme.palette.background.paper,
    border: "2px solid gray",
    boxShadow: theme.shadows[5],
    padding: theme.spacing(2, 4, 3)
  }
}));export default function App() {
  const classes = useStyles();
  const [open, setOpen] = React.useState(false); const handleOpen = () => {
    setOpen(true);
  }; const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <button type="button" onClick={handleOpen}>
        open modal
      </button>
      <Modal
        className={classes.modal}
        open={open}
        onClose={handleClose}
        closeAfterTransition
        BackdropComponent={Backdrop}
        BackdropProps={{
          timeout: 500
        }}
      >
        <Fade in={open}>
          <div className={classes.paper}>
            <h2>fade modal</h2>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
          </div>
        </Fade>
      </Modal>
    </div>
  );
}
```

创建一个打开或关闭时带有渐变过渡的模态。

这是通过`Fade`组件完成的。

`in`属性指定当`open`为`true`时我们显示过渡。

然后我们就有了我们想要在模态中显示的内容。

当我们点击打开模态按钮时，我们打开了模态。

当我们关闭它时，我们可以点击外部。

`handleOpen`将`open`状态设置为`true`打开模态。

`hanleClose`关闭模态关闭模态。

`onClose`是我们在模态外点击时运行的。

`BackdropComponent`设置背景的组件。

`closeAfterTransition`让我们等到嵌套转换完成后再关闭。

此外，我们可以使用 react-spring 包来添加过渡:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Modal from "[@material](http://twitter.com/material)-ui/core/Modal";
import Backdrop from "[@material](http://twitter.com/material)-ui/core/Backdrop";
import { useSpring, animated } from "react-spring/web.cjs"; // web.cjs is required for IE 11 supportconst useStyles = makeStyles(theme => ({
  modal: {
    display: "flex",
    alignItems: "center",
    justifyContent: "center"
  },
  paper: {
    backgroundColor: theme.palette.background.paper,
    border: "2px solid #000",
    boxShadow: theme.shadows[5],
    padding: theme.spacing(2, 4, 3)
  }
}));const Fade = React.forwardRef(function Fade(props, ref) {
  const { in: open, children, onEnter, onExited, ...other } = props;
  const style = useSpring({
    from: { opacity: 0 },
    to: { opacity: open ? 1 : 0 },
    onStart: () => {
      if (open && onEnter) {
        onEnter();
      }
    },
    onRest: () => {
      if (!open && onExited) {
        onExited();
      }
    }
  }); return (
    <animated.div ref={ref} style={style} {...other}>
      {children}
    </animated.div>
  );
});export default function SpringModal() {
  const classes = useStyles();
  const [open, setOpen] = React.useState(false); const handleOpen = () => {
    setOpen(true);
  }; const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <button type="button" onClick={handleOpen}>
        open menu
      </button>
      <Modal
        className={classes.modal}
        open={open}
        onClose={handleClose}
        closeAfterTransition
        BackdropComponent={Backdrop}
        BackdropProps={{
          timeout: 500
        }}
      >
        <Fade in={open}>
          <div className={classes.paper}>
            <h2>Spring modal</h2>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
          </div>
        </Fade>
      </Modal>
    </div>
  );
}
```

我们创建了`Fade`组件。

我们使用 react-spring 提供的`useSpring`钩子来添加动画。

我们在`friom`属性中指定入过渡的过渡样式。

此外，我们用`to`属性指定出过渡的过渡样式。

`onStart`和`onExited`是分别在过渡开始和结束时调用的函数，用于开始和结束过渡。

然后我们在情态中使用`Fade`。

# 结论

我们可以通过使用来自材质 UI 或 react-spring 的组件来创建带有过渡的模态。