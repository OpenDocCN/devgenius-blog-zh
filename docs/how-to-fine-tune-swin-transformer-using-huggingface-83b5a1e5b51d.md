# 如何使用 Huggingface 微调 Swin 变压器

> 原文：<https://blog.devgenius.io/how-to-fine-tune-swin-transformer-using-huggingface-83b5a1e5b51d?source=collection_archive---------1----------------------->

## 使用训练器 API 微调预训练的 SOTA 模型

![](img/5296240f234f796729d75718947c6d50.png)

詹姆斯·哈里森在 Unsplash 上的照片

CNN 统治计算机视觉领域的日子已经一去不复返了，现在基于变形金刚的模型比以往任何时候都更能展示 SOTA 在许多任务上的表现。

在本教程中，我将向你展示如何微调其中一个模型，用于图像分类的 [Swin Transformer](https://arxiv.org/abs/2103.14030) 。我将只使用 [Huggingface](https://huggingface.co) 平台在 [Foods101](https://huggingface.co/datasets/food101) 数据集上进行，更具体地说，是使用 transformers 和 datasets 库。

**备注:**

1.  如果您想在继续之前亲自测试该模型，可以找到[(此处)](https://huggingface.co/aspis/swin-finetuned-food101)。
2.  要理解本教程，你需要具备 python 和迁移学习的中级知识。
3.  如果你想知道更多关于 Swin 变压器的工作原理，[这篇伟大的文章](https://medium.com/towards-data-science/a-comprehensive-guide-to-swin-transformer-64965f89d14c)深入解释了它。
4.  如果你只是想要完整的代码，可以在这个 Google Colab 笔记本里找到[(这里)](https://colab.research.google.com/drive/15VROFR3wY56ubXh31iUR3Oa54-yGg2L8?usp=sharing)。

## 总结:

1.  步骤 1:加载和预处理数据。
2.  步骤 2:初始化模型。
3.  第三步:培训和评估。

## 步骤 1:加载和预处理数据。

本教程中使用的数据集是 [Foods101 数据集](https://huggingface.co/datasets/food101)，它已经在 [Huggingface 的 datasets](https://huggingface.co/docs/datasets/index) 库中可用，但在自定义数据集上执行此任务将是直接的，您只需有一个 csv 文件，其列的格式为:[PIL 图像|标签]，并使用 datasets 库加载它。

foods101 数据集是一个为图像分类而设计的数据集，它由 101 类食物的 101000 张图片组成，包括寿司、草莓酥饼、马卡龙等等。

为了加载数据，我们只需使用 [load_dataset](https://huggingface.co/docs/datasets/loading) 函数，并可视化数据集的结构、示例图像和所有可用的标签

但是我们不能只是将原始图像输入到我们的模型中，还需要一些预处理步骤，比如调整图像的大小和归一化，直到我们得到模型作为输入接收的像素值。为此，我们将使用 transformers 库中提供的该模型的内置特征提取器，然后将其批量应用于我们数据集中的图像:

## 步骤 2:初始化模型。

首先，我们需要创建数据排序器，它将负责从数据集中的示例列表创建批处理:

之后，我们需要定义我们将用来评估我们的模型的度量，这里使用的度量是准确性，因为这是图像分类任务的满意度量。

现在，我们使用 transformers 中的 SwinForImageClassification 类来定义模型:

我们根据数据集设置标签，并使用标签列表定义 id(整数)到标签(字符串)的交换。

由于该模型是在 Imagenet 1k 数据集上预先训练的，因此`ignore_mismatched_sizes = True``参数是必需的，这意味着它预期预测该数据集中 1000 个标签中的一个，而不是我们的 101 个标签，但是通过将该参数设置为`True``,我们告诉模型忽略标签数量的不匹配。

## 第三步:培训和评估。

下一步包括定义训练参数和训练器对象，以便我们可以训练我们的模型:

我们定义了基本但非常重要的训练参数，如学习速度、批量、热身步骤等等。

代码中值得注意的几行:

`remove_unused_columns = False``是必要的，因为我们需要 image 列来创建模型的输入(pixel_values)，但是因为这个列本身没有被模型使用，所以我们需要确保它没有被删除。

使用`push_to_hub = True``线，在训练结束后，模型被自动推送到 [Huggingface 的模型中枢](https://huggingface.co/models)。

定义参数后，我们用之前编码的函数和我们定义的参数实例化一个训练器对象。

对于下一部分，我们需要训练模型并评估验证集的结果:

现在剩下要做的就是等待训练结束，检查结果，并在任何你想要的时候用[推理 API](https://huggingface.co/docs/api-inference/index) 来使用它！

我希望这篇教程能帮助那些想用一种简单的方法得到好的结果的人，使用 SOTA 变换模型进行图像分类，使用非常简单的训练器 API 和 Huggingface 提供的强大的预训练模型。

## 参考

[1][https://arxiv.org/abs/2103.14030](https://arxiv.org/abs/2103.14030)

[2][https://huggingface.co/transformers/v4.1.1/examples.html](https://huggingface.co/transformers/v4.1.1/examples.html)

[3]https://huggingface.co/docs