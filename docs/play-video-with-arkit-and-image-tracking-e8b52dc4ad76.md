# 使用 ARKit 和图像跟踪播放视频

> 原文：<https://blog.devgenius.io/play-video-with-arkit-and-image-tracking-e8b52dc4ad76?source=collection_archive---------1----------------------->

[ARKit](https://developer.apple.com/arkit/) 是苹果的一个框架，负责处理为 iOS 设备构建的增强现实应用和游戏。这是一个高级 API，提供了众多强大的功能，让一个神奇的世界变得栩栩如生。

![](img/ccf8849ae52ae6b3a8e148007c58267e.png)

图片来源:developer.apple.com

# **层层 ARKit**

ARKit 可以大致分为 3 层。它们如下:

## 世界追踪

[世界追踪](https://developer.apple.com/documentation/arkit/world_tracking)是 ARKit 最关键的功能。它允许我们跟踪表面、图像、物体、人，甚至我们的脸。

## 场景理解

场景理解是指分析现实世界的场景、垂直和水平平面、物体和其他信息。有了这种理解，我们就可以将虚拟物体放入现实世界。ARKit 提供了 3 种场景理解:平面检测，点击测试和光照估计。

## 翻译

ARKit 使用不同的技术来处理 3D 模型并将其呈现给场景，例如:

*   [scene kit](https://developer.apple.com/documentation/scenekit)
*   [*现实工具包*](https://developer.apple.com/documentation/realitykit)
*   [*金属*](https://developer.apple.com/metal/)
*   [*SpriteKit*](https://developer.apple.com/spritekit/)

# 先决条件

ARKit 最低限度需要 **A9** 处理器硬件和 **iOS 11** 软件，另外还需要一个真实的摄像头。由于这个原因，我们不能用 Xcode 模拟器运行我们的项目。

在本教程中，我们将主要关注[图像跟踪](https://developer.apple.com/documentation/arkit/arimagetrackingconfiguration)，然后在图像上叠加一个要播放的视频。那么，我们开始吧。

# 辅导的

在我们开始这个教程之前，我们应该知道`[ARImageTrackingConfiguration](https://developer.apple.com/documentation/arkit/arimagetrackingconfiguration)`类的重要性。

## **ARImageTrackingConfiguration**

当我们只想使用设备的背面摄像头来跟踪已知图像时，我们会使用该配置。

通过`ARImageTrackingConfiguration`，ARKit 不是通过跟踪设备相对于世界的运动，而是仅仅通过检测和跟踪相机视野中已知 2D 图像的运动来建立 3D 空间。

![](img/4665429f390c8d167df11db107a4018b.png)

图片来源:[http://hongchaozhang.github.io/](http://hongchaozhang.github.io/)

> **让我们为这个教程收集资源**

对于本教程，我们将使用下面的**图像和视频**进行图像跟踪和叠加视频。

![](img/9d199d00131ef510bb8e3c3a7211448b.png)

图片来源:[https://foreignpolicy.com/](https://foreignpolicy.com/)

> **让我们创建一个新的增强现实应用**

选择增强现实应用程序作为我们新项目的模板。

![](img/e914250908dca8c5294f8bbd6295234d.png)

根据您的方便，为您的项目命名。我把它命名为***“ARDemo”。*** 选择语言为***【Swift】****内容技术为***【scene kit】***用户界面为 ***【故事板】*** 。*

*![](img/c73fd802660226a8600f886c7be2c11d.png)*

> ***让我们创建一个新的 AR 资源组***

*从项目文件夹结构中选择*“assets . xc assets”*创建一个新的 **AR 资源组**，然后选择底部的*“+”*图标，选择*“新建 AR 资源组”*选项，如下图所示。*

*![](img/654c6810c0b12e7df3dbf3939156009d.png)**![](img/fef6bb894be833f11241e7acd3232c84.png)*

*然后，我们需要将我们的参考图像添加到这个新的 AR 资源组中，如下所示。*

*![](img/55d829d2602cc95cd8729a80711df597.png)*

*您可能会收到一条警告，说明 **" *AR 参考图像"图像"必须具有非零的正宽度"。*** 为了消除这个警告，我们需要增加 AR 参考图像的尺寸，如下图所示。*

*![](img/f705eb66842a93df20aab77d6c3c8011.png)*

> ***让我们将视频添加到我们的项目中***

*然后，我们需要将视频添加到我们的项目中，该项目将在图像上叠加播放，如下所示。*

*![](img/563130fcc563c4c16b333167691fad6c.png)**![](img/42ad769ef3b0012388b1b409a9df26e0.png)*

> ***让我们看看 ship.scn 文件***

*起始项目将如下所示。*

*![](img/aae29239e339025965aba15aa4247a02.png)*

*从 ship.scn 文件中移除 shipMesh，并将场景图形重命名为*“container”*，如下图所示。*

*![](img/59de9579a8a6c275e45355ac7d575c85.png)*

*然后添加一个空节点作为“container”节点的子节点，命名为“videoContainer”。完成后，添加一个平面并重命名为*“视频”*，如下图所示。*

*![](img/0643c60e67bd06de5ec131ca8720fdab.png)**![](img/e5fdbd33c5b67b2d672cf91bce7dbba4.png)*

*此外，添加环境光和平行光作为容器节点的子节点，如下所示。*

*![](img/b635ce3f393e1419d7742ae0511b014a.png)*

*然后改变环境光的强度为 200，平行光的强度为 800，如下图所示。*

*![](img/0edfe7998c4092b67c0bc705875fc31d.png)**![](img/0fbe23f227ce80dfffca2be49712c3a6.png)*

*然后更改视频平面的比例值和欧拉角，并使容器节点隐藏，如下所示。*

*![](img/698f4e2ec50a067d316b836e86a2da7a.png)**![](img/c88e583993a5c83ebe92a3960edee683.png)*

*我们已经完成了 ship.scn 文件。*

> ***选择 ViewController.swift 文件***

*让我们首先为 videoNode 和 videoPlayer 创建引用，如下所示。*

```
***@IBOutlet** **var** sceneView: ARSCNView!
**var** videoNode: SKVideoNode!
**var** videoPlayer: AVPlayer!*
```

*用如下所示的代码替换 *"viewWillAppear"* 方法。这里，首先创建一个*“ARImageTrackingConfiguration”*的实例，然后我们从*“AR 资源”*中检索所有图像，并将其分配给配置的跟踪图像，然后运行配置。*

```
***override** **func** viewWillAppear(**_** animated: Bool) {
**super**.viewWillAppear(animated)// Create a session configuration
**let** configuration = ARImageTrackingConfiguration()**guard** **let** arImages = ARReferenceImage.referenceImages(inGroupNamed: "AR Resources", bundle: **nil**) **else** { **return** }configuration.trackingImages = arImages// Run the view's session
sceneView.session.run(configuration)
}*
```

*然后用下面的代码添加下面的委托方法*" scnscenerender "*，如下所示。*

```
***func** renderer(**_** renderer: SCNSceneRenderer, didAdd node: SCNNode, for anchor: ARAnchor) {**guard** anchor **is** ARImageAnchor **else** { **return** }**guard** **let** referenceImage = ((anchor **as**? ARImageAnchor)?.referenceImage) **else** { **return** }**guard** **let** container = sceneView.scene.rootNode.childNode(withName: "container", recursively: **false**) **else** { **return** }container.removeFromParentNode()
node.addChildNode(container)
container.isHidden = **false****guard** **let** videoURL = Bundle.main.url(forResource: "video", withExtension: ".mp4") **else** { **return** }
videoPlayer = AVPlayer(url: videoURL)**let** videoScene = SKScene(size: CGSize(width: 720.0, height: 1280.0))
videoNode = SKVideoNode(avPlayer: videoPlayer)
videoNode.position = CGPoint(x: videoScene.size.width/2, y: videoScene.size.height/2)
videoNode.size = videoScene.size
videoNode.yScale = -1
videoNode.play()
videoScene.addChild(videoNode)**guard** **let** video = container.childNode(withName: "video", recursively: **true**) **else** { **return** }video.geometry?.firstMaterial?.diffuse.contents = videoScenevideo.scale = SCNVector3(x: Float(referenceImage.physicalSize.width), y: Float(referenceImage.physicalSize.height), z: 1.0)video.position = node.position// For Animation
**guard** **let** videoContainer = container.childNode(withName: "videoContainer", recursively: **false**) **else** { **return** }videoContainer.runAction(SCNAction.sequence([SCNAction.wait(duration: 1.0), SCNAction.scale(to: 1.0, duration: 0.5)]))
}*
```

*现在在物理设备上运行应用程序。您应该能够看到视频在图像上方播放，如下所示。*

*此外，视频本身会随着锚定图像大小的变化而调整大小。*

*但是我们还没有完成。在结束之前，我们还需要思考一个问题。当我们将设备从锚定图像移开时，视频仍会继续播放。我们将如何解决这个问题？*

> ***实现暂停视频功能***

*只需将下面几行代码添加到**暂停或恢复**视频中，它就会为我们变魔术。*

```
***func** renderer(**_** renderer: SCNSceneRenderer, didUpdate node: SCNNode, for anchor: ARAnchor) {**guard** **let** imageAnchor = (anchor **as**? ARImageAnchor) **else** { **return** }**if** imageAnchor.isTracked {
   videoNode.play()
} **else** {
   videoNode.pause()
}
}*
```

# *结论*

*在本教程中，我们已经看到了如何将一个视频叠加在一个锚定图像上，它可以随着锚定图像的变化而动态地改变其大小。我们还看到了场景渲染器更新后如何暂停和恢复视频。*

*本教程的源代码可以在 [**这里**](https://github.com/paloamit/ARVideo.git) 找到。*

*如果你觉得这些有用，请随意分享。感谢阅读！*

*希望你喜欢看《猫和老鼠》。*