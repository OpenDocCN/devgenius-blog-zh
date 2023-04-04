# 用 Javascript 访问文件系统

> 原文：<https://blog.devgenius.io/access-the-filesystem-with-javascript-c36577dc0bb4?source=collection_archive---------6----------------------->

## 文件系统访问 API 的快速概述

![](img/0c1e5b0d285f4f00bae87d2dead9730d.png)

图片由 Patrick Lindenberg 通过 unsplash 提供

文件系统访问一直是 JS 开发人员的白日梦。直到今年，直到文件系统访问 API 的推出。这个 API 使 JS 开发人员能够读写主机的文件系统。在这篇文章中，我将带您了解如何读写文件系统中的数据。

# 访问目录和文件

您可以通过调用函数`window.showDirectoryPicker`来访问目录。这将打开一个选择目录的窗口。一旦选中，该函数将返回一个`Promise`。这个`Promise`将返回你的目录句柄。目录句柄是文件系统的网关。以下是演示此代码的示例函数:

```
let dirHandle;async function readConfig(){ dirHandle = await window.showDirectoryPicker();}
```

为了访问一个文件，我将调用目录句柄方法`getFileHandle`。这将返回一个文件句柄。在调用这个函数时，我将把选项`create`设置为真。这样，如果文件不存在，API 将创建该文件。为了访问实际的数据，我将从我返回的文件句柄中调用方法`getFile`。这将返回一个类型为`File`的对象。如果我打开的文件是基于文本的，我可以调用方法`text`来检索文件数据的字符串表示。如果文件为空或不存在，将返回一个空字符串。以下是执行此操作的代码:

```
const file = await dirHandle.getFileHandle("config.json", {
       create: true
})fileData = await file.getFile();let text = await fileData.text()alert(text)
```

上面的代码将从前面选择的目录中加载一个名为`config.json`的文件。现在，如何将数据写入文件系统呢？

# 写入数据

为了写数据，我将首先调用我的目录句柄上的`getFileHandle`方法。我将通过调用文件句柄方法`createWritable`来创建我的文件的可写流。写入流的数据将存储到基础文件中。下面是将 JSON 字符串写入文件的代码块:

```
const file = await dirHandle.getFileHandle("config.json", {
       create: true
})const sampleConfig = JSON.stringify({
     prop1 : true,
     prop2 : "Very cool key"
})const blob = new Blob([sampleConfig])const writableStream = await file.createWritable();// write our file
await writableStream.write(blob);// close the file and write the contents to disk.
await writableStream.close();
```

# 结论

在写这篇文章的时候，我发现了 API 的一些局限性。访问文件系统需要用户启动该过程。这意味着最终用户必须通过点击来调用访问文件系统的函数。如果不这样做，将导致引发异常。该 API 在没有 SSL 的网站上无法工作。这是出于显而易见的原因。但是，它可以通过本地主机访问网站。点击下面的链接，你可以了解更多关于文件系统访问 API 的信息。

# 其他来源:

[](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API) [## 文件系统访问 API-Web API | MDN

### 该 API 允许与用户本地设备上的文件或用户可访问的网络文件系统上的文件进行交互。核心…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API) [](https://github.com/cheikhshift/medium_examples/tree/main/jsfs) [## medium _ examples/JSF at main cheikh shift/medium _ examples

### 中型文章的代码示例。在 GitHub 上创建一个帐户，为 cheikhshift/medium_examples 开发做贡献。

github.com](https://github.com/cheikhshift/medium_examples/tree/main/jsfs)