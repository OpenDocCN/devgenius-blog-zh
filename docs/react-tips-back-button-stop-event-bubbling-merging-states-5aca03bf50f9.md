# 反应提示—后退按钮、停止事件冒泡、合并状态

> 原文：<https://blog.devgenius.io/react-tips-back-button-stop-event-bubbling-merging-states-5aca03bf50f9?source=collection_archive---------0----------------------->

![](img/f6517f0f9dd693487f7ec45e44e899dd.png)

由[paweczerwi ski](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 防止子级在父级上触发事件

我们可以通过调用`stopPropagation`方法来防止事件从子节点冒泡到父节点。

例如，我们可以写:

```
class App extends Component {
  constructor(){
    super();
  } handleParentClick(e) { 
    console.log('parent clicked');
  }, handleChildClick(e) {
    e.stopPropagation();
    console.log('child clicked');
  }, render() {
    return (
      <div>
        <p onClick={this.handleParentClick}>
          <span onClick={this.handleChildClick}>Click</span>
        </p>
      </div>
    );
  }
}
```

我们创建了一个包含父元素和子元素的组件。

span 附带有`handleChildClick`处理程序。

由于我们调用了`e.stopPropagation`，我们不会让孩子的事件冒泡到家长。

我们还有一个附加到 p 元素的`handleParentClick`处理程序。

# 在 React 中下载文件

我们可以用几种方法创建文件下载链接。

一种方法是通过编写以下代码来使用 React 路由器的`Link`组件:

```
<Link to="/files/file.pdf" target="_blank" download>Download</Link>
```

我们也可以创建一个链接并点击它来触发下载。

例如，我们可以写:

```
const link = document.createElement('a');
link.href = `file.pdf`;
document.body.appendChild(link);
link.click();
document.body.removeChild(link);
```

我们也可以通过编写以下内容来使用锚标记:

```
<a href={uploadedFileLink} target="_blank" rel="noopener noreferrer">
   download file
</a>
```

我们有`rel=”noopener noreferrer”`属性，这样我们可以防止跨来源目的地的安全问题。

此属性将阻止外部域访问当前网站。

# 在 React 路由器中拦截或处理浏览器的后退按钮

我们可以通过运行后退按钮动作的`setRouteLeaveHook` 事件来监听后退按钮动作。

例如，我们可以写:

```
import {Component} from 'react';
import {withRouter} from 'react-router';class App extends Component {
  componentDidMount() {
    this.props.router.setRouteLeaveHook(this.props.route, this.onLeave);
  } onLeave(nextState) {
    if (nextState.action === 'POP') {
      //...
    }
  } //...
}export default withRouter(App);
```

我们调用`withRouter`来注入`router`对象，它有`setRouteLeaveHook`方法让我们监听`this.props.route`的变化。

然后我们创建了自己的`onLeave`方法，作为第二个参数传入。

它采用了`nextState`对象，该对象具有我们可以检查的`action`。

如果是`'POP'`，则后退按钮被按下。

有了函数组件，我们可以监听`history`对象的变化。

`history`具有`action`属性，该属性返回正在进行的操作。

如果是`'POP'`，那么我们知道用户按了返回键。

例如，我们可以写:

```
import { useHistory } from 'react-router-dom'const App = () => {
  const { history } = useRouter();

  useEffect(() => {
    return () => {
      if (history.action === "POP") {
        //...
      }
    };
  }, [history]) //...
}
```

我们将它放在我们在`useEffect`回调中返回的函数中，这样我们就可以检查组件何时被卸载。

还有一个`history.listen`方法，让我们监听历史的变化。

例如，我们可以写:

```
import { useHistory } from 'react-router-dom'const App = () => {
  const history = useHistory(); useEffect(() => {
    return history.listen(location => {
      if (history.action === 'POP') {
        //...
      }
    })
  }, []) //...
}
```

我们可以像前面的例子一样听`'POP'`动作。

# 使用 React useState()挂钩更新和合并状态对象

为了用`useState`更新和合并状态对象，我们可以向状态改变函数传递一个回调。

它将前一阶段的值作为参数，并返回我们想要的新状态值。

例如，我们可以写:

```
setState(prevState => {
  return { ...prevState, ...newValues };
});
```

我们使用 spread 操作符来扩展新旧状态对象的所有属性。

同样，我们可以使用`Object.assign`来做同样的事情。

例如，我们可以写:

```
setState(prevState => {
  return Object.assign({}, prevState, newValues);
});
```

做同样的事情。

或者，如果我们真的不关心当前的状态值，我们可以把它合并进来。

我们可以写:

```
setState({
  ...state,
  foo: 'bar'
});
```

`state`的价值无法保证。

![](img/54be918fbf534a0e4d064c72045d790c.png)

由 [Davide Pietralunga](https://unsplash.com/@davide_pietralunga?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

有许多方法可以合并旧状态和新状态。

我们可以调用`stopPropagation`来停止传播事件。

有许多方法可以观察后退按钮的按下情况。