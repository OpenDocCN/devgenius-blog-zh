# SwiftUI 中的嵌入式 YouTube 视频

> 原文：<https://blog.devgenius.io/embedded-youtube-videos-in-swiftui-900d3a0e80d4?source=collection_archive---------1----------------------->

## 如何用 SwiftUI 嵌入 YouTube 视频

![](img/593ec404c8d6cc0715f61e84d06987cc.png)

克里斯蒂安·威迪格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

要在 SwiftUI 中嵌入 YouTube 视频，首先我们必须创建一个 WebView。为此，您可以遵循本文:

[](https://marcelojosel15.medium.com/webviews-in-swiftui-d5b1229e37ba) [## SwiftUI 中的网络视图

### 如何在 SwiftUI 中制作 WebView

marcelojosel15.medium.com](https://marcelojosel15.medium.com/webviews-in-swiftui-d5b1229e37ba) 

现在我们已经通过 SwiftUI 文章中的**网络视图获得了这个网络视图，我们有了以下内容:**

只需用你的 YouTubeURL 替换 **WebViewModel** init 中的 URL，就这样，你就有了一个嵌入在应用程序中的 YouTube 视频。

现在，如果你想全屏打开 YouTube 视频，只需替换出现的**手表？** for **watch_popup？**如下:

**返回**URL . replacing occurrences(of:" watch？，带有:“watch_popup？”)

就这样，使用 SwiftUI，您的应用程序中就有了您的 YouTube 视频。

与我们合作:

[](https://www.avilatek.com/en/) [## 阿维拉泰克

### 技术创新的发展

www.avilatek.com](https://www.avilatek.com/en/) 

我的 Linkedin 个人资料:

 [## Marcelo Laprea -软件工程师- UXSmobile | LinkedIn

### 经验丰富的 iOS 应用程序开发人员，有在计算机软件行业工作的经历。熟练于…

www.linkedin.com](https://www.linkedin.com/in/marcelo-laprea/)