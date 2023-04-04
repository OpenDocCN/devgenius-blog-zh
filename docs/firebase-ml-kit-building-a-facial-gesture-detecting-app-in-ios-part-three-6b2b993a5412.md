# Firebase ML 工具包:在 iOS 中构建面部姿态检测应用程序(第三部分)

> 原文：<https://blog.devgenius.io/firebase-ml-kit-building-a-facial-gesture-detecting-app-in-ios-part-three-6b2b993a5412?source=collection_archive---------16----------------------->

本文展示了如何借助 [Firebase ML Kit 人脸检测](https://firebase.google.com/docs/ml-kit/detect-faces) API 检测不同的面部表情(点头、眨眼、微笑等)。在这里，我们将主要关注 Firebase ML Kit Vision API 的用例，以检测不同的面部手势。对于该项目的源代码模板，您可以访问本系列的第二部分[**。**](https://medium.com/p/2f5322906d1d/edit)

> **检测眨眼(双眼、左眼、右眼、笑脸)**

为此我使用了 JGProgressHUD 作为第三方的 Cocoapods 库来显示眨眼的表情符号。

> **检测头点头(右&左)**

为此我创建了一个[**UIPageViewController**](https://developer.apple.com/documentation/uikit/uipageviewcontroller)用于水平改变页面。

# 演示

# 结论

本文研究了人脸检测 API 的不同用例。您可以利用这些模板来设计自己的面部手势应用程序或使用 Firebase ML Kit Vision API 的无触摸应用程序。

本文的完整项目可以在这里找到[](https://github.com/paloamit/FaceGestureDemo/commits/FaceGestureUseCase)****，并且不要忘记在构建项目之前运行*“pod install”*命令。****

****如果你想让我实现任何与人脸检测相关的特定用例，你可以在评论区告诉我。****

****如果你觉得这些有用，请随意分享。感谢阅读！****

****希望你喜欢使用 Firebase ML 工具包学习**人脸检测 API。******

****![](img/ddab51b8952d57baffcba86fc9ffc793.png)****