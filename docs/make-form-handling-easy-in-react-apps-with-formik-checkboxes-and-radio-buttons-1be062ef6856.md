# 使用 Formik(复选框和单选按钮)在 React 应用程序中轻松处理表单

> 原文：<https://blog.devgenius.io/make-form-handling-easy-in-react-apps-with-formik-checkboxes-and-radio-buttons-1be062ef6856?source=collection_archive---------5----------------------->

![](img/c6bc560a7eb2522b09abb8714952b10c.png)

[OCV 摄](https://unsplash.com/@ocv_photo?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的

Formik 是一个使 React 应用程序中的表单处理变得简单的库。

在本文中，我们将看看如何用 Formik 处理表单输入。

# 复选框

我们可以用 Formik 将复选框值绑定到状态。

例如，我们可以写:

```
import React from "react";
import { Formik, Form, Field } from "formik";export const FormExample = () => (
  <div>
    <Formik
      initialValues={{
        toggle: false,
        checked: []
      }}
      onSubmit={(values) => {
        alert(JSON.stringify(values, null, 2));
      }}
    >
      {({ values }) => (
        <Form>
          <label>
            <Field type="checkbox" name="toggle" />
            {`${values.toggle}`}
          </label>
          <div>Checked</div>
          <div>
            <label>
              <Field type="checkbox" name="checked" value="apple" />
              apple
            </label>
            <label>
              <Field type="checkbox" name="checked" value="orange" />
              orange
            </label>
            <label>
              <Field type="checkbox" name="checked" value="grape" />
              grape
            </label>
          </div> <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  </div>
);
export default function App() {
  return (
    <div className="App">
      <FormExample />
    </div>
  );
}
```

我们将`initialValues`设置为我们想要的。

在第一个`Field`组件中，我们将`name`设置为`'toggle'`以绑定到切换字段。

这将把选中的值绑定到`values.toggle`属性。

选中时为`true`，否则为`false`。

在 div 中，我们有 3 个`Field`组件。

除了设置`name`之外，我们还设置了`value`，让我们用被选中的复选框的`value`属性值填充`checked`数组。

当我们提交表单时，我们看到`checked`和`toggle`中的选中项是`true`或`false`，取决于它是否被选中。

# 单选按钮

Formik 还简化了单选按钮的状态绑定。

为此，我们写道:

```
import React from "react";
import { Formik, Form, Field } from "formik";export const FormExample = () => (
  <div>
    <Formik
      initialValues={{
        picked: ""
      }}
      onSubmit={(values) => {
        alert(JSON.stringify(values, null, 2));
      }}
    >
      {({ values }) => (
        <Form>
          <div>Picked</div>
          <div>
            <label>
              <Field type="radio" name="picked" value="apple" />
              One
            </label>
            <label>
              <Field type="radio" name="picked" value="orange" />
              Two
            </label>
            <div>Picked: {values.picked}</div>
          </div> <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  </div>
);
export default function App() {
  return (
    <div className="App">
      <FormExample />
    </div>
  );
}
```

我们将单选按钮组的`name`设置为相同的名称。

并且我们还为每个设置了`value`。

然后我们可以从具有相同`name`值的收音机中挑选一个。

我们将看到带有`value.picked`属性的值。

# 结论

我们可以用 Formik 将复选框和单选按钮的值绑定到状态。