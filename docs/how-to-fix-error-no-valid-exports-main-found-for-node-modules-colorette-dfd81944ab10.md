# 如何修复“错误:找不到“/node_modules/colorette”的有效导出主文件”

> 原文：<https://blog.devgenius.io/how-to-fix-error-no-valid-exports-main-found-for-node-modules-colorette-dfd81944ab10?source=collection_archive---------0----------------------->

![](img/6999b563aeb9879c65fc843cff3be1cd.png)

照片由[丹尼尔·伊德里](https://unsplash.com/@ricaros)在 [Unsplash](https://unsplash.com/photos/FCHlYvR5gJI) 拍摄

最近，当我在开发一个 React 应用程序时，我在尝试启动该应用程序时遇到了以下错误:

> 错误:找不到“/project-path/node _ modules/colorette”的有效导出主文件

自从我看到了`node_modules`，我本能地感觉到有些东西与我的`npm`或`node`版本相冲突，所以我运行命令来检查这些:

```
eto-tremaine$ npm -v
5.6.0eto-tremaine$ node -v
v13.3.0
```

# 解决方案

经过一番挖掘，我发现了[建议](https://github.com/facebook/create-react-app/issues/9273)到**更新到节点 14.5** 。有许多方法可以做到这一点；我选择使用[节点版本管理器](https://github.com/nvm-sh/nvm)，简称 **nvm** 。可以在 GitHub 上查看其自述文件中的[“安装和更新”](https://github.com/nvm-sh/nvm#installing-and-updating)一节进行安装。

因为我已经安装了 nvm，所以为了升级，我只运行了以下命令:

```
eto-tremaine$ nvm install 14.5
```

为了确保它能够工作，我检查了我的机器运行的 Node 的版本:

```
eto-tremaine$ node -v
v14.5.0
```

而且真的是这样！在这之后启动应用程序工作。

# 为什么会这样？

包 [colorette](https://www.npmjs.com/package/colorette) 在您的项目中，在 14 主版本以下的 Node 版本上不支持[。](https://github.com/facebook/create-react-app/issues/9273#issuecomment-658377736)

GitHub [SumitJadiya](https://github.com/SumitJadiya) 注意到，如果你不想专门升级到 Node 14.5，你可以回到不需要 Node 14+的 colorette 版本:

> 要卸载 colorette:
> npm 卸载 colorette
> 
> 要安装 colorette 版本 1.2.0:
> npm 安装 colorette@1.2.0

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)