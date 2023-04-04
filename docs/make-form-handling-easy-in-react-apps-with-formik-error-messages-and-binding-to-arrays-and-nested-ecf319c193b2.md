# 使用 Formik 简化 React 应用程序中的表单处理—错误消息以及绑定到数组和嵌套属性

> 原文：<https://blog.devgenius.io/make-form-handling-easy-in-react-apps-with-formik-error-messages-and-binding-to-arrays-and-nested-ecf319c193b2?source=collection_archive---------2----------------------->

![](img/bb7f3b128885c29be8503a1c231753b1.png)

马蒂·索思韦尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Formik 是一个使 React 应用程序中的表单处理变得简单的库。

在本文中，我们将看看如何用 Formik 处理表单输入。

# 显示错误消息

我们可以通过呈现`errors`对象的值来显示表单验证错误消息。

例如，我们可以写:

```
import React from "react";
import { Formik, Form, Field } from "formik";
import * as Yup from "yup";const schema = Yup.object().shape({
  email: Yup.string().email("Invalid email").required("Required")
});export const FormExample = () => (
  <div>
    <Formik
      initialValues={{
        email: ""
      }}
      validationSchema={schema}
      onSubmit={(values) => {
        console.log(values);
      }}
    >
      {({ errors, touched }) => (
        <Form>
          <Field name="email" />
          {touched.email && errors.email && <div>{errors.email}</div>}
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

然后我们呈现`errors.email`属性来获取`email`字段的值。

当我们键入非电子邮件地址的内容时，我们会看到显示的错误消息。

# 数组和嵌套对象

Formik 处理数组和嵌套对象。

例如，我们可以通过编写以下代码将表单字段绑定到嵌套属性:

```
import React from "react";
import { Formik, Form, Field } from "formik";export const FormExample = () => (
  <div>
    <Formik
      initialValues={{
        social: {
          facebook: "",
          twitter: ""
        }
      }}
      onSubmit={(values) => {
        console.log(values);
      }}
    >
      <Form>
        <Field name="social.facebook" />
        <Field name="social.twitter" />
        <button type="submit">Submit</button>
      </Form>
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

在`initialValues`道具中，我们有`social`物体，里面有一些属性。

为了绑定到这些属性，我们只需将`Field`组件的`name`属性设置为这些属性的属性路径。

那么`onSubmit`回调中的`values`将会设置这些值。

我们还可以将我们的`Field`输入值绑定到数组条目。

为此，我们写道:

```
import React from "react";
import { Formik, Form, Field } from "formik";export const FormExample = () => (
  <div>
    <Formik
      initialValues={{
        friends: ["james", "mary"]
      }}
      onSubmit={(values) => {
        console.log(values);
      }}
    >
      <Form>
        <Field name="friends[0]" />
        <Field name="friends[1]" />
        <button type="submit">Submit</button>
      </Form>
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

我们只是将`name`属性设置为我们想要绑定的数组条目的路径。

此外，我们可以通过添加带点的属性来分隔路径段，从而移除嵌套。

例如，我们可以写:

```
import React from "react";
import { Formik, Form, Field } from "formik";export const FormExample = () => (
  <div>
    <Formik
      initialValues={{
        "owner.name": ""
      }}
      onSubmit={(values) => {
        console.log(values);
      }}
    >
      <Form>
        <Field name="['owner.name']" />
        <button type="submit">Submit</button>
      </Form>
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

我们在`initialValues`中有`'owner.name'`属性。

然后我们将`name`道具设置为`['owner.name']`进行自动绑定。

# 结论

我们可以显示表单验证错误消息，并用 Formik 将输入字段绑定到嵌套属性和数组条目。