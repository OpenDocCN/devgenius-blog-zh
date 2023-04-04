# 顶级反应库—形式和尺寸

> 原文：<https://blog.devgenius.io/top-react-libraries-forms-and-sizes-42bbc3033f50?source=collection_archive---------15----------------------->

![](img/ab381ad63dc46d348a50bed9cb71d312.png)

由 [Hermes Rivera](https://unsplash.com/@hermez777?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 反应最终形式

React Final Form 是一个软件包，可以让我们轻松地创建带有数据处理的表单。

我们还可以用它来添加我们自己的验证逻辑。

要安装它，我们运行:

```
npm i react-final-form
```

然后我们可以通过写来使用它:

```
import React from "react";
import { Form, Field } from "react-final-form";export default function App() {
  const onSubmit = values => {
    window.alert(JSON.stringify(values, 0, 2));
  };return (
    <div className="app">
      <Form
        onSubmit={onSubmit}
        validate={values => {
          const errors = {};
          if (!values.username) {
            errors.username = "Username is required";
          }
          if (!values.password) {
            errors.password = "Password is required";
          }
          if (!values.confirm) {
            errors.confirm = "Confirm password is required";
          } else if (values.confirm !== values.password) {
            errors.confirm = "Passwords must match";
          }
          return errors;
        }}
        render={({ handleSubmit, form, submitting, pristine, values }) => (
          <form onSubmit={handleSubmit}>
            <Field name="username">
              {({ input, meta }) => (
                <div>
                  <label>Username</label>
                  <input {...input} type="text" placeholder="Username" />
                  {meta.error && meta.touched && <span>{meta.error}</span>}
                </div>
              )}
            </Field>
            <Field name="password">
              {({ input, meta }) => (
                <div>
                  <label>Password</label>
                  <input {...input} type="password" placeholder="Password" />
                  {meta.error && meta.touched && <span>{meta.error}</span>}
                </div>
              )}
            </Field>
            <Field name="confirm">
              {({ input, meta }) => (
                <div>
                  <label>Confirm</label>
                  <input {...input} type="password" placeholder="Confirm" />
                  {meta.error && meta.touched && <span>{meta.error}</span>}
                </div>
              )}
            </Field>
            <div className="buttons">
              <button type="submit" disabled={submitting}>
                Submit
              </button>
              <button
                type="button"
                onClick={form.reset}
                disabled={submitting || pristine}
              >
                Reset
              </button>
            </div>
          </form>
        )}
      />
    </div>
  );
}
```

我们使用提供的`Form`组件和接受输入值的`onSubmit`属性。

`validate`是我们的验证函数。

它将输入的值作为参数，我们可以在里面检查输入值的有效性。

它返回一个带有错误的对象。

然后我们有一个`render`道具和我们渲染的表单。

`handleSubmit`功能被传递给`onSubmit`道具。

`input`有输入的道具。

`meta`具有类似错误及其接触状态的表单元数据。

`submitting`是提交时的状态。

`pristine`表示是否有过交互。

`Field`组件包围着每个字段，并为我们提供所有这些属性。

我们可以用这个图书馆做很多事情。

# 反应-调整大小-检测器

我们可以使用 react-resize-detector 包来监视元素的大小调整。

要安装它，我们运行:

```
npm i react-resize-detector
```

然后我们可以使用提供的`ReactResizeDetector`组件来观察视口的宽度和高度变化:

```
import React from "react";
import ReactResizeDetector from "react-resize-detector";export default function App() {
  return (
    <div className="app">
      <ReactResizeDetector handleWidth handleHeight>
        {({ width, height }) => <div>{`${width}x${height}`}</div>}
      </ReactResizeDetector>
    </div>
  );
}
```

我们使用带有`handleWidth`和`handleHeight`的`ReactResizeDetector`组件来观察宽度和高度的变化。

此外，我们可以使用`withResizeDetector`高阶组件来创建带有`width`和`height`道具的组件:

```
import React from "react";
import { withResizeDetector } from "react-resize-detector";const Comp = withResizeDetector(({ width, height }) => (
  <div>{`${width}x${height}`}</div>
));export default function App() {
  return (
    <div className="app">
      <Comp />
    </div>
  );
}
```

我们将一个组件传递给了更高级的组件`withResizeDetector`来观察`width` 和`height`道具。

这样，我们得到了组件中的`width`和`height`道具。

我们也可以节流或谴责刷新的高度和宽度。

![](img/9c652530aa9a38593cfe6b9537cb312f.png)

米歇尔·布莱克威尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

React 最终表单库允许我们创建带验证的表单。

react-resize-detector 是一个库，可以让我们观察视口的宽度和高度的变化。