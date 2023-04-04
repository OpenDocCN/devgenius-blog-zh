# React 提示—表单验证、链接和同构组件

> 原文：<https://blog.devgenius.io/react-tips-form-validation-links-and-isomorphic-components-cdda1cf363e3?source=collection_archive---------17----------------------->

![](img/1e8cc3e4c3f8de84a664a249211f32cb.png)

照片由[александргосс](https://unsplash.com/@boney?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 使用 React 进行表单输入验证

我们可以用自己的检查进行表单验证。

例如，我们可以在提交处理程序中进行值检查。

我们可以写:

```
class Signup extends React.Component{
  constructor(props){
    super(props);
     this.state = {
       isDisabled:true
     }                                                                                                 
     this.submitForm = this.submitForm.bind(this);
  }

  handleChange(e){
    const target = e.target;
    const value = target.value;
    const name = target.name;
    this.setState({
      [name]: value
    }); if (e.target.name==='firstname'){
      if (e.target.value === '' || e.target.value === null){
        this.setState({
          firstnameError: true
        })
      } else {
        this.setState({
          firstnameError: false,     
          firstName: e.target.value
        })
      }
    } if (e.target.name==='lastname'){
      if(e.target.value==='' || e.target.value === null) {
        this.setState({
          lastnameError: true
        })
      } else {
        this.setState({
          lastnameError:false,
          lastName:e.target.value
        })
      }
    }

  } submitForm(e){
    e.preventDefault();
    const data = {
     firstName: this.state.firstName,
     lastName: this.state.lastName
    }
  } render(){
    return (
      <form>
        <div>
          <input type="text" id="firstname" name="firstname" placeholder="firstname" onChange={(e)=>{this.handleChange(e)}} />
          <label htmlFor="firstname">firstname</label>
            {this.state.firstnameError ? 'Please Enter some value' : ''} 
        </label>
        <div>
           <input type="text" name="lastname" placeholder="lastname" onChange={(e)=>{this.handleChange(e)}} />
            <label htmlFor="lastname">lastname</label>
              {this.state.lastnameError ? '>Please Enter some value' : ''}
            </label>
         </div>              
         <button onClick={this.submitForm}>Signup</button>
      </form>
    );
  }
}
```

我们有名字和姓氏字段。

我们观察它们的输入值，并在`handleChange`方法中检查它们。

`target.name`有 name 属性的值。

`target.value`有输入值。

然后在每个字段中，我们检查它是否被填写。

在`submitForm`方法中，我们获取数据，然后提交它。

我们必须调用`preventDefault`来停止默认提交行为。

为了使我们的生活更容易，我们可以使用像 React Hook Forms 这样的包来进行表单验证。

对于 instacne，我们可以写:

```
import React from "react";
import useForm from 'react-hook-form';function Form() {
  const { useForm, register } = useForm();
  const contactSubmit = data => {
    console.log(data);
  }; return (
    <form onSubmit={contactSubmit}>
      <div className="col-md-6">
        <fieldset>
          <input name="name" type="text" size="30" placeholder="Name" ref={register} />
          <br />
          <input name="email" type="text" size="30" placeholder="Email" ref={register} />
        </fieldset>     
        <fieldset>
          <button id="submit" value="Submit">
            Submit
          </button>
        </fieldset>
      </div>
    </form>
  );
}
```

我们使用包装附带的`useForm`挂钩。

然后我们调用从`useForm`返回的`register`来注册输入字段，这样我们就可以通过提交处理程序获得表单值。

这些值在`contactSubmit`功能的`data`参数中。

# 在同构的 React 组件中导入 CSS 文件

在同构的 React 组件中，我们可以在导入 CSS 之前检查环境。

例如，我们可以写:

```
if (process.env.BROWSER) {
  require("./style.css");
}
```

我们在导入 CSS 之前检查环境变量。

我们只在浏览器环境中导入。

然后我们可以定义一个 Webpack 插件来设置环境变量:

```
plugins: [
  // ...
  new webpack.DefinePlugin({
    "process.env": {
      BROWSER: JSON.stringify(true)
    }
  })
]
```

# 如何递归呈现 React 中的子组件

我们可以通过在组件中呈现相同的组件来递归地呈现子组件。

例如，我们可以写:

```
import React, { Component, PropTypes } from 'react'export default class Comments extends Component {render() {
    const { comments } = this.props;

    return (
      <div>
        {comments.map(comment =>
          <div key={comment.id}>
            <span>{comment.content}</span>
            {comment.comments && <Comments comment={comment.comments} />}
          </div>
        )}
      </div>
    )
  }
}Comments.propTypes = {
  comments: PropTypes.array.isRequired
}
```

我们创建一个`Comment`组件，它接受一个`comments`属性，该属性被呈现到一个`Comment`组件数组中。

我们将`comments` prop 设为 required 和一个数组，这样如果它们被传入，我们就可以渲染它们。

# react-路由器在使用标签时刷新页面

为了在我们点击一个`a`标签时停止刷新页面，我们应该使用`Link`组件。

例如，我们可以写:

```
import React from "react";
import { Link } from 'react-router-dom';export class App extends React.Component {
  render() {
    return (
      <Link to="/foo">Click Here</Link>
    )
  }
};
```

我们将标签`Link`与包含路线的属性`to`一起使用。

![](img/0f35fa539b80e221c3af0f2f90d61f3a.png)

照片由[艾琳·威尔森](https://unsplash.com/@erinw?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用多种方式验证表单输入。

为了让我们的生活更容易，我们应该使用一个表单验证库

此外，在同构的 React 组件中导入 CSS 之前，我们会检查环境。

如果我们使用 React 路由器，我们将使用`Link`组件进行链接。