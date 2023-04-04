# React 代码分割、捆绑、延迟加载和暂停——初学者指南

> 原文：<https://blog.devgenius.io/react-code-splitting-bundling-lazy-loading-and-suspens-all-you-need-to-know-beginner-guide-c6e122454db6?source=collection_archive---------4----------------------->

## React 捆绑优化变得简单

![](img/674d93908075bb035da72c6869515710.png)

照片由[梅姆](https://unsplash.com/@picoftasty?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 什么是代码捆绑和拆分，为什么需要它？

## **代码捆绑**

是将应用程序的所有文件捆绑成一个文件(捆绑包)的过程，在该文件中，应用程序可以立即提供给客户端。

## **代码拆分**

是将包/构建文件分割成更小的块，根据需要提供给客户机的过程。

## **我们为什么需要拆分？**

基本上是因为我们不想给客户提供他们可能永远不需要或者在以后阶段(不是在最初的几毫秒内)需要的代码。

# React 代码捆绑…和拆分

绑定和拆分的概念存在于 React 中，事实上 React 文档专门讲述了如何优化 React 应用程序以支持代码拆分。

React 生态系统本身有一些很酷的开箱即用的解决方案，它们也利用了这些概念，像 [CRA](https://github.com/facebook/create-react-app) 、[下一个](https://nextjs.org/)和[盖茨比](https://www.gatsbyjs.com/)这样的工具可以做最少的工作，以非常少的配置获得应用程序的优化构建文件。

## React 代码分割是如何工作的

要回答这个问题，我们需要对一些著名而神秘的工具有一个总体的了解:
1-[**web pack**](https://webpack.js.org/guides/code-splitting/#prefetchingpreloading-modules):一个捆绑工具，做一些很酷的事情，其中一个就是根据一些配置捆绑和拆分你的代码。
2- [**巴别塔**](https://babeljs.io/docs/en/) :一个移植工具，非常适合像 ECMS - > CJS 这样的事情转换。-阅读更多关于它的内容-
3-[**roll up**](https://rollupjs.org/guide/en/#overview):一个工具，它能做一些令人惊奇的事情，其中之一就是将 JS 模块(ECMS)移植到 commonJS (CJS)。

Webpack 是一个到处都在使用的著名工具，所以我们将继续这篇文章，深入研究它的工作原理以及如何使用 webpack 优化我们的应用程序。

现在，如果您正在使用 create-react-app 或 NextJs，您可能不需要在 webpack 或任何其他捆绑工具中进行任何直接配置，因为这些工具已经预先设置了完成工作的 webpack——尽管您可能希望查看这些工具的文档以了解如何扩展 web pack 的这些预先配置的设置。但是，如果您没有使用上面提到的任何工具，那么您应该从以下几点开始:

## Webpack 自己会变魔术

您所需要做的就是指定一个入口点和输出目录，您的文件应该被打包到这个目录中。以下是 webpack 文件的一个片段:

```
const path = require('path');
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

所以这将把`index.js` 和它所有的依赖项捆绑到一个名为`bundle.js`的文件中，并把它放在一个名为`root-folder/dist`的文件夹中

现在和我一起想一想，如果我们的应用程序很大呢？捆绑会超级大对吧？那又怎样？
一个巨大的包文件意味着更慢的 TTFB(到达第一个字节的时间),这意味着用户将在页面上显示任何内容之前等待一段时间。

我们肯定可以通过 webpack 进行优化，我们有(2)个推荐选项:

## Webpack 动态导入:

只提供用户访问页面时第一次看到的内容，其余的内容可以在以后提供(或者按需提供，或者通过并行加载)。这被称为动态导入，顾名思义，它使用异步导入机制，将每个异步导入的代码拆分到自己单独的文件中。让我们看一个例子:

```
async function getComponent() {
  const element = document.createElement('div');
  const { default: _ } = await import('lodash');

   const element = document.createElement('div');
   element.innerHTML = _.join(['Hello', 'webpack'], ' ');

   return element;
}(async () => {
  const Component = await getComponent();
  ReactDOM.render( <Component />,document.getElementById('some-id'))
})()
```

让我们假设您有一个使用`lodash`的组件，我们想单独加载该组件及其依赖关系(`lodash`)，所以我们构建了一个动态导入`lodash`的函数，它将告诉 webpack 将`lodash’s`代码拆分到它自己的文件中。这将使我们的组件包文件变得更小，从而在客户端花费更少的时间来加载。-如果您不熟悉 async/await 语法，您可以在这里阅读更多信息-

## Webpack 共享模块:

如果我们有 3 个页面使用了名为`moment`的第三方库，我们想为每个页面制作 3 个包

```
const path = require('path');
module.exports = {
  entry: {
   firstPage: ./src/firstPage.js, 
   secondPage: ./src/secondPage.js
   thirdPage: ./src/thirdPage.js
  }
  ...
};
```

当 Webpack 运行文件并分别捆绑每个页面时，它将遍历每个页面(定义为入口点)并捆绑其代码和它使用的任何依赖项)这意味着`moment`将与 3 个页面捆绑在一起，换句话说，它将被复制 3 次。我们可以增强上面的代码片段，将`moment`分割成它自己的块，并在每个文件中都需要它。

```
const path = require('path');
module.exports = {
 firstPage: {
  import: './src/firstPage.js',
  dependOn: 'shared',
 },
 secondPage: {
  import: './src/secondPage.js',
  dependOn: 'shared',
 },
 thirdPage: {
  import: './src/thirdPage.js',
  dependOn: 'shared',
 }, shared: 'lodash',
};
```

[阅读更多关于预取/预加载模块的信息](http://Prefetching/Preloading modules)

# 什么是懒装？

还记得我们的动态导入示例吗？我们基本上是在稍后阶段加载`lodash`，但是这里要注意的一点是，加载`lodash`不需要用户交互，这意味着每次页面刷新时，我们都会再次调用获取`lodash`，而不管组件是否已经在页面上呈现(装载)，这并不是真正的最佳选择。

惰性加载将拆分带到了下一个层次，它只在组件(代码脚本)执行时获取所需的块。有一些利用 webpack 本身的本地方法可以做到这一点，但是因为这篇文章是关于 react 的，所以让我们来看看 React 的方法。

# `React.Lazy() and React.suspens`

React 有它的`lazy()`功能，可以让我们动态加载任何组件，但是只有当它的调用程序/导入程序第一次挂载时。让我们看看下面的例子

```
const Header = () => <div>Header</div>
const Button = () => <button>Button</button>const Body = ({children}) => (
 <div>
  ...
  {children}
 </div>
);---------------------------------------------------import Header from './Header';
import Body from './Header';
import Button from './Button';const App = () =>(
 <div>
  <Header/>
  <Body>
   </Button>
  </Body>
 </div>
);
```

在这个例子中，我们将`Header`和`Body` 组件导入到我们的应用程序中，`Body`组件导入并使用另一个组件`Button` 。当我们捆绑我们的应用程序时，这两个组件将在一个捆绑包中，并将立即加载。

怎样才能懒加载`Button`？换句话说，我们怎样才能让组件只在`Body`第一次挂载/渲染后加载。

我们简单地使用`React.Lazy()`——如果我们在客户端提供应用程序。
*注意:您可能需要一些 babel 配置来转换导入语法

```
import Header from './Header';
import Body from './Header';
// import Button from './Button';
const Button = React.lazy(() => import('./Button'));const App = () =>(
 <div>
  <Header/>
  <Body>
   </Button>
  </Body>
 </div>
);
```

如果您正在加载您的组件服务器端呢？看看[@可加载](https://github.com/gregberge/loadable-components)

## 什么是 React 悬疑，用来做什么？

要回答这个问题，让我们回头看看上面的惰性加载示例，在加载`Button`组件时会向用户显示什么？基本没什么！对吗？嗯`suspens`是为了确保用户看到一些东西，表明一些东西正在被加载。点击阅读更多关于如何使用[的信息。](https://github.com/gregberge/loadable-components)

使用`suspens`的一个很好的用例是基于路径分块/分割代码，或者构建一个组件，它的 UI 严重依赖于从 API 获取的数据。这样`suspens`可以同时显示装载骨架。

# 总结和奖金

分裂或分块这个术语听起来可能有些挑衅，但它绝对是一个方便且非常强大的概念，使得 web 应用程序变得更加轻便和易于呈现。然而，我们需要确保我们均匀地分割块，并避免以任何方式中断用户体验。还有很多其他工具和技术可以用来实现公平的拆分工作，比如 [Split.io](https://www.split.io/) 和 [Rollup.js](https://rollupjs.org/guide/en/#overview) 。这些工具可能适用于更复杂的应用程序。