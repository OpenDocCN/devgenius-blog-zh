# 反应自然—使您的导入易于编写

> 原文：<https://blog.devgenius.io/react-native-make-your-imports-easy-to-write-bcb13c0b6c7e?source=collection_archive---------1----------------------->

不要使用导入../../../

![](img/d06dff105506511f243982d44ffa1ab7.png)

React Native-React Native 中的简单导入

这是一篇关于让你的文件导入在 React Native 中易于编写的简单文章！

这在有很多嵌套文件夹的项目中非常有用。

# 你写**不累吗../../?**

## 1.“嵌套方式”

假设你在一个你想要导入的文件夹外面，这是导入文件的例子。

```
// Components
import SomeNestedComponent from '../../../components/Component'
import JustPlaceMeInTheRoot from '../.../../../../screens/Screen'
```

## **2。更好的方法**

你要让你的进口看起来像这样

```
// Components
import ComponentOne from '@components/ComponentOne'
import SomeScreen from '@screens/SomeScreen'
```

有意思？让我们滚动！

# 让我们在您的 React Native 中实现这个漂亮的导入

首先，非常感谢**巴别插件模块解析器！**

> 我假设你安装了 React Native 并初始化了项目，不管你使用 Expo 还是 CLI。

## 1.安装[巴别插件模块解析器](https://github.com/tleunen/babel-plugin-module-resolver)

要像上面的第二种方法一样进行漂亮的导入，您必须安装一个名为**babel-plugin-module-resolver**的包。

转到 React 本地项目文件夹，安装 NPM 或 Yarn。

```
// Using NPM
npm install --save-dev babel-plugin-module-resolver// Using Yarn
yarn add --dev babel-plugin-module-resolver
```

## 2.编写. babelrc 文件

在项目中安装了这个包之后，转到项目文件夹的根目录，创建一个名为**的文件。babelrc** 。并将这段代码粘贴到下面。

。babelrc 为导入定义您的别名。

之后，运行你的 React 原生应用，开始做这个方法！

```
// Components
import YourComponent from '@components/YourComponent'
```

> **注意:如果您之前已经运行了 React Native，您必须关闭当前运行的 React Native 并重新运行。此外，如果您在 React Native 运行时创建一个新别名，您也必须重新运行 React Native。**

你可以在文档中看到这个包的完整用法。这是它的链接。

[](https://github.com/tleunen/babel-plugin-module-resolver) [## tle unen/babel-插件-模块-解析器

### 一个巴别塔插件，当使用巴别塔编译你的代码时，为你的模块添加一个新的解析器。这个插件允许你…

github.com](https://github.com/tleunen/babel-plugin-module-resolver) 

# 结论

我猜你再也不会编写嵌套导入了。希望这篇文章有帮助。

这使得你的项目看起来更好。

说到**更好，**我正在写一篇关于**反应**的文章，是关于如何更好地管理你的状态！下面是它的链接！ **ReactJS** ？不用担心，可以在你的 React 原生应用中实现！

[](https://medium.com/dev-genius/reactjs-manage-your-state-nicely-with-context-1ed3090a6a46) [## ReactJS——利用上下文很好地管理您的状态

### 用一种反应的方式表达你的状态

medium.com](https://medium.com/dev-genius/reactjs-manage-your-state-nicely-with-context-1ed3090a6a46) 

希望你喜欢这篇文章，如果你喜欢，请在离开前留下掌声，如果你想讨论，请分享讨论。

下一篇文章再见！