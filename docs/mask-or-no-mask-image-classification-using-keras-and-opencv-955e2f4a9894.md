# 基于 Keras 和 OpenCV 的掩膜或无掩膜图像分类

> 原文：<https://blog.devgenius.io/mask-or-no-mask-image-classification-using-keras-and-opencv-955e2f4a9894?source=collection_archive---------8----------------------->

# 什么是 Keras 和 OpenCV？

Keras 是为人类设计的 API，不是为机器设计的。Keras 遵循减少认知负荷的最佳实践:它提供一致而简单的 API，最大限度地减少常见用例所需的用户操作数量，并提供清晰可操作的错误消息。它还有大量的文档和开发人员指南。

OpenCV(开源计算机视觉库)是一个开源的计算机视觉和机器学习软件库。OpenCV 旨在为计算机视觉应用提供一个公共基础设施，并加速机器感知在商业产品中的应用。作为一个 BSD 许可的产品，OpenCV 使得企业利用和修改代码变得很容易。

# 什么是图像分类？

![](img/0e6e8c0b1765e8d16d387fb5bc541072.png)

图像分类如何工作的表示

图像分类是指计算机视觉中可以根据图像的视觉内容对图像进行分类的过程。例如，图像分类算法可以被设计成辨别图像是否包含人形。虽然检测物体对人类来说是微不足道的，但鲁棒的图像分类仍然是计算机视觉应用中的一个挑战。

现在，为了在 Keras 中获得这种功能，我们使用我们称之为卷积神经网络的东西

![](img/bf8b76e7870795a38c75550fe4f6a238.png)

CNN 工作方式的展示

# 基于 Keras 的图像分类

因此，首先，我们需要数据，而这种需求是通过使用 Kaggle 的掩膜数据集来满足的

[](https://www.kaggle.com/ahmetfurkandemr/mask-datasets-v1) [## 掩膜数据集 V1

### 戴口罩和不戴口罩的人。

www.kaggle.com](https://www.kaggle.com/ahmetfurkandemr/mask-datasets-v1) 

现在我们需要安装一些额外的东西

> **pip 安装 keras opencv**

现在让我们导入重要的库。

如果您需要有关的更多信息，请参考 Keras 文档，网址为

[](https://keras.io/) [## keras:Python 深度学习 API

### Keras 是为人类设计的 API，不是为机器设计的。Keras 遵循减少认知负荷的最佳实践:it…

keras.io](https://keras.io/) 

现在，让我们准备数据集，以便在 CNN 模型中使用它

第三，定义我们的 CNN 模型

现在到了最终编译并在数据集上拟合模型的时候了

训练数据集的精度为 0.9907，测试数据集的 val_accuracy 为 0.9829，我们可以开始了。

在对模型进行训练之后，我们使用 Keras save()函数将模型保存为 mask.h5。

现在到了使用 OpenCV 的部分，并通过网络摄像头尝试这个模型。

再次为所有函数使用参考请访问

[](https://opencv.org/) [## OpenCV

### 开放计算机视觉库

opencv.org](https://opencv.org/) 

现在来看看实际情况

现在，要获得更多代码和详细代码，请参考我的 GitHub 库

[](https://github.com/aditya0072001/Maskify) [## aditya0072001/Maskify

### 使用 CNN 和 OpenCV 的掩膜或无掩膜图像分类

github.com](https://github.com/aditya0072001/Maskify) 

如果你想和我联系，请通过我的 LinkedIn 联系我

https://www.linkedin.com/in/tripathiadityaprakash