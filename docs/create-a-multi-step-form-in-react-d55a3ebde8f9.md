# 在 React 中创建多步表单

> 原文：<https://blog.devgenius.io/create-a-multi-step-form-in-react-d55a3ebde8f9?source=collection_archive---------1----------------------->

![](img/9ec7c28af67da633bc35d9ba0fce016a.png)

太多的流浪汉！

有时当我们创建一个表单时，可能会有很多我们想要的信息。如果你把所有的字段都放在一个页面上，可能会惹恼用户，他可能会在完成表单之前离开。如果想提高用户体验，可以做多步表单。

在本文中，我将讲述如何在 React 中创建多步表单。这是一份报名表。第一步，我会收集用户的组织信息；然后第二步，我会收集用户的个人信息。一旦全部完成，它将向您显示一条成功(或失败)的消息。

# 我们需要一个容器组件！

解决这个问题的非常直观的方法是创建三个不同的组件来完成我想分三步做的事情。确实如此！但是，想一想，我需要三个不相关的组件吗？不要！

所有这三个组成部分都是我注册过程的一部分，它们有一些共同之处。例如，在第二步中，它将提交从第二个和第一个组件获得的信息。提交后，第三个组件将根据您是否提交成功显示提示。

如何在三个不同的组件之间共享所有这些信息？答案是:**使用容器组件！**

有了顶部的父组件，我们可以拥有以下优势:

1.  父组件可以包含所有子组件共享的公共方法和变量，方法是将它们作为道具传递下去。所以，你可以保持代码干爽(不要重复)！当您需要更改所有三个组件都将使用的方法或变量时，代码维护会更容易。
2.  此外，当一个孩子改变一个家长的状态字段时，其他孩子也会得到通知，因为他们共享同一个家长！这样，您就不必手动地将新输入的信息传递给所有组件，这是非常容易出错的。
3.  子组件可以用函数组件而不是类组件来编写。这样，它将提高应用程序的性能，因为功能组件只是没有生命周期的无状态普通 JavaScript 函数。这样可以节省渲染时进行生命周期相关检查的时间。

# 实现子组件

正如我们刚刚谈到的，所有的子组件都是功能组件。它们不处理任何业务逻辑，而只是显示或接受从父组件传来的任何属性(变量)。

典型的子组件如下所示:

```
import React from "react"const CompanyInfo = props =>{
  const { values, handleChange, next} = props
  return(
      <>
        <label for="companyName" >
          Company Name
          <input
            type="text"
            name="companyName"
            value={values.companyName}
            // to invoke the handleChange method, you need to specify a name to it so that the state will change accordingly
            onChange={handleChange('companyName')}
          />
        </label ><br />
        <label for="companyAddress">
          Company Address
          <input
            type="text"
            name="companyAddress"
            value={values.companyAddress}
            // to invoke the handleChange method, you need to specify a name to it so that the state will change accordingly
            onChange={handleChange('companyAddress')}
          />
        </label ><br />
          <button onClick={next}>Next</button>//since it is in first step, so only one "Next" button
    </>
  )}export default CompanyInfo;
```

通常，除了第一步，它会返回**输入框和两个按钮**。这两个按钮是“下一步”和“上一步”,您可以使用它们在表单的不同部分之间来回切换。

但是因为是在第一步，所以只有一个“下一步”按钮，在最后一步，两个按钮是“返回”和“提交”。

该组件将通过父组件传递的 props 得到字段的`handleChange`方法、`next`方法和`values`(因为它是 React 的一种受控形式)以正确运行。

# 实现父组件

父组件需要保存字段的`handleChange`方法、`next`方法、`back`方法、`handleSubmit`方法和`values`，以便根据需要传递给不同的子组件。

我的父组件如下所示:

```
// pages/signup.js
import React from "react"
import Layout from "../components/layout";
import CompanyInfo from "../components/signupComponents/companyInfo";
import UserInfo from "../components/signupComponents/userInfo";
import AfterSubmit from "../components/signupComponents/afterSubmit";
// import "../components/style/signUp.css"export default class signUp extends React.Component {
  state = {
    step: 1,
    companyName: "",
    companyAddress: "",
    name: "",
    phone:"",
    signupSuccess: false  
  }// process to next step
  next = () => {
    // update state.step by adding to previous state
    this.setState(prevState => {
      return {step: prevState.step + 1
    }})
  }

  // process to previous step
  back = () => {
    // update state.step by minus 1 from previous state
    this.setState(prevState => {
      return {step: prevState.step - 1
    }})
  }

  //handle all the field changes
  handleChange = input => e => {
    this.setState({
      [input]: e.target.value
    })
    console.log(this.state)
  }//handle form submit
  handleSubmit = () =>{
    //connect to the database, depending on the returned state, 
    //will change the state.signupSuccess to be true
    //and then render success component
    this.setState({
      signupSuccess: true
    }, () => this.next())
  }

  render(){
    const { step } = this.state
    const { companyName, companyAddress, name, phone,signupSuccess } = this.state
    const values = { companyName, companyAddress, name, phone,signupSuccess }switch (step) {
      case 1:
        return (
        <Layout>
          <CompanyInfo 
          values = {values} 
          handleChange = {this.handleChange}
          next = {this.next}
          />
        </Layout>
        ) case 2:
        return (
          <Layout>
          <UserInfo 
          values = {values} 
          handleChange = {this.handleChange}
          back = {this.back}
          handleSubmit = {this.handleSubmit}
          />
        </Layout>
        )

      case 3:
        return <AfterSubmit signupSuccess = {values.signupSuccess}/>
    }
  }}
```

## 使用“步骤”渲染不同的组件

如您所见，我添加了一个`step`状态字段来控制我想要呈现哪个组件。它默认为“1”，所以当指向我的注册页面时，它将呈现`CompanyInfo`组件。

我们改变另一个组件的方法是改变`step`的值。这就是为什么`next`和`back`方法基本上都是改变`step`的状态，这样父组件就会相应地改变组件。

## 使用“注册成功”来控制表单提交后发生的事情

我还在这里添加了另一个`signupSuccess`来控制表单提交后表单应该做什么。

如您所见，在`handleSubmit`方法中，它将连接到数据库并尝试创建一个用户(我没有将代码放在那里，因为这不是本文的重点)。

一旦完成，您可以根据服务器返回的内容更改`signupSuccess`值:如果成功返回，您将设置状态的`signupSuccess`为真。并且还调用回调，这里是`next`方法。这将改变状态的`step`,以便呈现`AfterSubmit`组件。

`AfterSubmit`可以访问`signupSuccess`并且会有类似这样的东西来控制流量:

```
{props.signupSuccess ? <SuccessComponnet /> : <FailedComponent />
```

这样，我们成功地在 React 中创建了一个多步表单！感谢阅读！

# 结束