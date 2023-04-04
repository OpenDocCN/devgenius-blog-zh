# 借助 Formik 相关字段和对象数组字段，在 React 应用中轻松处理表单

> 原文：<https://blog.devgenius.io/make-form-handling-easy-in-react-apps-with-formik-dependant-fields-and-object-array-fields-984d3b1041da?source=collection_archive---------4----------------------->

![](img/405a571d45dd7ad896c6442bec09ecb2.png)

[Ales Krivec](https://unsplash.com/@aleskrivec?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Formik 是一个使 React 应用程序中的表单处理变得简单的库。

在本文中，我们将看看如何用 Formik 处理表单输入

# 从属字段

我们可以根据一个字段的值来设置另一个字段的值。

例如，我们可以写:

```
import React from "react";
import { Formik, Field, Form, useField, useFormikContext } from "formik";const MyField = (props) => {
  const {
    values: { textA },
    touched,
    setFieldValue
  } = useFormikContext();
  const [field, meta] = useField(props); React.useEffect(() => {
    if (textA.trim() !== "" && touched.textA) {
      setFieldValue(props.name, `textA: ${textA}`);
    }
  }, [textA, touched.textA, setFieldValue, props.name]); return (
    <>
      <input {...props} {...field} />
      {!!meta.touched && !!meta.error && <div>{meta.error}</div>}
    </>
  );
};const initialValues = { textA: "", textB: "" };export const FormExample = () => (
  <div>
    <Formik
      initialValues={initialValues}
      onSubmit={async (values) => alert(JSON.stringify(values, null, 2))}
    >
      <Form>
        <label>
          textA
          <Field name="textA" />
        </label>
        <label>
          textB
          <MyField name="textB" />
        </label>
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

我们创建了`MyField`组件，并使用`useFormikContext`组件来获取`textA`字段的状态。

我们从`values.textA`属性中获取它的值。

`touched`有感动的价值。

`setFieldValue`让我们设置另一个字段的字段值。

我们观察具有`useEffect`回调的`textA`字段的值和具有`textA`字段值的数组，具有`textA`、`setFieldValue`和`props.name`的已触摸状态的`touched.textA`属性。

我们观察他们的变化。

然后在`FormExample`中，我们使用`MyField`来创建`textB`字段。

它观察`textA`字段的变化，因此当我们在`textA`字段中输入内容时，我们会看到用`textA`的值设置的`textB`的文本。

# 对象数组字段

我们可以将字段绑定到对象数组。

为此，我们写道:

```
import React from "react";
import { Formik, Field, Form, FieldArray, ErrorMessage } from "formik";const initialValues = {
  friends: [
    {
      name: "",
      email: ""
    }
  ]
};export const FormExample = () => (
  <div>
    <Formik
      initialValues={initialValues}
      onSubmit={async (values) => {
        await new Promise((r) => setTimeout(r, 500));
        alert(JSON.stringify(values, null, 2));
      }}
    >
      {({ values }) => (
        <Form>
          <FieldArray name="friends">
            {({ remove, push }) => (
              <div>
                {values.friends.length > 0 &&
                  values.friends.map((friend, index) => (
                    <div className="row" key={index}>
                      <div className="col">
                        <label htmlFor={`friends.${index}.name`}>Name</label>
                        <Field name={`friends.${index}.name`} type="text" />
                        <ErrorMessage
                          name={`friends.${index}.name`}
                          component="div"
                          className="field-error"
                        />
                      </div>
                      <div className="col">
                        <label htmlFor={`friends.${index}.email`}>Email</label>
                        <Field name={`friends.${index}.email`} type="email" />
                        <ErrorMessage
                          name={`friends.${index}.name`}
                          component="div"
                          className="field-error"
                        />
                      </div>
                      <div className="col">
                        <button
                          type="button"
                          className="secondary"
                          onClick={() => remove(index)}
                        >
                          X
                        </button>
                      </div>
                    </div>
                  ))}
                <button
                  type="button"
                  className="secondary"
                  onClick={() => push({ name: "", email: "" })}
                >
                  Add
                </button>
              </div>
            )}
          </FieldArray>
          <button type="submit">Invite</button>
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

我们只是在`Field`组件中将`name`设置为我们想要绑定到的属性的路径。

同样，我们对`ErrorMessage`字段进行同样的操作，以获得具有给定属性路径的字段的表单验证错误消息。

要删除一个条目，我们可以使用 Formik 附带的`remove`函数。

# 结论

我们可以用 Formik 添加依赖字段并将字段绑定到一个对象数组。