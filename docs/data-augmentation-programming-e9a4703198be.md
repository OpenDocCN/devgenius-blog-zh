# 使用 Python 进行深度学习的数据增强

> 原文：<https://blog.devgenius.io/data-augmentation-programming-e9a4703198be?source=collection_archive---------0----------------------->

![](img/b8d26f14177fe9452e263fbb2461ef4a.png)

[来源](https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/bd253797333277.5ec2cda43ec50.png)

## 处理深度学习的小数据集。

数据扩充是一种技术，可用于制作数据集中图像的更新副本，以人为增加训练数据集的大小。当训练数据集非常小时，这种技术非常有用。

已经有很多关于这个概念的好文章发表了。我们可以参考其中的一些文章，在[数据增强](https://medium.com/nanonets/how-to-use-deep-learning-when-you-have-limited-data-part-2-data-augmentation-c26971dc8ced)了解[何时使用](https://medium.com/nanonets/nanonets-how-to-use-deep-learning-when-you-have-limited-data-f68c0b512cab)数据增强，以及其他重要概念。

想象一下，你害怕灭霸，并且相信他是真实存在的，并且有一天会造访地球。作为衡量的标志，您想要建立一个依靠摄像机输入的防御系统。当灭霸到达地球时，这个系统将被激活，对他从相机里得到的图像进行分类。为此，我们需要训练一个可靠的模型来激活防御系统。如果我们只有 10 张灭霸的照片，就很难建立一个可靠的模型来捕捉他的存在。

因此，为了训练集有多个图片，我们可以考虑数据扩充。在[点击这里](https://medium.com/nanonets/nanonets-how-to-use-deep-learning-when-you-have-limited-data-f68c0b512cab)中提到了何时使用增强的更好的例子和场景。让我们考虑下面的图像是一个我们想要执行数据扩充。

![](img/119336d66b1ec210d8530c88ff930a3c.png)

图片由 Matt McGloin 在 [comicbook.news](https://cosmicbook.news/avengers-infinity-war-characters-assemble-giant-poster)

在本文中，我将只关注数据扩充的编码部分。

首先，我们将看看如何使用 NumPy 来实现这一点，然后我们将讨论 Keras 中的图像预处理数据增强类，它为这项任务带来了简单性。

## 使用 Numpy

导入所需的模块。

加载要处理的图像。

**裁剪:**通过裁剪，我们可以捕捉图像中需要的部分。这里我们随机裁剪来捕捉图像的随机窗口。从原始图像中裁剪过小的图像会导致信息丢失。

![](img/be0029a1911b57a53be557953ae66930.png)

从原始图像中随机裁剪的图像

**旋转图像:**旋转图像，捕捉不同角度拍摄画面的实时效果。

![](img/d99f5f9c4013ff3b7ce92735f355a819.png)

旋转图像后的样本输出

**图像移动或称为图像平移:**这只不过是在某个方向上移动图片的像素，并在相反的方向上将移动的像素加回来。

![](img/a722984d0fc1472a447a9afcc6279fb0.png)

移动图像后的样本输出

为了获得更好的效果，我们可以结合这些技术，因为我们将得到不同风格的增强图片。

我们已经看到，使用 NumPy 需要花费大量精力来手动更改图像数组的值，这不仅计算量大，而且需要大量代码，如上所述。

现在，我们可以尝试使用 Keras 神经网络框架进行增强，这使得我们的工作容易得多。

# 使用张量流和 Keras

TensorFlow 有一个单独的类，它通过许多不同的选项来处理数据扩充，而不仅仅是翻转、缩放和裁剪图像。

通过使用 Keras，不需要手动调整像素。Keras 有处理这些事情的内置函数。因此，使用 Keras 进行增强所需的代码更少，而且有多种选择。

让我们看看 Keras 的 image preposing imagedata generator 类:

让我们看看常用数据论证技术的重要论点:

*   **旋转 _ 范围:** Int。随机旋转的度数范围。
*   **width_shift_range:** Float，一维数组类型或 int —总宽度的一部分
*   **height _ shift _ range:**Float，一维数组类型或 int —总高度的一部分
*   **brightness_range:** 两个浮点的元组或列表。选择亮度偏移值的范围。
*   **剪切 _ 范围:**浮动。剪切强度(逆时针方向的剪切角度，单位为度)
*   **zoom_range:** Float 或【下，上】。随机缩放的范围。如果是浮点型，[lower，upper] = [1-zoom_range，1+zoom_range]。要缩放的整个图像的一部分。
*   **水平 _ 翻转:**布尔型。随机水平翻转输入。
*   **垂直 _ 翻转:**布尔型。随机垂直翻转输入。
*   **重新调整:**重新调整因子。默认为无。如果无或为 0，则不应用重缩放，否则我们将数据乘以所提供的值(在应用所有其他变换之后)。
*   **预处理 _ 函数:**将应用于每个输入的函数。该功能将在图像调整大小和放大后运行。该函数应该采用一个参数:一个图像(秩为 3 的 Numpy 张量),并且应该输出具有相同形状的 Numpy 张量。
*   **data_format:** 图像数据格式，可以是“channels_first”，也可以是“channels_last”。
*   validation_split: Float 保留用于验证的图像比例(严格介于 0 和 1 之间)。
*   dtype:用于生成的数组的 Dtype。

更多细节和论据请查看 tf 文档**[**。**](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator)**

现在，我们将使用一些最常见的技术来增强我们的图像，如翻转、旋转、宽度和高度移动、改变图像的亮度、缩放和重新缩放图像。

![](img/087be70ca28658c04a575a5dbc8eaf49.png)

使用 Keras 增强后的示例输出

现在让我们看看如何扩充一个完整的数据集。我们将考虑 cifar10 数据集。

从上面的例子中我们可以注意到，使用 Keras 进行数据扩充比使用 NumPy 更好。

希望这些大量的增强图像可以帮助你激活你的防御系统，拯救我们的星球。

完整的木星笔记本可以在我的[***git hub***](https://github.com/sai-teja-ponugoti/Machine-Learning-Concepts)***找到。***

这是我的第一篇文章，请从现在开始就如何改进我的文章提供反馈。