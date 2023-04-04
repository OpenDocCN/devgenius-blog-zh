# 如何使用 react-dropzone

> 原文：<https://blog.devgenius.io/how-to-use-react-dropzone-dc615a8cae5d?source=collection_archive---------8----------------------->

![](img/9c51165c181ee6df8365f762215c403e.png)

# 目的

图像上传与拖放是非常重要的一部分，在现代发展，因为这个功能是好的 UX。当然，你可以自己创造，但这需要很多时间。所以我在项目中使用了 react-dropzone，关于如何使用我会和大家分享一下。

# 什么是 react-dropzone？

react-dropzone 是一个拖放库，用于上传图像或在项目中插入任何图像。基本上你可以用简单的钩子来使用它，你可以用样式组件，ref，react-testing-library 来测试。即使没有 JSX，你也可以使用类组件。所以这个库使用起来很灵活。

在我的情况下，我想在发送服务器之前用 react-dropzone 准备预览图像，所以我将分享如何做。

# 示例(预览)

## 步伐

1.  用创建 react 应用程序创建一个 react 项目，如下所示

```
> npx create-react-app react-dropzone //Project name is up to you
```

2.安装此库

```
> npm install --save react-dropzone
```

3.创建一个文件以使用 dropzone。在我的例子中，我想在几个地方使用这个特性，所以使用 dropzone 创建一个自定义钩子。基本上你需要用 useDropzone 使用道具，用 onDrop 赋值。这个例子是直接使用 imageList，所以如果你想只提取 src，你需要一个不同的方法(我将在后面解释)。而如果要使用打开文件对话框和选择文件来上传文件(正常的做法)，就需要使用“open”道具和“noClick: true”配合 useDropzone。除非你有“noClick: true”，否则文件对话框会被打开两次。不要忘记用 useEffect 和 return 移除副作用，否则这个函数会一直存在

4.如果你想使用自定义钩子，只使用上面的图片列表，你需要为这个文件创建额外的 css 或者从 App.css 导入(我选择了第一种方法)

5.更改应用程序文件，如下所示。拖放需要使用 getRootProps 和 getInputProps。在这种情况下，我们创建了 imageList 作为模板，这样你就可以直接注入，不需要 img 标签。

6.如下所示更改 App.css

7.这是创建基本下拉菜单所需的所有步骤，这是应用程序屏幕。当然，您可以选择文件作为正常上传。

![](img/d708455024ab063aa84e442fd6a7ecd7.png)

基本结束了。然而，如果你想把一些图像数据作为 src 发送到服务器，这种方法并不完美，因为我们已经在自定义钩子中映射了图像模板。

为了避免这种情况，除了映射，你需要从图像中提取 src

# 示例:提取 src

并更改 App.js。不要将你的上传图片类移动到 App.css，因为你返回的只是一个 src，而不是 img 标签。

在同一个页面中使用两个不同的拖放区怎么样？因为如果你只是复制你的图像容器，它是同步的。解决方案很简单，你需要有两个不同的功能和状态，如下所示。重要的部分是为第二个 dropzone 重命名 getRootProps 以区别第一个。

# 示例:两个不同的拖放区

您可以在 App.js 中使用两个不同的 dropzone，如下所示

你可以检查应用程序屏幕，你已经创建了 2 个不同的 dropzone。精彩！

![](img/3b137afe17982775e98dc93c0048f889.png)

# 结论

react-dropzone 是 react 最有用的拖放库之一。当你想使用 react-dropzone 的时候，希望这篇文章对你有所帮助。

# 参考

官方指南:[https://react-dropzone.js.org/](https://react-dropzone.js.org/)

感谢您的阅读！！