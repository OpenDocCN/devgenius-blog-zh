# 使用 Formik-Hooks 在 React 应用程序中轻松处理表单

> 原文：<https://blog.devgenius.io/make-form-handling-easy-in-react-apps-with-formik-hooks-544980da0cea?source=collection_archive---------2----------------------->

![](img/f4eff82253458071407010e097ea70e2.png)

[猎人詹姆斯](https://unsplash.com/@hunterjamesphotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Formik 是一个使 React 应用程序中的表单处理变得简单的库。

在本文中，我们将看看如何用 Formik 处理表单输入

# 使用字段

我们可以使用`useField`钩子来创建自己的文本字段，用于 Formik。

例如，我们可以写:

```
import React from "react";
import { useField, Form, Formik } from "formik";const MyTextField = ({ label, ...props }) => {
  const [field, meta] = useField(props);
  return (
    <>
      <label>
        {label}
        <input {...field} {...props} />
      </label>
      {meta.touched && meta.error ? (
        <div className="error">{meta.error}</div>
      ) : null}
    </>
  );
};export const FormExample = () => (
  <div>
    <Formik
      initialValues={{
        email: ""
      }}
      onSubmit={(values, actions) => {
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          actions.setSubmitting(false);
        }, 1000);
      }}
    >
      {(props) => (
        <Form>
          <MyTextField name="email" type="email" label="Email" />
          <button type="submit">Submit</button>
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

我们用`useField`钩子创建了`MyTextField`组件。

我们将道具和`field`对象作为道具传入输入，让 Formik 绑定到状态，并设置类似`touched`和`error`的输入状态。

然后在`FormExample`中，我们像往常一样用`Formik`和`Form`组件创建表单。

但是我们使用`MyTextField`组件将输入字段添加到表单中。

# 使用 Formik

我们可以使用`useFormik`钩子来创建 Formik 表单。

例如，我们可以写:

```
import React from "react";
import { useFormik } from "formik";export const FormExample = () => {
  const formik = useFormik({
    initialValues: {
      firstName: "",
      lastName: "",
      email: ""
    },
    onSubmit: (values) => {
      alert(JSON.stringify(values, null, 2));
    }
  });
  return (
    <form onSubmit={formik.handleSubmit}>
      <label htmlFor="firstName">First Name</label>
      <input
        id="firstName"
        name="firstName"
        type="text"
        onChange={formik.handleChange}
        value={formik.values.firstName}
      />
      <label htmlFor="lastName">Last Name</label>
      <input
        id="lastName"
        name="lastName"
        type="text"
        onChange={formik.handleChange}
        value={formik.values.lastName}
      />
      <label htmlFor="email">Email Address</label>
      <input
        id="email"
        name="email"
        type="email"
        onChange={formik.handleChange}
        value={formik.values.email}
      />
      <button type="submit">Submit</button>
    </form>
  );
};export default function App() {
  return (
    <div className="App">
      <FormExample />
    </div>
  );
}
```

我们将`useFormik`添加到`FormExample`中，让我们用 Formik 创建一个表单。

我们通过里面的`initialValues`来设置初始值。

`onSubmit`有表单的提交处理程序。

然后我们可以从`formik`对象中获取属性。

这包括设置为`formik.handleSubmit`的`onSubmit`回调。

`formik.handleChange`是表单字段的变更处理程序。

`form.values`具有表单值。

# useFormikContext

我们可以使用`useFormikContext`钩子让我们从表单内部的组件访问表单。

例如，我们可以写:

```
import React from "react";
import { Field, Form, Formik, useFormikContext } from "formik";const AutoSubmitToken = () => {
  const { values, submitForm } = useFormikContext();
  React.useEffect(() => {
    if (values.token.length === 6) {
      submitForm();
    }
  }, [values, submitForm]);
  return null;
};export const FormExample = () => {
  return (
    <Formik
      initialValues={{ token: "" }}
      validate={(values) => {
        const errors = {};
        if (values.token.length < 5) {
          errors.token = "Invalid code. Too short.";
        }
        return errors;
      }}
      onSubmit={(values, actions) => {
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          actions.setSubmitting(false);
        }, 1000);
      }}
    >
      <Form>
        <Field name="token" type="tel" />
        <AutoSubmitToken />
      </Form>
    </Formik>
  );
};export default function App() {
  return (
    <div className="App">
      <FormExample />
    </div>
  );
}
```

我们有带有表单值的属性`values`的`AutoSubmitToken`组件。

`submitForm`具有表单提交功能。

返回具有所有这些属性的对象。

`FormExample`有创建表单的常用道具和组件，自动提交表单的`AutoSubmitToken`组件`values.token.length`大于或等于 6。

# 结论

我们可以使用 Formik 自带的钩子来创建表单。