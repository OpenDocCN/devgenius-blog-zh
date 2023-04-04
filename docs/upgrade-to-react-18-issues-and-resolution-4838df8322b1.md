# 升级以作出反应 18-问题和解决方案

> 原文：<https://blog.devgenius.io/upgrade-to-react-18-issues-and-resolution-4838df8322b1?source=collection_archive---------0----------------------->

![](img/8a3089e9c4fc000814d7b6e383f6cf6d.png)

reactor 18 已经过时，大多数人都希望升级到 reactor 18，以便通过在承诺范围内自动批处理来利用性能改进，并通过并发 reactor 获得更好的用户界面体验。在并发 reactor 中，渲染可能会因用户输入等紧急更新而中断，并且会同时准备多个版本的用户界面。您可以从这里获得更多信息—[https://reactjs.org/blog/2022/03/29/react-v18.html](https://reactjs.org/blog/2022/03/29/react-v18.html)。作为使用所有这些功能的第一步，我们需要先升级到 React 18。以下是我根据[https://react js . org/blog/2022/03/08/react-18-upgrade-guide . html](https://reactjs.org/blog/2022/03/08/react-18-upgrade-guide.html)对我的 react/redux 应用程序执行的步骤。反应路由器 dom 问题没有记录在那里，所以我想分享我的经验。

1.  安装最新版本的 React:

```
npm install react react-dom
```

2.移动到新的根应用编程接口。

```
// Beforeimport { render } from 'react-dom';
const container = document.getElementById('app');
render(
  <React.StrictMode>
   <Provider store={store}>
    <MyApp />
   </Provider>
  </React.StrictMode>, 
container); // Afterimport { createRoot } from 'react-dom/client';
const container = document.getElementById('app');
const root = createRoot(container); 
root.render(
  <React.StrictMode>
   <Provider store={store}>
    <MyApp />
   </Provider>
  </React.StrictMode>
);
```

3.我没有任何 unmountComponentAtNode 的用法，也没有来自 render 的回调。我的应用程序也没有使用服务器端呈现或类型脚本。如果您使用其中任何一种，可以按照此处的说明进行操作—[https://reactjs.org/blog/2022/03/08/react-18-upgrade](https://reactjs.org/blog/2022/03/08/react-18-upgrade-guide.html#updates-to-client-rendering-apis)

4.升级反应-redux 到 8，因为 redux 8 支持反应 18。在版本 8 中，React Redux 的两个公共 API(`connect`和`useSelector`)被重写以支持 React 18。

```
npm install react-redux
```

5.升级反应脚本到 5.0.1。反应脚本 5.0.1 提高了与反应 18 的兼容性。

```
npm install react-scripts
```

如果您以前使用过 react-scripts 4，您可能会看到 webpack5 的一些问题，因为 react-scripts 5 使用 webpack5。就我而言，我有两个问题。

I .反应-脚本开始显示来自内部使用的一个节点模块的“无法解析源映射”警告。使用反应脚本时，将 GENERATE_SOURCEMAP 设置为 false。

```
GENERATE_SOURCEMAP=false react-scripts start
```

ii .我用 [msw](https://www.npmjs.com/package/msw) 作为 API 的模拟库。MSW 对 webpack 5 有意见。相关链接—[https://github.com/mswjs/msw/issues/1142](https://github.com/mswjs/msw/issues/1142)[https://github.com/mswjs/msw-storybook-addon/issues/47](https://github.com/mswjs/msw-storybook-addon/issues/47)。使用以下技术解决—[https://github.com/mswjs/msw-storybook-addon/pull/53](https://github.com/mswjs/msw-storybook-addon/pull/53)

# 问题和决议

1.  经过上述更改后，我的应用程序呈现了空白页面。

## 分辨率:

做出反应。StrictMode 组件我的 src/index.js 文件中 Provider 组件的子/后代。

React 18—[https://React js . org/blog/2022/03/08/React-18-upgrade-guide . html](https://reactjs.org/blog/2022/03/08/react-18-upgrade-guide.html#updates-to-strict-mode)中对 StrictMode 进行了一些重大更新，所以我必须将 strict mode 下移一级才能让它工作。

2.之后一切看起来都很好，但是我意识到 BrowserRouter 中的 Link 标记只改变了 URL，而没有呈现组件。react-router-dom@5 和 React 18 之间目前存在不兼容问题。

## 分辨率:

为此，我尝试了两种解决方案。

1.做出反应。StrictMode 组件 BrowserRouter 组件的子/后代。

这很有效，尽管我喜欢下一个更干净的解决方案。

2.升级到 react-router-dom@6。我用的是 react-router-dom@5.3

下面是我升级到 react-router-dom@6 的步骤。

1.  安装最新的 react-router-dom 6

```
npm install react-router-dom
```

2.将所有<switch>元素更新为<routes>，并将所有路线元素更新为具有元素属性而非组件。</routes></switch>

```
// Before<BrowserRouter>
   <Switch>
      <Route exact path="/" component=Home />
      <Route path="/users" component=Users />
   </Switch>
</BrowserRouter> // After<BrowserRouter>
   <Routes>
      <Route exact path="/" element=<Home /> />
      <Route path="/users" element=<Users /> />
   </Routes>
</BrowserRouter>
```

3.用<navigate>替换所有的<redirect>标签，如果<redirect>存在于<switch>内部，将`<Redirect>`移动到`<Route render>`道具中。</switch></redirect></redirect></navigate>

```
// Before<Switch>
  <Redirect from="about" to="about-us" />
</Switch> // After<Switch>
  <Route path="about" render={() => <Navigate to="about-us" />} />
</Switch>
```

4.用带钩路由器替换任何路由器。

```
// Beforeimport { withRouter } from 'react-router';

const ShowTheLocation = ({ match, location, history }) => {
    return <div>You are now at {location.pathname}</div>;
}

const ShowTheLocationWithRouter = withRouter(ShowTheLocation); // Afterimport { useLocation } from 'react-router-dom';const ShowTheLocation = () => {
  const location = useLocation();

  return <div>You are now at {location.pathname}</div>;
}
```

经过这些修改后，我的应用程序看起来不错。希望以上几点有帮助。计划很快开始使用并发渲染特性，将在我的下一篇文章中分享我的经验。快乐升级到 React 8！