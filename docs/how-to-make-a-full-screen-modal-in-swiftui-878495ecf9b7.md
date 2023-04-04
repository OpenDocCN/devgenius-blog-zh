# 如何在 SwiftUI 中制作全屏模态

> 原文：<https://blog.devgenius.io/how-to-make-a-full-screen-modal-in-swiftui-878495ecf9b7?source=collection_archive---------1----------------------->

iOS 13 版 SwiftUI 中的全屏模式

![](img/0416ca78230b3c1ce9a2ed99129ee428.png)

卡里·谢伊在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我被要求在我的一个项目中建立一个全屏模型，而我作为 SwiftUI 的新手，不知道如何去做。在做一些研究时，我发现了一种在 SwiftUI 中创建模态的方法，它使用了以下代码:

这很好，但它不是我想要的全屏。于是我在保罗·哈德森的网站[https://www . hackingwithswith swift . com/quick-start/swift ui/how-to-present-a-full-screen-model-view-using-fullscreencover](https://www.hackingwithswift.com/quick-start/swiftui/how-to-present-a-full-screen-modal-view-using-fullscreencover)上找到了这个，他使用 full screen cover 的方式如下:

这正是我想要的，但它只在 iOS 14 中可用，我需要它在 iOS 13 中也能工作。所以我需要找到另一种方法。

然后我发现了 Brian Voong 的一个视频，它解释了如何做到这一点。[https://www.youtube.com/watch?v=514A7yzznJE](https://www.youtube.com/watch?v=514A7yzznJE)

他基本上所做的，就是利用视图的偏移来进行模态展示。所以我做的是构建一个接受任何视图的 ModalBaseView，来模拟他的方法:

所以，稍微解释一下代码，我们在 **ModalBase** 中有一个带有绑定变量 **showModal** 和 **Content** 的视图，它可以是任何视图。如果 showModal 为真，视图的偏移量将为 0，所以会被呈现，如果 showModal 为假，偏移量将为屏幕的大小，所以会消失。

现在，在我们想要呈现的 **ModalView** 中，我们需要放一个回调函数，并在我们想要关闭或关闭视图时调用它。

最后，在**视图**(在这种情况下，是**内容视图)**中，我们要呈现**模式视图，**我们要把所有的内容放在一个 ZStack 中，最后，我们将添加我们的**模式库**。

这将使我们想要的全屏模式呈现，至少如果我们不能使用仅在 iOS 14 上可用的全屏覆盖。

如果你有其他方法，请分享给我，这样我可以改进这段代码，并学习一种新方法来制作这种模态表示。

与我们合作:

[](https://www.avilatek.com/en/) [## 阿维拉泰克

### 技术创新的发展

www.avilatek.com](https://www.avilatek.com/en/) 

领英:[https://www.linkedin.com/in/marcelo-laprea/](https://www.linkedin.com/in/marcelo-laprea/)