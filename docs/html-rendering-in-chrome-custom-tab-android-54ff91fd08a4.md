# chrome 自定义标签中的 HTML 呈现(Android)

> 原文：<https://blog.devgenius.io/html-rendering-in-chrome-custom-tab-android-54ff91fd08a4?source=collection_archive---------1----------------------->

![](img/6e2bdfcf7ab5e858d3a5a45af5c493e9.png)

> 这是在 chrome 自定义标签或任何其他 web 视图库中打开自定义 html 页面的一种变通方法。

## 介绍

android 中的 Chrome 自定义标签是一个库，它可以打开本地应用程序中的任何 url。它给人的感觉类似于应用程序主题的网页。此外，它还帮助我们以比原生 webview 快 4 倍的速度呈现网页，因为它使用了 chrome 浏览器的缓存机制。这都是关于打开网址。但是如何在 chrome 自定义标签中呈现 html 呢？

## 目标

我们将在 chrome 自定义标签中呈现我们的本地自定义 html 脚本。

## 我们开始吧

> 第一步:chrome 自定义标签的 SDK 集成。

```
implementation 'androidx.browser:browser:1.2.0'
```

要了解更多关于库和 sdk 集成的信息， [*点击这里*](https://developer.chrome.com/multidevice/android/customtabs) *。*

> 步骤 2:创建一个定制的 html 页面。

我们使用这个 html 只是为了演示。我们可以按照我们的要求创建任何 html。并将该文件放在 android 的 asset 文件夹中。

> 步骤 3:现在让我们有一些实际的代码。

因为 chrome 自定义标签只支持 url，不支持原始 html。所以我们需要将 html 文件转换成某种 url。通过下面编写的方法，我们将获得一个存储在外部缓存目录中的文件的 url。

> 第四步:打开 chrome 自定义的 URL。

***恭喜恭喜！我们做到了。***