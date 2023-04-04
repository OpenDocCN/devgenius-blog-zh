# 使用 React 挂钩实现联系人表单

> 原文：<https://blog.devgenius.io/implementing-a-contact-form-using-react-hooks-c30ec4aa59ab?source=collection_archive---------3----------------------->

![](img/baabdfc4871fc720aaf62eac364ecaf5.png)

图多尔·巴休在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在过去的一周里，我一直在使用 React 实现一个[联系表单](https://sbtc9.csb.app/)。由于我也一直在学习 React Hooks，所以我决定将我的 ContactForm 实现为一个功能组件，并使用 React Hooks，而不是从 App.js 传入 props 或将本地状态存储在 Contact Form 组件本身中。

## 要求

*   联系形式将采取 4 个输入字段:姓名，电子邮件地址，主题和自由形式的信息
*   输入字段下方的提交按钮
*   输入字段左侧的图片
*   表单验证，检查输入字段是否为空，或者电子邮件输入是否为有效输入
*   如果有任何错误，在提交按钮的正上方显示一些红色文本，说明哪个输入字段为空/无效。
*   如果所有输入字段都正确，则显示一条带有文本“电子邮件已发送”的警告

## 联系人表单组件

我将联系人表单实现为一个功能组件，使用 *useState* 钩子来存储我的状态。**use state 钩子允许你给函数组件添加 React 状态**。首先，我为所有的输入字段创建了状态变量。***use state*函数接受变量**的初始值。因为我们所有的变量都是字符串，所以我用一个空字符串作为 useState 的参数来初始化它们。

我使用 material-ui 库来实现我的表单元素。

```
import React, { useState } from "react";import { TextField, Button } from "@material-ui/core";function CountactUs() { const [email, setEmail] = useState(""); const [name, setName] = useState(""); const [subject, setSubject] = useState(""); const [message, setMessage] = useState("");return(
 <div> 
   <form> <form>
 </div>)
```

**use state 返回一对值:当前状态和更新它的函数**。然后，我们使用下面的语法来析构返回值。

```
**const [subject, setSubject]** = useState("");
```

为了捕获来自用户的输入，我们使用由 useState 返回的函数，该函数被设置为文本字段的 onChange 函数和由 e . target . value(event . target . value)更新的状态变量。下面的代码所做的是注册对文本字段的任何更改，并将它们设置为各自的状态变量。

```
<form> 
           <TextField label="Name" type ="text" **onChange = {e => setName({name: e.target.value})}**/> <TextField label="Email"  type ="email" **onChange ={e => setEmail({email: e.target.value})}**/><TextField label="Subject" type ="text"  **onChange = {e => setSubject({subject: e.target.value})}**/> <TextField label="Message" type ="text" " **onChange  ={e => setMessage({message: e.target.value})}**/> <Button color="primary" type="button">  Submit </Button>
          </form>
```

## 错误验证

我为错误消息创建了一个数组状态变量，它被初始化为一个空数组，并创建了另一个布尔状态变量来确定是显示还是隐藏错误。

```
const [errorMessages, setErrorMessages] = useState([]);const [showErrors, setShowErrors] = useState(false);
```

我创建了一个 formValidation 函数来检查输入字段是否为空或无效。

我创建了一个全局错误数组，在那里我可以适当地放入错误消息。在 formValidation 方法的开始，我将 errorMessages 状态变量重置为一个空数组，这样每当有人提交表单时，以前的错误就不会被存储。

我使用 ValidateEmail(email)函数根据我在网上找到的 regex 模式来验证电子邮件输入。

在检查输入语句的有效性之后，我的最后一个 if 语句检查 errors.length 是否> 0，如果是，我将错误消息状态变量设置为 errors 数组，并将 ShowErrors 设置为 true。否则，我将 ShowErrors 设置为 false，并发出警告消息“Email sent！”

```
let errors = [];//validate email input function ValidateEmail(email) {
if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(email)) {return true;}return false;}const formValidation = () => { setErrorMessages([]); const isNameValid = (name !== "");
   const isMessageValid = ( message !== "");
   const isSubjectValid = (subject !== ""); if (!isNameValid) {
   errors.push("Name is not valid, please try again.");
}if (!ValidateEmail(email)) {
   errors.push("Email is not valid, please try again.");
}if (email === "") {
   errors.push("Email field is empty, please try again.")
}if (!isMessageValid) {
  errors.push("Message is not valid, please try again.");
}if (!isSubjectValid) {
   errors.push("Subject is not valid, please try again.");
}if (errors.length > 0) { setShowErrors({ showErrors: true });
  setErrorMessages(errors);
} 
else { setShowErrors({ showErrors: false });
   alert("Email Sent");
}};
```

然后我将 formValidation 方法传递给按钮，这样每次用户提交表单时都会进行验证。

```
<Button variant="contained" color="primary" type="button" **onClick = {formValidation}**>  Submit </Button>
```

我通过映射按钮上方的 error messages 变量，根据 showErrors 是真还是假，有条件地将错误消息呈现为 ul 元素。

```
{showErrors ? errorMessages.map((item, index) => {
        return <ul key={index}>{item}</ul>;}) : null<Button ... </Button>
```

这对我来说是一个有趣的挑战，因为我有机会测试我的反应钩技能。你可以在这里找到成品:)