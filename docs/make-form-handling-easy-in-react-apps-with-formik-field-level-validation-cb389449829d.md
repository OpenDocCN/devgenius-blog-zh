# 使用 Formik 简化 React 应用程序中的表单处理—字段级验证

> 原文：<https://blog.devgenius.io/make-form-handling-easy-in-react-apps-with-formik-field-level-validation-cb389449829d?source=collection_archive---------3----------------------->

![](img/8253f460a323dec67f9489f8a7f8a9b1.png)

照片由 [Sean Musil](https://unsplash.com/@seanmusil?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Formik 是一个使 React 应用程序中的表单处理变得简单的库。

在本文中，我们将看看如何用 Formik 处理表单输入。

# 字段级验证

我们可以通过创建验证函数并将其传递给`Field`组件的`validate`属性来添加字段级验证。

例如，我们可以写:

```
import React from "react";
import { Formik, Form, Field } from "formik";function validateEmail(value) {
  let error;
  if (!value) {
    error = "Required";
  } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(value)) {
    error = "Invalid email address";
  }
  return error;
}export const FormExample = () => (
  <div>
    <h1>Signup</h1>
    <Formik
      initialValues={{
        email: ""
      }}
      onSubmit={(values) => {
        console.log(values);
      }}
    >
      {({ errors, touched, isValidating }) => (
        <Form>
          <Field name="email" validate={validateEmail} />
          {errors.email && touched.email && <div>{errors.email}</div>}
          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  </div>
);export default function App() {
  return (
    <div className="App">
      <FormExample />
    </div>
  );
}
```

我们创建了接受`value`参数的`validateEmail`函数，该参数有输入值。

然后我们检查该值，并在检查该值是否有效后返回验证错误消息。

在`FormExample`中，我们用`Formik`和`Form`组件创建表单。

`initialValues`有每个字段的初始值。

`Field`组件的`name`属性设置为与`initialValues`中相同的值。

`validate`有我们之前创建的验证函数。

`errors`有错误信息。

`touched`具有输入的触摸状态。

`isValidating`告诉我们表单是否被验证。

# 手动触发验证

我们可以用 Formik 手动触发验证。

例如，我们可以写:

```
import React from "react";
import { Formik, Form, Field } from "formik";
function validateUsername(value) {
  let error;
  if (value === "admin") {
    error = "Nice try!";
  }
  return error;
}export const FormExample = () => (
  <div>
    <h1>Signup</h1>
    <Formik
      initialValues={{
        username: "",
      }}
      onSubmit={(values) => {
        console.log(values);
      }}
    >
      {({ errors, touched, validateField, validateForm }) => (
        <Form>
          <Field name="username" validate={validateUsername} />
          {errors.username && touched.username && <div>{errors.username}</div>}
          <button type="button" onClick={() => validateField("username")}>
            Check Username
          </button>
          <button
            type="button"
            onClick={async () => {
              const result = await validateForm();
              console.log(result);
            }}
          >
            Validate All
          </button>
          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  </div>
);export default function App() {
  return (
    <div className="App">
      <FormExample />
    </div>
  );
}
```

我们有验证`username`字段的`validateUsername`函数。

在`Form`元素中，我们有 Check Username 按钮，它用`'username'`调用`validateField`，这是`Field`组件的`name`值。

名称将匹配，因此将运行`validateUsername`功能。

我们可以用`validateForm`函数来验证表单中的所有字段，该函数会返回一个包含所发现的任何错误的承诺。

因此，如果我们在输入中键入“admin ”,返回的承诺将解析为:

```
{username: "Nice try!"}
```

# 结论

我们可以添加字段级验证，并用 Formik 手动触发表单验证。