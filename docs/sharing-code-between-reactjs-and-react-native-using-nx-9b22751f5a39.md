# 使用 Nx 在 ReactJS 和 React Native 之间共享代码。

> 原文：<https://blog.devgenius.io/sharing-code-between-reactjs-and-react-native-using-nx-9b22751f5a39?source=collection_archive---------1----------------------->

Monorepo 是维护大型项目和在多个项目之间共享公共代码/逻辑的一种方便而有用的技术。

在本文中，我们将使用 Nx 工具创建 monorepo。

在 ReactJS 和 React Native 内共享代码的第一步是创建一个工作区。

创建 nx 工作空间的命令是

```
npx create-nx-workspace name-of-the-workspace
```

![](img/abb7769a408d84f6cd5eb7e50794da01.png)![](img/4b6ca8e4b3c85988edac285a66a9e479.png)

在下一步中，我们将获得一个选项来选择我们希望在工作区中使用的应用程序类型。我首先创建一个 React 本地项目，然后将创建一个 ReactJS 项目。

选择应用程序时，它会询问应用程序的名称。我将 React-native 应用程序命名为“移动”，将 ReactJS 应用程序命名为“web”。

![](img/c4ec9c24b0727bd8af9145434558a42d.png)

工作区现在创建完毕。在下图中，我们可以看到名为 monorepo-nx 的工作区，其中包含一个应用程序文件夹。我们将把 React-native 和 ReactJS 项目放在 apps 文件夹下，并将公共代码放在 libs 文件夹中。

![](img/37b930397f5294a9a23c732223bff591.png)

现在，是时候创建我们的 **ReactJS** 应用程序了。命令是

```
npx nx g @nrwl/react:app name-of-reactjs-app
```

![](img/75e447199a64d4ee197fa28afdf5db89.png)

现在，它将在我们工作区的 apps 文件夹下创建一个名为 web 的 **ReactJS** 应用程序。我们也可以使用 NX 控制台来实现这一点。

![](img/4531a78694f118c23a54fad04899a3b7.png)

为了简化使用 Nx workspace 的工作，我们可以做的下一件事是安装 Nx Console extension for Visual Studio 代码。

![](img/c5ae0d7409a1ebe108ab4bedc0fffbb0.png)![](img/9e501cc6577da6bb3904f6e894db225d.png)

使用 NX 控制台，我们可以轻松地运行、构建、服务和部署我们的代码，而无需记住确切的命令。

我们可以使用 NX 控制台轻松运行我们的 web 或移动应用程序，只需右键单击项目名称，然后从提供的选项中选择 Nx run。

![](img/4c28f9022f435c0e35092670fe20eec0.png)

我们可以选择是构建还是服务于我们的项目。在这里，我选择了 serve 来执行命令 **nx run web:serve** 。请注意，我们也可以从我们的终端运行命令 **nx run web:serve** 。

![](img/1f6d0bdb39b1d46791a85c85571be703.png)![](img/c8b0df295de121ddae0604c8dcafbccf.png)

我们的 web 应用程序现在运行在 [http://localhost:4200/。以同样的方式，我们可以运行我们的 react 本地应用程序。](http://localhost:4200/.)

现在我们将创建一个库来保存我们的公共代码。

为了创建一个库，我再次使用 NX 控制台。点击 generate，它会给我们提供一个我们可以生成的东西的列表。从这里开始，我要选择@nrwl/react — library。

![](img/6675a9ffd81f5348480086cfec10c541.png)

一旦我们选择了库选项，我们将得到一个表单来填写关于我们想要创建的库的信息，如库的名称，使用的样式，linter 等。我已经将我的库命名为 shared，并提供了导入路径 [@nx/shared](http://twitter.com/nx/shared) 。此名称( [@nx/shared](http://twitter.com/nx/shared) )将用于导入 web 和移动应用中的共享代码。

在 libs 文件夹下，我们现在可以看到我们新创建的库“共享”。我们现在将对 **shared.tsx** 进行修改，并将该代码用于 ReactJS 和 React Native app。

![](img/751ed26cbafcc0c6faaac684767529f0.png)

我选择使用 monorepo 的一个主要原因是为了在 ReactJS 和 React Native app 之间共享 API 调用代码。

我在例子中使用的是 https://jsonplaceholder.typicode.com/posts/1[的](https://jsonplaceholder.typicode.com/posts/1) api。

![](img/646194d66a841057492589cfe4437488.png)

现在，让我们创建一个 getPosts() api 调用函数来调用 https://jsonplaceholder.typicode.com/posts/1 的。一旦我们的函数准备就绪，我们现在将在 React Native 和 ReactJS 应用程序中导入该函数。

![](img/ed2575f1d983a42b8f64af900ce6ec3e.png)![](img/46d90d146a031646746f649193175e41.png)

我们可以看到，我们正在使用 import {getPosts}从“@nx/shared”导入共享库中的 getPosts 函数。

现在，如果我们运行 web 和移动应用程序，我们将在两者中看到相同的输出。

![](img/1496bdddc282027fe345cc86a5ee4bb4.png)

ReactJS 应用

![](img/cefd77e8c81fe2287ff3cb325ebf11d9.png)

React 本机应用程序

就是这样。