# 顶级 React 库—复选框、对齐和代码编辑器

> 原文：<https://blog.devgenius.io/top-react-libraries-checkbox-alignment-and-code-editor-34c8573dde38?source=collection_archive---------5----------------------->

![](img/c56bce8a41e81d462f1c526140551f70.png)

照片由[克里斯·莱佩尔特](https://unsplash.com/@cleipelt?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# RC-复选框

rc-checkbox 为我们提供了一个样式化的复选框组件。

要安装它，我们运行:

```
npm i rc-checkbox
```

然后我们通过书写来使用它:

```
import React from "react";
import Checkbox from "rc-checkbox";
import "rc-checkbox/assets/index.css";function onChange(e) {
  console.log(e.target.checked);
}class App extends React.Component {
  render() {
    return (
      <div style={{ margin: 20 }}>
        <div>
          <p>
            <label>
              <Checkbox checked onChange={onChange} />
              &nbsp; controlled checkbox
            </label>
            &nbsp;&nbsp;
          </p>
          <p>
            <label>
              <input checked type="checkbox" onChange={onChange} />
              &nbsp; controlled checked native checkbox
            </label>
          </p>
        </div><div>
          <p>
            <label>
              <Checkbox defaultChecked onChange={onChange} />
              &nbsp; checkbox
            </label>
            &nbsp;&nbsp;
          </p>
          <p>
            <label>
              <input
                type="checkbox"
                defaultChecked
                onChange={onChange}
                disabled
              />
              &nbsp; native
            </label>
            &nbsp;&nbsp;
          </p>
        </div><div>
          <p>
            <label>
              <Checkbox
                name="my-checkbox"
                defaultChecked
                onChange={onChange}
                disabled
              />
              &nbsp; defaultChecked rc-checkbox with name
            </label>
            &nbsp;&nbsp;
          </p>
          <p>
            <label>
              <input
                name="my-checkbox"
                type="checkbox"
                defaultChecked
                onChange={onChange}
                disabled
              />
              &nbsp; native with name
            </label>
            &nbsp;&nbsp;
          </p>
        </div>
      </div>
    );
  }
}
export default App;
```

我们使用 CSS 中的`Checkbox`组件来显示带有样式的复选框。

`onChange`属性让我们获得检查过的值。

`checked`使其受到检查。

`disabled`使其失效。

`name`有这个名字。

`type`具有输入类型。

`defaultChecked`默认选中。

# 反应代码镜像 2

react-codemirror2 是一个代码编辑器 react 组件。

要安装它，我们运行:

```
npm i react-codemirror2 codemirror
```

然后，我们可以通过编写以下内容将其作为不受控制的组件:

```
import React from "react";
import { UnControlled as CodeMirror } from "react-codemirror2";
import "codemirror/lib/codemirror.css";
import "codemirror/theme/material.css";const App = () => {
  return (
    <>
      <CodeMirror
        value="<h1>hello world</h1>"
        options={{
          mode: "xml",
          theme: "material",
          lineNumbers: true
        }}
        onChange={(editor, data, value) => {}}
      />
    </>
  );
};
export default App;
```

我们进口与款式相配的服装。

然后，我们用要显示的值设置`value`道具。

`options`有一些显示代码的选项。

`mode`是语言代码。

`theme`是主题样式编辑器。

`lineNumbers`设置为`true`表示我们想在左侧显示行号。

我们还可以导入一个控制器组件，并使用它:

```
import React from "react";
import { Controlled as CodeMirror } from "react-codemirror2";
import "codemirror/lib/codemirror.css";
import "codemirror/theme/material.css";class App extends React.Component {
  state = {
    value: "<h1>hello world</h1>"
  }; render() {
    return (
      <CodeMirror
        value={this.state.value}
        options={{
          mode: "xml",
          theme: "material",
          lineNumbers: true
        }}
        onBeforeChange={(editor, data, value) => {
          this.setState({ value });
        }}
        onChange={(editor, data, value) => {}}
      />
    );
  }
}
export default App;
```

我们用`state`将代码保存在一个字符串中。

然后我们可以用它作为`value`的值。

`options`与非受控版本相同。

我们还有`onBeforeChange`用新的输入值更新`value`状态。

它发出许多其他事件，包括焦点、拖动、按键等等。

# RC-对齐

rc-align 是一个让我们将元素对齐到给定位置的包。

要安装它，我们运行:

```
npm i rc-align
```

然后我们可以通过写来使用它:

```
import React from "react";
import Align from "rc-align";const align = {
  points: ["0", "0"]
};class App extends React.Component {
  state = {
    point: null
  }; onClick = ({ pageX, pageY }) => {
    this.setState({ point: { pageX, pageY } });
  }; render() {
    return (
      <div style={{ marginBottom: 170 }}>
        <div
          style={{
            margin: 20,
            border: "1px solid green",
            padding: "100px 0",
            textAlign: "center"
          }}
          onClick={this.onClick}
        >
          Click me
        </div> <Align ref={this.alignRef} target={this.state.point} align={align}>
          <div
            style={{
              position: "absolute",
              width: 100,
              height: 100,
              background: "lightgreen",
              pointerEvents: "none"
            }}
          >
            Align
          </div>
        </Align>
      </div>
    );
  }
}
export default App;
```

我们在`Align`组件内部创建了一个 div。

这是一个框，如果我们单击它，`Align`组件中的 div 将进入这个框。

当我们点击它时，`onClick`功能将运行，新的坐标将被设置。

`align`拥有来自 dom-align 库中的 align 配置。

将`target`设置为`point`状态的值，这样 Align div 将在框内移动。

![](img/5790b04cd8c67f25e3b3b8e1016a0778.png)

由[安东·克拉夫](https://unsplash.com/@antohakraev?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

rc-checkbox 为我们提供了复选框组件。

react-codemirror2 是一个代码编辑器组件。

rc-align 允许我们将元素对齐到给定的位置。