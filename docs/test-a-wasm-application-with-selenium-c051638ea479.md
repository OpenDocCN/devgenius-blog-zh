# 用 Selenium 自动化测试 WASM 应用程序。

> 原文：<https://blog.devgenius.io/test-a-wasm-application-with-selenium-c051638ea479?source=collection_archive---------9----------------------->

## 因为单元测试是不够的…

![](img/67c8aaeaf4f607f492c0ce023801c6dc.png)

图片由 [Francesco](https://unsplash.com/@detpho) 通过 unsplash 提供

集成测试被定义为一种测试类型，其中软件模块被逻辑地集成并作为一个组进行测试。在这篇文章中，我将描述我用 WASM 二进制执行集成测试的方法。我将使用包`github.com/tebeka/selenium`与 Selenium 交互。Selenium 是一个“自动化浏览器”的工具它创建了一个可以加载真实网页的虚拟浏览器。在这篇文章中，这将是我执行 WASM 二进制的途径。在开始之前，这里有一个需求清单(如果你正在跟进的话) :

*   `xvfb`，这是一个 X 虚拟帧缓冲区。x 是 linux 的窗口管理系统。
*   Java，你知道什么是 Java。

# 该项目

对于这篇文章，我将从创建一个空目录开始。我将在目录中添加两个文件。第一档将是`main.go`。我将在文件中添加一个名为`Sample`的函数。该函数将在 id 为`output`的网页上找到一个元素，并将该元素的文本设置为`Hello World`。以下是该文件的定义:

第二个文件将称为`index.html`。这个网页将有一个 id 为`output`的元素。它也将有一个 id 为`action`的按钮，点击时，这个按钮将执行我的 WASM 二进制函数。以下是该文件的代码:

为了构建这个项目，我运行了以下命令:

```
GOOS=js GOARCH=wasm go build -o lib.wasm
```

现在我有了自己的项目，是时候配置 Selenium 了。

# 运行硒

本节专门用`github.com/tebeka/selenium`推出 Selenium。为了简单起见，我将这个 go 包称为`selenium`。我在之前创建的新文件夹中创建了一个名为`drivers`的文件夹。从 go 包`selenium`中，我复制了一个文件([https://github . com/te beka/selenium/blob/master/vendor/init . go](https://github.com/tebeka/selenium/blob/master/vendor/init.go))到 drivers 文件夹中。复制完成后，我运行文件`init.go`来下载额外的 selenium 依赖项。我使用以下标志运行该文件:

```
$ cd drivers
$ go run init.go --alsologtostderr  --download_browsers --download_latest
$ cd ../
```

# 编写测试

下载完成后，我开始编写我的集成测试。因为启动 Selenium 相当复杂，所以我专门创建了一个函数来设置我的测试的所有需求。该函数的定义如下:

该函数将首先初始化 Go 服务器。我实现了`FileServer`处理程序来服务文件`index.html`，以及我的 WASM 二进制文件。文件服务器在 Goroutine 上启动，因此功能`setup`的执行不会被阻止。这就是 Selenium 能够导航到包含我想要测试的 WASM 二进制文件的网页的方式。下面几行将启动一个 selenium 实例并打开一个新的 remote。需要注意的是，该函数返回三个变量。这是因为我希望能够通过关闭测试期间启动的所有服务器来进行适当的清理。下一步自然是编写实际的测试。去打包`selenium provides`一个与 DOM 交互的 API。在这个测试中，我点击了 id 为`action`的按钮。之后，我检查 div 元素是否已经正确更新。下面是这个测试的代码:

开始的那组`defer`调用会在测试结束后清理。我将使用以下命令运行测试:

```
go test ./integration_test.go
```

由于`syscall/js`不存在于`WASM`架构之外，我单独运行了测试文件。

# 结论

对于这种情况，静态分析不会捕捉任何 DOM 行为。这就是集成测试有用的原因。假设我在函数`Sample`中输入了错误，我写了错误的元素 ID，这个错误在我打开网页之前不会被注意到。这也是捕捉新浏览器版本产生的 bug 的好方法。也许你使用的一个 Javascript 函数被否决了，这是一个自动发现它的方法。我是在 Linux 机器上写的这篇文章，所以我不确定 Selenium 将如何为其他操作系统设置。你可以找到这个包以及这篇文章的源代码。

如果你在市场上寻找一个新的代码编辑器，这里有一个指导视频，展示了启动和调试一个项目是多么容易(3 分钟长) :

# 其他来源

[](https://www.guru99.com/integration-testing.html) [## 集成测试:什么是，类型，自顶向下和自底向上的例子

### 集成测试侧重于检查这些模块之间的数据通信。因此它也被称为“I & T”

www.guru99.com](https://www.guru99.com/integration-testing.html)  [## medium _ examples/wasm-在主 cheikhshift/medium_examples 进行测试

### 中型文章的代码示例。在 GitHub 上创建一个帐户，为 cheikhshift/medium_examples 开发做贡献。

github.com](https://github.com/cheikhshift/medium_examples/tree/main/wasm-test)  [## GitHub-te beka/Selenium:Selenium/web driver 客户端 for Go

### 这是一个用于 Go 的 WebDriver 客户端。它支持 WebDriver 协议，并且已经过各种版本的测试…

github.com](https://github.com/tebeka/selenium)