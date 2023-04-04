# 在 React Typescript 项目中使用代码拆分实现 React Router v6。

> 原文：<https://blog.devgenius.io/implementing-react-router-v6-with-code-splitting-in-a-react-typescript-project-14d98e2cab79?source=collection_archive---------0----------------------->

**React 路由器**是 React 中路由的标准库。它支持 React 应用程序中各种组件视图之间的导航，允许更改浏览器 URL，并使 UI 与 URL 保持同步。

**React 路由器 v5 vs v6**
React 路由器第 6 版在第 5 版的基础上引入了一些突破性的变化。React Router v6 大量使用了 [React 挂钩](https://reactjs.org/docs/hooks-intro.html)，所以在尝试升级到 React Router v6 之前，您需要安装 React 16.8 或更高版本。

还有一些变化像**开关**，**确切的说是**，**元件**关键字都被去掉了。

```
//v5
<Switch>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
</Switch>//v6
<Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
</Routes>
```

让我们深入研究代码。

1.  使用 typescript 模板创建 react 项目

```
$ npx create-react-app react-router-typescript --template typescript
```

2.安装反应路由器 dom 依赖项

```
$ cd react-router-typescript
$ npm install react-router-dom
```

3.编辑 index.tsx 文件。用 BrowserRouter 封装 App 组件

> BrowserRouter 是一个路由器实现，它使用 HTML5 历史 API(pushState、replaceState 和 popstate 事件)来保持您的 UI 与 URL 同步。它是用于存储所有其他组件的父组件。

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { BrowserRouter } from "react-router-dom";ReactDOM.render(
  <React.StrictMode>
 **<BrowserRouter>
    <App />
  </BrowserRouter>**
  </React.StrictMode>,
  document.getElementById('root')
);// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: [https://bit.ly/CRA-vitals](https://bit.ly/CRA-vitals)
reportWebVitals();
```

4.创建三个组件，Home、Topics、Settings，这三个组件将映射到我们的三个路由/、/topics 和/settings。

```
**// src/components/Home.tsx**
const Home = () => {
    return <p>Home</p>;
}
export default Home;**// src/components/Settings.tsx** const Settings = () => {
    return <p>Settings</p>;
}
export default Settings;**// src/components/Topics.tsx**
const Topics = () => {
    return <p>Topics</p>;
}
export default Topics;
```

5.创建一个组件 Main.tsx，其中包含组件的路由路径。

```
import { Routes, Route } from 'react-router-dom';import Home from './components/Home';
import Topics from './components/Topics';
import Settings from './components/Settings';const Main = () => {
return (         
    <Routes>
    <Route path='/' element={<Home/>} />
    <Route path='/topics' element={<Topics/>} />
    <Route path='/settings' element={<Settings/>} />
  </Routes>
);
}
export default Main;
```

6.编辑 App.tsx

```
import * as React from 'react';
import { Link} from 'react-router-dom';
import Main from './Main';export default function App() {
  return (
    <>  
      <div>
        <ul>
          <li><Link to='/'>Home</Link></li>
          <li><Link to='/topics'>Topics</Link></li>
          <li><Link to='/settings'>Settings</Link></li>
        </ul>
        <hr />
        <Main />       
      </div>   
    </>
  )
}
```

这是 react-router v6 的简单实现。运行应用程序。

```
$ npm start
```

**代码分割**是 webpack 最引人注目的特性之一。这个特性允许您将代码分割成不同的包，然后按需或并行加载。它可以用来实现更小的包并控制资源负载优先级，如果使用正确，可以对加载时间产生重大影响。

现在，构建上面创建的代码以查看生成的代码。我们将比较构建 js 文件和实现代码分割后的构建文件。

```
$ npm run build
$ ls build/static/js//output 
**main.1faed537.js
787.672c193d.chunk.js**
main.1faed537.js.map
787.672c193d.chunk.js.map
main.1faed537.js.LICENSE.txt
```

为了实现代码分割，我们将使用 React.lazy 和 React。悬疑成分。

**React.lazy** 接受一个参数，一个调用动态导入的函数，并返回一个常规的 React 组件。

```
const LazyHomeComponent = React.lazy(() => import(‘./Home’));
```

LazyHomeComponent 的特别之处在于，React 不会在需要时加载它，直到它被渲染。这意味着，如果我们将 React.lazy 与 React Router 结合起来，我们可以推迟加载任何组件，直到用户访问某个路径。

**做出反应。悬疑** 由于动态导入是异步的，在组件被加载、渲染和 UI 显示之前，用户需要等待一段不确定的时间。要告诉 React 显示什么，可以使用 **React。悬念**组件传递给它一个回退元素。React 的好处是。悬念是悬念可以接受多个延迟加载的组件，同时仍然只呈现一个后备元素。

7.创建一个组件 Main2.tsx，它包含我们的组件的路由路径，这些组件带有延迟加载的组件

```
import * as React from 'react';
import {Routes,Route } from 'react-router-dom'const Home = React.lazy(() => import('./components/Home'));
const Topics = React.lazy(() => import('./components/Topics'));
const Settings = React.lazy(() => import('./components/Settings'));const Loading = () => <p>Loading ...</p>;const Main2 = () => {
return (
    <React.Suspense fallback={<Loading />}>
    <Routes>
      <Route path='/' element={<Home/>} />
      <Route path='/topics' element={<Topics/>} />
      <Route path='/settings' element={<Settings/>} />
    </Routes>
  </React.Suspense>
);
}
export default Main2;
```

8.编辑 App.tsx。用 Main2 组件替换主组件。

```
import * as React from 'react';
import { Link} from 'react-router-dom';
import Main2 from './Main2';export default function App() {
  return (
    <>  
      <div>
        <ul>
          <li><Link to='/'>Home</Link></li>
          <li><Link to='/topics'>Topics</Link></li>
          <li><Link to='/settings'>Settings</Link></li>
        </ul>
        <hr />
        <Main2 />       
      </div>   
    </>
  )
}
```

就是这样。您可以运行该应用程序。

尝试构建应用程序来比较最终构建的 js 文件。

```
$ npm run build
$ ls build/static/js//outputmain.9d908c9c.js
787.672c193d.chunk.js
**444.07cb29fe.chunk.js
428.54364591.chunk.js
419.fac5c1c4.chunk.js**
main.9d908c9c.js.map
main.9d908c9c.js.LICENSE.txt
787.672c193d.chunk.js.map
444.07cb29fe.chunk.js.map
428.54364591.chunk.js.map
419.fac5c1c4.chunk.js.map
```

资源:

[https://ui.dev/react-router-code-splitting/](https://ui.dev/react-router-code-splitting/)T7[https://webpack.js.org/guides/code-splitting/](https://webpack.js.org/guides/code-splitting/)