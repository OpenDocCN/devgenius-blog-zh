# 游戏开发的第 57 天:如何在 Unity 中使用 Cookies 进行逼真的照明！

> 原文：<https://blog.devgenius.io/day-57-of-game-dev-how-to-use-cookies-for-realistic-lighting-in-unity-5a72cba60119?source=collection_archive---------7----------------------->

**目标:**回顾什么是**cookie**并落实到我的 **Unity** 场景中。

在 **Unity** 中的 **cookie** 是一种只关心 alpha/透明通道的纹理或遮罩。你可以在聚光灯或点光源上使用它们来获得特定的效果，比如手电筒。

![](img/05fd4a7312dc288b4a0076b32c1a8bba.png)![](img/06db6ac50ff5839775e3d48f49cfb1c0.png)

对于我的例子，我将把上面的第一张图片(在 google 上找到的)放到我的项目中。我们可以将这个 cookie 应用到聚光灯或点光源上，每个都有一点不同。

让我们从聚光灯开始。我将为我的场景添加一个聚光灯。提高强度和更多，使这真的很容易看到。

![](img/d943fe2afcdd2251472436753cc28207.png)![](img/2569ff621d1a4141bcf78c086ff56cef.png)

在纹理上，我需要设置纹理类型为 **cookie** 。

![](img/001fc3cb99206dc54f307a463658a824.png)

确保灯光类型设置为**聚光灯**。

![](img/e07edee1b7ef1a1971d9e8fc4c2d950e.png)

然后将 alpha 源设置为**灰度**并检查 **alpha is transparency** 框。

![](img/0a4a7268d66e132617ff2cf3d0cf1d96.png)![](img/698d4feb1cfdfeb1479156fade2e27fd.png)

应用更改

回到聚光灯下，将纹理分配给 **cookie** 槽。

![](img/9b2b7c8dfb8260ddabcf77cf749e7b8f.png)

点击**修复**。

![](img/37ce4be86fd1bc2715abac7153a4263c.png)

看哪！

![](img/843e41653d1276a22b18f62dec1f2ad1.png)

当使用手电筒时，这有助于你的灯光看起来令人惊叹和真实。当然，这可以用来实现许多不同的效果。到聚光灯上！

![](img/60f4e97fa1b1b411979e4fb725117574.png)![](img/9221c36e820df96605897a6d1e0454d7.png)

在我的场景中添加了点光源并提高了亮度以便于观察之后，我将再次处理纹理。做和上次一样的事情，除了不是选择聚光灯，而是选择**点**。并且还要检查**夹具**边缘接缝**盒。然后点击应用！**

![](img/fa71ea7a20fd27c6cf961f0d502dc366.png)

然后我会应用纹理，点击**修复**。

![](img/8a0184d67d8d863a29c0303e44659c36.png)

使用它，我们可以制作一个非常酷的老式灯。这可以用来做吊灯、烛台等等。

![](img/4fa395bb05d384a82ae8d0f39700f560.png)

***本文到此为止。如果您有任何问题或建议，请随时评论。让我们做一些很棒的游戏吧！***