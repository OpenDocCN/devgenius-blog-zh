# React 提示—共享数据、模拟函数和本地存储

> 原文：<https://blog.devgenius.io/react-tips-share-data-mock-functions-and-local-storage-a6c3d4e6e7ac?source=collection_archive---------6----------------------->

![](img/0d83eee3ebc44ad2483d224532c324e3.png)

照片由[玛利亚·特内娃](https://unsplash.com/@miteneva?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 从使用者更新提供者中的上下文值

如果我们将函数传入上下文，就可以从提供者那里更新上下文值。

例如，我们可以写:

```
const MyContext = React.createContext({});class Child extends Component {
  constructor(props) {
    super(props);
    this.state = {        
      name: ""
    };
  } onChange(e){
    const name = e.target.value;
    this.setState({
      name
    });
    this.props.context.updateValue('name', name);
  } render() {
    return (
       <React.Fragment>
         <input onChange={this.onChange} />
       </React.Fragment>
    )
  }
}const withContext = (Component) => {
  return (props) => {
    <MyContext.Consumer>    
      {(context) => {
           return <Component {...props} context={context} />
      }}
    </MyContext.Consumer>
  }
}Child = withContext(Child)class Parent extends Component {
  constructor(props) {
    super(props);
    this.state = {
      name: "bar",
    };
  } updateValue = (key, val) => {
    this.setState({[key]: val});
  }

  render() {
    return (
      <MyContext.Provider value={{ state: this.state, updateValue: this.updateValue }}>      
        <Child /> 
      </MyContext.Provider>
    )
  }
}
```

我们创建上下文并将值从`Parent`传递给上下文中的组件。

该对象具有`state`和`updateValue`功能。

然后我们从`props.context`属性中获取`updateValue`方法，这就是我们所拥有的。

然后我们通过调用`updateValue`方法设置`Parent`的`name`状态来设置名称。

我们必须记住将`MyContext.Consumer`添加到任何消耗上下文的组件中。

为此，我们创建了`withContext`高阶组件，用上下文消费者包装任何组件。

# React 类的 getInitialState

我们将初始状态放在类组件的构造函数中。

例如，我们可以写:

```
import { Component } from 'react';class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      foo: true,
      bar: 'no'
    };
  } render() {
    return (
      <div className="theFoo">
        <span>{this.state.bar}</span>
      </div>
    );
  }
}
```

我们在构造函数中设置了`foo`和`bar`状态的初始值。

# 将本地存储与 React 一起使用

我们可以在 React 中使用本地存储方法。

例如，我们可以写:

```
import React from 'react'class App extends React.Component {
  constructor(props) {
    super(props);
    const storedClicks = 0; if (localStorage.getItem('clicks')) {
      storedClicks = +(localStorage.getItem('clicks'));
    } this.state = {
      clicks: storedClicks
    };
    this.click = this.click.bind(this);
  } onClick() {
    const newClicks = this.state.clicks + 1;
    this.setState({ clicks: newClicks });
    localStorage.setItem('clicks', newClicks);
  } render() {
    return (
      <div>
        <button onClick={this.onClick}>Click me</button> 
        <p>{this.state.clicks} clicks</p>
      </div>
    );
  }
}
```

如果本地存储存在，我们会从本地存储中获取点击。

然后我们解析它是否存在，并将其设置为`clicks`状态的初始值。

然后在`render`方法中，我们有一个按钮用`onClick`方法更新`clicks`状态和本地存储`clicks`值。

它更新`clicks`状态。

此外，我们在那之后更新本地存储的`clicks`值。

# 用 Jest 测试 React 组件函数

我们可以模仿组件所依赖的函数，这样我们就可以用它进行测试。

例如，我们可以写:

```
import React, { Component } from 'react';class Child extends Component {
   constructor (props) {
      super(props);
   }; render() {
      const { onSubmit, label} = this.props;
      return(
        <form  onSubmit={onSubmit}>
          <Button type='submit'>{label}</Button>
        </form >
      );
   };
};export default class App extends Component {
  constructor (props) {
    super(props);
    this.label = “foo”;
  }; onSubmit = (option) => {
    console.log('submitted');
  }; render () {
    return(
      <div className="container">
        <Child label={this.label} onSubmit={this.onSubmit} />
      </div>
    );
  };
};
```

然后，当我们通过编写以下代码来测试孩子时，我们可以模仿`onSubmit`函数:

```
import React from 'react';
import { shallow } from 'enzyme';
import Child from '../Child';const onSubmitSpy = jest.fn();
const onSubmit = onSubmitSpy;const wrapper = shallow(<Child onSubmit={onSubmitSpy} />);
let container, containerButton;describe("Child", () => {
  beforeEach(() => {
    container = wrapper.find("div");
    containerButton = container.find(“Button”);
    onSumbitSpy.mockClear();
  }); describe("<Button> behavior", () => {
     it("should call onSubmit", () => {
       expect(onSubmitSpy).not.toHaveBeenCalled();
       containerButton.simulate(‘click’);
       expect(onSubmitSpy).toHaveBeenCalled();
     });
  });
});
```

我们用由`onSubmitSpy`填充的`onSubmit`属性挂载组件，这是一个模拟函数。

我们通过模拟`Child`组件按钮上的点击事件进行测试。

然后我们检查被模仿的函数是否用`toHaveBeenCalled`调用。

![](img/1df220504ee7329ead437ea1493a0a93.png)

照片由[丹尼尔·塔特尔](https://unsplash.com/@danieltuttle?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用上下文 API 传递数据。

此外，我们可以用 Jest 模仿函数，这样我们就可以把它们作为道具传入来测试我们需要测试的东西。

本地存储可以在 React 组件中按原样使用。