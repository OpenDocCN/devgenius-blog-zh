# SwiftUI 中的网络视图

> 原文：<https://blog.devgenius.io/webviews-in-swiftui-d5b1229e37ba?source=collection_archive---------0----------------------->

## 如何在 SwiftUI 中制作 WebView

![](img/2a35e0b361eed80addf57e25ea5c80c0.png)

米娅·贝克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了在 SwiftUI 中使用 WebView，我们必须构建一个 UIViewRepresentable。让我们开始吧。

让我们首先创建一个 ObservableObject，这样我们的视图和 UIViewRepresentable 就可以通信了。

*   **URL** :我们想要在 WebView 中显示的路径。
*   **isLoading** :了解 webView 何时完成加载，以便我们可以隐藏/显示加载器指示器。
*   **可返回**:了解 webView 何时导航到另一个网页，并可返回。
*   **shouldGoBack** :通知 webView，我们要返回到上一个网页。

创建完这个之后，现在我们继续创建 UIViewRepresentable

*   makeCoordinator() :这里我们正在初始化协调器(我们将马上解释如何创建它)
*   **makeUIView()** :这里我们创建 WKWebView 并加载 URL 请求。
*   **updateUIView()** :这里我们将在 webViewModel 发生变化时更新 UI，在这种情况下，如果用户点击返回按钮，我们将告诉 WKWebView 返回()。

现在我们必须创建协调器，这样我们就可以使用 WKNavigationDelegate 方法。我们将创建它作为 WebViewContainer 的扩展:

这里我们实现了 **WKNavigationDelegate** 方法。

现在，如何在 SwiftUI 视图中实现这个 WebViewContainer？让我们这样做:

如您所见，在 SwiftUI 中实现 WebView 并不困难。现在你可以创建你自己的，并添加你想要的功能。

我用作参考的一些文章是:

[](https://medium.com/macoclock/how-to-use-webkit-webview-in-swiftui-4b944d04190a) [## 如何在 SwiftUI 中使用 WebKit WebView

### 使用 SwiftUI 在 Xcode 11.4 上开发

medium.com](https://medium.com/macoclock/how-to-use-webkit-webview-in-swiftui-4b944d04190a) [](https://medium.com/@mdyamin/swiftui-mastering-webview-5790e686833e) [## SwiftUI:掌握 WebView

### 将 web 应用加载到 SwiftUI 视图以及 iOS 原生应用和 web 应用之间接口的完美方式。

medium.com](https://medium.com/@mdyamin/swiftui-mastering-webview-5790e686833e) 

与我们合作:

[](https://www.avilatek.com/en/) [## 阿维拉泰克

### 技术创新的发展

www.avilatek.com](https://www.avilatek.com/en/) 

我的 Linkedin 个人资料:

 [## Marcelo Laprea - iOS 软件工程师| LinkedIn

### 经验丰富的 iOS 应用程序开发人员，有在计算机软件行业工作的经历。熟练于…

www.linkedin.com](https://www.linkedin.com/in/marcelo-laprea/)