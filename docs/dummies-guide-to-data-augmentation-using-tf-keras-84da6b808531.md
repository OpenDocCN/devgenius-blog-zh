# 使用 tf.Keras 进行数据扩充的假人指南

> 原文：<https://blog.devgenius.io/dummies-guide-to-data-augmentation-using-tf-keras-84da6b808531?source=collection_archive---------13----------------------->

## 所有你需要知道的关于使用 keras 的数据扩充。

![](img/b287f204256a60000f7fdf2236c85788.png)

照片由 [Soragrit Wongsa](https://unsplash.com/@invictar1997?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

D 数据扩充是我们在修改现有数据的基础上创建新数据的过程，因此我们正在对训练集中的数据进行合理的修改。

# ☁️Reason's 为什么我们需要做数据增强。

## ➡️ 1.我们可能只是想向我们的训练集添加更多的数据

假设我们的训练集中的数据相对较少，并且很难获得更多的数据。因此，我们可以使用数据扩充从现有数据集创建更多数据，从而创建更多样本

## ➡️ 2.我们可能想用它来减少过度拟合。

如果我们有更少的训练数据，那么我们的模型更容易**过度拟合**，减少**过度拟合**的一种方法是向我们的训练集添加更多的数据或样本。因此，如果我们无法获得额外的样本，我们可以使用**数据扩充**向我们的训练集添加更多的数据。

# 使用 Keras 实现数据扩充。

## 📃导入所需的库。

![](img/1693ddac94e3e9d9921230ce534ed3a3.png)![](img/dc31206df526077656e96b40843509fe.png)

## 👉1.旋转图像😺

![](img/8de6efab9818c1f036951b97630c87c5.png)![](img/e5de7b6dee997c300fb381188a112a39.png)![](img/bdfc7415514d79e121f85881e0333660.png)

## 👉2.改变亮度。🌄

![](img/488a9ce76d868a01989b79aee1b49882.png)![](img/4822fe57e3e9dc84b96d820b1f342817.png)![](img/1cc40a3ec4f6fdfb710167a8c2bdc6ea.png)

## 👉 **3。移动高度和宽度。🔳**

![](img/226a05526db8f20ef0f93a3746f3b24c.png)![](img/967c6164fd150a2f9cc631a6c7ca9951.png)![](img/eb871cac8230bff0465652a09be26604.png)

## 👉4.剪切图像。🔷

![](img/dc311af1b25580cd19710f79d357f843.png)![](img/1cc40a3ec4f6fdfb710167a8c2bdc6ea.png)![](img/9ffcea240986eb4ba4bdcafc3bcb5ed6.png)

## 👉5.缩放图像。🔲

![](img/e41919d5694e1910b42d2c64702a09af.png)![](img/a5176d85617ecc09c6f04d2098e39f45.png)![](img/69dcd130890c97f0565ef12f2390680a.png)

## 👉6.转换频道

![](img/f54eb5cd67448037582bb8787b5fabf5.png)![](img/01a33c5d22c11c9981e1e6fa7c54ec9c.png)![](img/320514629a991cdba81d17c4dd4c2bf1.png)

## 👉7.水平翻转图像⏫或者垂直▶️.

![](img/b8fbacf3159baa31e2d280071f88b51f.png)![](img/5a0dd7f66e39bde4c72e7abd607bba3a.png)![](img/19bfc9b4868262f5c5f6fc718ca91a5f.png)

## 👉8.灰度和饱和度。🎴

![](img/2433ca2d37c77b557757416857e6d947.png)![](img/52e8a91874bcc52851eb67f0f555f027.png)![](img/821534521723eefaf3e4c3231f43ebcb.png)

# 📑**更多用于数据扩充的 tf.image 函数:**

`**tf.image.stateless_random_brightness**`

`**tf.image.stateless_random_contrast**`

`**tf.image.stateless_random_crop**`

`**tf.image.stateless_random_flip_left_right**`

`**tf.image.stateless_random_flip_up_down**`

`**tf.image.stateless_random_hue**`

`**tf.image.stateless_random_jpeg_quality**`

`**tf.image.stateless_random_saturation**`

# 💭何时使用数据增强，何时不使用？

假设一个场景，您希望训练模型来预测图像是否是狗🐕或者不是狗❎🐕。

![](img/7fd2d264e91fec6b40f8ecb7d34bf665.png)

1.  你想要训练你的模型在**直立姿势** ( *左图*)的狗的图像上，因为模型更可能在大多数真实世界的图像中看到直立的狗，而不是狗的**垂直翻转图像** ( *右图*)。**这就是为什么我们不应该在这种情况下垂直翻转数据。**

![](img/0fad167244b147d0e085e779854f1a18.png)

2.如果你只有向左的**狗的图像**🐕并且你在这些图像上训练该模型，很可能你的模型可以预测面向右边的狗的图像不是狗。

我们可以使用**数据增强**通过垂直翻转图像来生成图像。