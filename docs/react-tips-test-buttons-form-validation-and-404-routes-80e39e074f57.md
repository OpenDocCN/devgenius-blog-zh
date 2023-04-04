# React 提示—测试按钮、表单验证和 404 路线

> 原文：<https://blog.devgenius.io/react-tips-test-buttons-form-validation-and-404-routes-80e39e074f57?source=collection_archive---------16----------------------->

![](img/570ce76c47b0c93723cd7354ec1ce1c3.png)

照片由 [Piotr Stefański](https://unsplash.com/@mnemotechnikon?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 开玩笑地模拟按钮点击

为了用 Jest 模拟按钮点击，我们可以写:

```
import React from 'react';
import { shallow } from 'enzyme';
import Button from './Button';describe('Test button', () => {
  it('Test click event', () => {
    const mockOnClick = jest.fn();
    const button = shallow((<Button onClick={mockOnClick}>click me</Button>));
    button.find('button').simulate('click');
    expect(mockOnClick.mock.calls.length).toEqual(1);
  });
});
```

我们用创建一个模拟函数，并把它传递给我们的按钮组件。

然后我们得到按钮，用参数`'click'`调用`simulate`来模拟一次点击。

然后我们可以检查最后是否调用了`mockOnClick`函数。

或者，我们可以在 Sinon 测试中编写类似的代码。

例如，我们可以写:

```
import React from 'react';
import { shallow } from 'enzyme';
import sinon from 'sinon';import Button from './Button';describe('Test Button component', () => {
  it('simulates click events', () => {
    const mockOnClick = sinon.spy();
    const button = shallow((<Button onClick={mockOnClick}>click me</Button>)); button.find('button').simulate('click');
    expect(mockOnClick).toHaveProperty('callCount', 1);
  });
});
```

我们用`sinon.spy()`创建一个模拟函数。

然后我们将它传递给`onClick`回调函数。

然后我们用`'click'`参数调用`simulate`来模拟点击。

我们也可以直接从道具中调用我们的`onClick`函数。

例如，我们可以写:

```
wrapper.find('Button').prop('onClick')()
```

# 如何向 Redux 添加多个中间件

我们可以用`applyMiddleware`方法给 Redux 添加多个中间件。

例如，我们可以写:

```
const createStoreWithMiddleware = applyMiddleware(ReduxThunk, logger)(createStore);
```

我们只是将它们直接传递给`applyMiddleware`函数。

此外，我们可以将它们全部放入一个数组中，并将它们分散到`applyMiddleware`中:

```
const middlewares = [ReduxThunk, logger];
applyMiddleware(...middlewares)
```

# 在 React 中加载 DOM 时回调

`componentDidMount`是加载 DOM 时调用的回调函数。

例如，我们可以写:

```
class Comp1 extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoad = this.handleLoad.bind(this);
  } componentDidMount() {
    window.addEventListener('load', this.handleLoad);
  } componentWillUnmount() { 
    window.removeEventListener('load', this.handleLoad)  
  } handleLoad() {
    //...
  } render() {
    //...
  }
}
```

我们有作为`componentDidMount`中的`load`事件的监听器添加的`handleLoad`方法。

在`componentWillUnmount`中，当组件卸载时，我们移除事件监听器。

# 使用 React 正确验证输入值

为了简化表单验证，我们可以使用 react-hook-form 库。

例如，我们可以写:

```
import React from 'react'
import useForm from 'react-hook-form'function App() {
  const { register, handleSubmit, errors } = useForm() 
  const onSubmit = (data) => { console.log(data) }   return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input name="firstname" ref={register} /> <input name="lastname" ref={register({ required: true })} /> 
      {errors.lastname && 'last name is required'} { <input name="age" ref={register({ pattern: /\d+/ })} />
      {errors.age && 'invalid age.'} <input type="submit" />
    </form>
  )
}
```

我们使用包装附带的`useForm`挂钩。

然后我们可以使用钩子返回的函数和对象来检查有效的输入并显示错误消息。

`register`让我们用 react-hook-form 注册输入字段，并添加验证规则。

注册该字段将使它在我们运行提交处理程序时可用。

我们可以传递一个带有验证规则的对象，就像我们在姓氏和年龄输入中所做的那样。

我们对姓氏字段应用了`required`规则。

我们在`age`字段中为有效格式设置了一个正则表达式。

在`onSubmit`函数中，我们有`data`参数，它有填充值。

它只在所有字段都有效的情况下运行。

`errors`在某一领域有错误。

# 使用 React 路由器添加未找到的页面

我们可以通过添加一个没有路径的`Router`组件来添加一个没有找到的页面。

例如，我们可以写:

```
<Switch>
  <Route exact path="/">
    <Home />
  </Route>
  <Route path="/will-match">
    <WillMatch />
  </Route>
  <Route path="*">
     <NoMatch />
  </Route>
</Switch>
```

最后一项:

```
<Route path="*">
  <NoMatch />
</Route>
```

是当 URL 与列出的任何其他内容都不匹配时加载的通配符路由。

![](img/2b9f2ec79b5dcacd95a2d79c2c3e7285.png)

照片由 [Dieny Portinanni](https://unsplash.com/@dienyportinanni?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用 React 路由器创建不匹配的路由。

我们可以用模拟函数来模拟按钮点击。

为了简化表单验证，我们应该使用一个包。

我们可以用 React-Redux 添加多个中间件。