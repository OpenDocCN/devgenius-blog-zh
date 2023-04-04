# 游戏开发的第 70 天:如何在 Unity 中为单个摄像机添加自定义后期处理配置文件！

> 原文：<https://blog.devgenius.io/day-70-of-game-dev-how-to-add-custom-post-processing-profiles-to-individual-cameras-in-unity-c1bedb2cbd9d?source=collection_archive---------15----------------------->

**目的:**使用 **Cinemachine** 为不同的虚拟摄像机添加自定义**后处理轮廓**。

首先确保 **Post** **Processing** 安装在您的项目中。

![](img/34e266263b7fbe0bc2d42bdd3ff4fec2.png)

在主相机上，你需要添加一个**后处理层**。

![](img/efd74c1cc4882f65de996a528e5f3a16.png)

在组件中，您会注意到需要设置图层，并且您可能还没有一个专用于**后处理**的图层。所以我们需要创建一个并分配它。

![](img/1bca5da3332c729735643b7832160fc7.png)![](img/9b339456809b2ae117e0f351bd2ee54a.png)![](img/70f43e4a4c9ac31eae47cc892118011d.png)

我想展示我们如何在两个不同的摄像机之间切换，并对它们进行两种不同的**后处理**效果。

我需要给两台相机添加 **Cinemachine Post** **处理扩展**，并给两台相机添加一个新的配置文件。

![](img/79fed7d2e3a105c3fc0481668a52830e.png)![](img/2648d6ac52c7237601df78e9ad3724fd.png)

我在一台相机上添加了一点蓝色，在另一台相机上添加了高光，以使区别更加明显。

![](img/7aa2da2c7b7e5c7a244ccb35ac464890.png)![](img/2216727952f9b15ab7e444acd23f5478.png)

现在，当我在我使用的相机之间切换时，每个相机都有自己的自定义效果！

![](img/d4ca8667b486c9a743f9e57793467c79.png)

***如有任何问题或想法欢迎评论。让我们做一些很棒的游戏吧！***