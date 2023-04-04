# 超越拥抱脸的视觉变形金刚(ViT)|第 3 部分

> 原文：<https://blog.devgenius.io/scale-vision-transformers-vit-beyond-hugging-face-part-3-5b8c13ef6477?source=collection_archive---------22----------------------->

## 加快拥抱脸最先进的维生素 t 模型🤗借助 Databricks、Nvidia 和 Spark NLP，速度提升高达 2300%(25 倍)🚀

![](img/c7572592ba711def70ee3e626385d598.png)

**通过使用**数据模块**、**英伟达**和 **Spark NLP** 扩展**基于**变压器**的型号

《超越拥抱脸|第二部 》前情提要:

> **Databricks 单节点: *Spark NLP*** *比* ***上的抱脸器快 15%****CPU**在带有 3K 图像的样本数据集上预测图像类，在带有 34K 图像的较大数据集上快 34%****。****Spark NLP****也比***快 51%****GPU****对于拥有 34K 图像的更大数据集，速度可达******快 36%********

> ****本文的目的是演示如何从 Hugging Face 向外扩展 Vision Transformer (ViT)模型，并将其部署到生产就绪环境中，以实现加速和高性能的推理。最后，我们将通过使用 Databricks、Nvidia 和 Spark NLP，将拥抱脸的 ViT 模型扩展 25 倍(2300%)。****

## ****在本文的第 3 部分，我将:****

*   ****数据块内部的基准 Spark NLP 通过 CPU 和 GPU 扩展到 10 倍节点****
*   ****总结一切！****

> *****本着完全透明的精神，GitHub* 上的[](https://github.com/JohnSnowLabs/spark-nlp-workshop/tree/master/tutorials/blogposts/medium/scale-vision-transformers-vit-beyond-hugging-face?ref=hackernoon.com)****

******[](/scale-vision-transformers-vit-beyond-hugging-face-part-1-e09318cab588) [## 超越拥抱脸的视觉变形金刚(ViT)|第 1 部分

### 加快拥抱脸最先进的维生素 t 模型🤗使用 Databricks、Nvidia 和……最高可达 2300%(快 25 倍)

blog.devgenius.io](/scale-vision-transformers-vit-beyond-hugging-face-part-1-e09318cab588) [](/scale-vision-transformers-vit-beyond-hugging-face-part-2-b7b296d548b7) [## 超越拥抱脸的视觉变形金刚(ViT)|第 2 部分

### 加快拥抱脸最先进的维生素 t 模型🤗使用 Databricks、Nvidia 和……最高可达 2300%(快 25 倍)

blog.devgenius.io](/scale-vision-transformers-vit-beyond-hugging-face-part-2-b7b296d548b7) [](/scale-vision-transformers-vit-beyond-hugging-face-part-3-5b8c13ef6477) [## 超越拥抱面部的电子秤视觉变形金刚(ViT)|第 3 部分

### 在拥抱脸中加速最先进的 ViT 模型🤗使用 Databricks、Nvidia 和…可实现高达 2300%的性能提升(速度提升 25 倍)

blog.devgenius.io](/scale-vision-transformers-vit-beyond-hugging-face-part-3-5b8c13ef6477) 

# 超越单台机器

到目前为止，我们已经确定在裸机服务器和 Databricks 单节点上， **GPU** 上的**拥抱脸**比**CPU**上的**拥抱脸**更快。当您用这些新的基于变压器的模型比较 GPU 和 CPU 时，这就是您所期望的。

在相同的数据集上，在裸机服务器和 Databricks 单节点集群中，对于相同的管道(ViT 模型)，我们已经确定 **Spark NLP** 优于**拥抱脸**，并且在 CPU 和 GPU 设备上都表现更好。另一方面，这并不是我所期望的。当我准备这篇文章的时候，我期望 Spark NLP 中的 TensorFlow 推理比使用 PyTorch 的拥抱脸推理稍微慢一点，或者至少是不相上下。我的目标是这一部分，超越单台机器**扩展管道。但是 Spark NLP 似乎比拥抱脸更快，即使是在一台机器上，在两个 **CPU** 和 **GPU** 上，在两个**小的**和**大的**数据集上。**

**问题:**如果您想加快 ViT 管道的速度，该怎么办？如果您有更大的数据集，却无法将它们放在一台机器中，或者只需要太长时间才能得到结果，该怎么办？

**回答:**向外扩展！这意味着不要调整同一台机器的大小，而是向集群中添加更多的机器。您需要一些东西来管理所有这些作业/任务/调度 DAGs 管理失败的任务/等等。这些都有它们的开销，但是如果你需要更快或者可能的东西(在一台机器之外),你必须使用某种分布式系统。

> **按比例放大=** 使机器更大或更快，以便能够处理更多负载。
> 
> **横向扩展=** 并行添加更多机器以分散负载。

## 缩小拥抱脸:

查看拥抱脸官网的页面表明，只有使用多 GPU 才能进行缩放推断。正如我们所描述的横向扩展，它仍然停留在一台机器上:

![](img/a9b448ecfa2399e169931ade829d99ab.png)

[https://huggingface.co/docs/transformers/performance](https://huggingface.co/docs/transformers/performance)

> 另外，更不用说目前还不存在拥抱脸中**推理**的**多处理器**解决方案:

![](img/b8bd9bab0fdc85f032cb102b3ba4c848.png)

[https://hugging face . co/docs/transformers/perf _ infer _ GPU _ many](https://huggingface.co/docs/transformers/perf_infer_gpu_many)

因此，似乎没有本地/官方的方式来**扩展**拥抱脸管道。您可以实现由一些微服务组成的架构，例如作业队列、消息传递协议、RESTful APIs 后端和一些其他必需的组件，以将每个请求分布到不同的机器上，但这是按单个用户扩展请求，而不是扩展实际的系统本身。

此外，此类系统的延迟与 Apache Spark 等本地分布式系统不可同日而语(gRPC 可能会降低延迟，但仍然没有竞争力)。更不用说单点故障问题，管理失败的作业/任务/输入，以及 Apache Spark 提供的数百个其他特性，现在您必须自己实现/维护这些特性。

在拥抱脸网站上有一篇博文描述了通过扩展 REST 端点来服务更多用户的完全相同的架构:“[部署🤗ViT on Kubernetes with TF Serving](https://huggingface.co/blog/deploy-tfserving-kubernetes)“我相信其他公司也在使用类似的方法来扩展拥抱脸，但是，他们都在扩展触及推理 REST 端点的用户/请求的数量。此外，您不能在**数据块上以这种方式缩放拥抱脸。**

> 例如，fastAPI 内部的推理比本地推理慢 10 倍:[https://towards data science . com/hugging-face-transformer-inference-under-1 毫秒-latency-e1be0057a51c](https://towardsdatascience.com/hugging-face-transformer-inference-under-1-millisecond-latency-e1be0057a51c)

一旦拥抱脸提供了一些本地解决方案，我将再次运行基准。在此之前，当您必须在循环算法中从单台机器遍历数据集以到达 REST 端点时，没有向外扩展。(再想想我们一次将行/序列/图像分批送入 GPU 的部分，你就明白了)

## 扩展 Spark NLP:

Spark NLP 是 Spark ML 的扩展，因此它可以在 Apache Spark 支持的所有平台上进行本机无缝扩展，例如(但不限于)Databricks、AWS EMR、Azure Insight、GCP Dataproc、Cloudera、SageMaker、Kubernetes 等等。

> 零代码变化是必要的！Spark NLP 可以在不改变任何代码的情况下从单台机器扩展到无限多台机器！

您也不需要从 Spark NLP 中导出任何模型，就可以在一个完全不同的库中使用它来加速或扩展推理。

![](img/9b1bad07da23d8b0c50be3108e6d5360.png)

Spark NLP 生态系统:经过优化、测试和支持的集成

# AWS 上带 CPU 的 Databricks 多节点

让我们创建一个集群，这次我们选择**标准**内部**集群模式**。这意味着我们的集群中可以有不止一个节点，在 Apache Spark 术语中，这意味着 1 个驱动程序和 N 个工作器(执行器)。

![](img/da07268fe0bda4ee36742edbd9368fa7.png)

我们还需要通过`Libraries`选项卡在这个新集群中安装 Spark NLP。对于带 CPU 的单节点数据块，您可以遵循我在上一节中提到的步骤。如您所见，我选择了我用来测试 Hugging Face 和 Spark NLP 的基于 CPU 的 AWS 实例，这样我们就可以看到当我们添加更多节点时它是如何扩展的。

这是我们的集群配置的样子:

![](img/80faa32b85c6e0d184e812928ec5a2bd.png)

只有 CPU 的 Databricks 多节点(标准)集群

我将重用我在以前的基准测试中使用的相同的 Spark NLP 管道**(不需要更改任何代码)**，并且我将只使用具有 34K 图像的较大数据集。我们开始吧！

## 在具有 2 个节点的 CPU 上扩展 Spark NLP

![](img/7b71b564be70f221bda01f394ab04eca.png)

具有 2 个节点的数据块—仅限 CPU

我们只需再添加 1 个节点，将进行处理的机器总数从 1 台增加到 2 台。当您从单机设置(您的 Colab、Kaggle、Databricks 单节点，甚至您的本地 Jupyter 笔记本)过渡到多节点集群设置(Databricks、EMR、GCP、Azure、Cloudera、YARN、Kubernetes 等)时，我们不要忘记 Spark NLP 的魅力。)，需要零代码修改！我是说零！考虑到这一点，我将在这个新集群中对包含 34K 图像的较大数据集运行相同的基准测试:

![](img/50d3018d6008f5aa06d07ff8e8b62e64.png)

Spark NLP 图像分类流水线在带 CPU 的 2x 节点上预测 34742 幅图像

花了大约 **9 分钟** ( **550 秒**)完成了对 34K 图像的分类预测。让我们将在 **2x 节点**上的结果与 Spark NLP 和在 Databricks 单节点上的拥抱面结果进行比较(我将在单个节点上重复拥抱面结果作为参考，因为拥抱面不能在多台机器上扩展，尤其是在 Databricks 上):

![](img/bbea6b011556cd6f29678d8b8049d203.png)

**Spark NLP** 比拥有 **2x 节点**的拥抱脸**快 124%**

之前，Spark NLP 在单节点 Databricks 集群上仅使用 CPU 就击败了 Hugging Face**15%**。

> 这一次，Spark NLP 通过仅使用 2x 个节点而不是 1 个节点，完成了超过 34K 张图像的处理，比拥抱脸快了 124%。

## 在具有 4 个节点的 CPU 上扩展 Spark NLP

让我们像以前一样将集群的规模扩大一倍，从 **2x 节点**增加到 **4x 节点。**这是带有 4 个节点的集群的外观:

![](img/74e2c48ad59f2a7057dc58e508d66345.png)

带 4 个节点的数据块—仅限 CPU

我将在具有 34K 图像的较大数据集上对这个新集群运行相同的基准测试:

![](img/4cc7302a36d02c20f4a47608cef824be.png)

Spark NLP 图像分类流水线在带 CPU 的 4x 节点上预测 34742 幅图像

用了大约 **5 分钟** ( **289 秒**)完成了 34K 图像的预测分类。让我们比较一下在带有 Spark NLP 的 **4x 节点**上的结果和在 Databricks 上的 CPU 上的拥抱脸:

![](img/73e9f4deec683448139a6fcdcdedffaf.png)

**Spark NLP** 比拥有 **4x 节点**的拥抱脸**快 327%**

> 可以看出，Spark NLP 现在比 CPU 上的 Hugging Face 快**327%**,而在数据块中只使用了 **4x 个节点**。

## 在具有 8 个节点的 CPU 上扩展 Spark NLP

现在，让我们通过再添加 4x 个节点来使之前的集群翻倍，总共有 **8x 个节点**。顺便说一下，调整集群的大小非常简单，只需增加集群配置中的工作线程数量:

![](img/9ec2a3ae2cafb7767545052c98748d7c.png)

调整数据块中火花簇的大小

![](img/74f359e098fb9c3ab35239089543007c.png)

具有 8 个节点的数据块—仅限 CPU

让我们这次在 **8x 节点上运行相同的基准:**

![](img/0205fb6a8d8a05e5fc15799519b0a63a.png)

带 CPU 的 **8x 节点**上的 Spark NLP 图像分类流水线(oneDNN) —预测 34742 幅图像

用了 2 分半多钟( **161 秒**)完成了 34K 图像的预测分类。让我们比较一下在 **8x 节点**上 Spark NLP 的结果与在 Databricks 上 CPU 上拥抱脸的结果:

![](img/e7cee4124a4d99bdb00dff8380b29305.png)

**Spark NLP** 比 **666%快**用 **8x 节点抱脸**

> 可以看出，Spark NLP 现在比 CPU 上的 Hugging Face 快**666%**,而在数据块中仅使用 **8x 节点**。

让我们忽略这里的数字 6！(如果能让你感觉好点的话，是 665.8%)

## 在具有 10 倍节点的 CPU 上扩展 Spark NLP

为了使用 Spark NLP 完成我们对数据块中 CPU 的 ViT 模型预测，我将再次调整集群的大小，并将其增加到 **10x 节点:**

![](img/35387bb342509bc440368bb3dc510524.png)

具有 10 个节点的数据块—仅限 CPU

让我们这次在 **10x 节点上运行相同的基准:**

![](img/7369d45a6cca5bb693ce4c50a00f49bf.png)

在 **10x 节点**上使用 CPU(oneDNN)的 Spark NLP 图像分类流水线—预测 34742 幅图像

用了不到 **2 分钟** ( **112 秒**)完成了对 34K 图像的预测分类。让我们将 **10x 节点**上的结果与之前在 Databricks 上 Spark NLP 与 CPU 上 Hugging Face 的所有结果进行比较:

![](img/8e74ba0d57267ff42567a718599860a0.png)

**火花 NLP** 比 **10x 节点**抱脸**快 1000%**

> 这就是你如何通过使用 Databricks 中的 **Spark NLP** 在 **10x 节点**上扩展来自拥抱脸的视觉转换器模型！我们的流水线现在比 CPU 上的拥抱脸快 1000%。

我们设法通过简单地使用 Spark NLP 使我们的 **ViT** 流水线**比卡在 1 个单节点中的 Hugging Face**快 1000%,但是我们只使用了**CPU**。让我们看看是否可以通过在 **GPU 集群**上扩展我们的流水线来获得同样的改进。

## AWS 上带有 GPU 的 Databricks 多节点

基于 GPU 的多节点 Databricks 集群与单节点集群非常相似。唯一的区别是选择**标准**并保持相同的 ML/GPU 运行时和我们在单个节点上的 GPU 基准中选择的相同 AWS 实例规格。

我们还需要通过**库**选项卡在这个新集群中安装 Spark NLP。和以前一样，您可以按照我在使用 GPU 的单节点数据块中提到的步骤进行操作。

![](img/0d86b68b6b6eddaa73e08146c8f5c168.png)

带有 GPU 的 Databricks 多节点(标准)集群

## 在具有 2 个节点的 GPU 上扩展 Spark NLP

我们的多节点 Databricks GPU 集群使用相同的 AWS GPU 实例 **g4dn.8xlarge** ，我们之前使用该实例在单节点 Databricks 集群上运行我们的基准来比较 Spark NLP 与 Hugging Face。

这是对这一次两个节点情况的总结:

![](img/21cf05b03121602371cde7a1a808075b.png)

具有 2 个节点的数据块—每个节点 1 个 GPU

我将在这个带有 **2x 节点的 GPU 集群中运行相同的管道:**

![](img/57ce3be7d7954c3e897497039184242c.png)

带 GPU 的 **2x 节点**上的 Spark NLP 图像分类流水线—预测 34742 幅图像

用了 4 分钟( **231 秒**)完成了对 **34K 图像**的预测分类。让我们比较一下使用 Spark NLP 在 **2x 节点**上的结果与在 Databricks 中 GPU 上的拥抱脸的结果:

![](img/cbe3d6fcab53d8b68930bf493818d813.png)

**Spark NLP** 比 **2x 节点**拥抱脸**快 185%**

> 在使用 **GPU 的情况下， **2x 节点**的 Spark NLP 比 1 个单节点上的拥抱脸几乎快了**3 倍** ( **185%** ) 。**

## 在具有 4 个节点的 GPU 上扩展 Spark NLP

让我们将 GPU 集群的规模从 2x 节点调整为 **4x 节点。**这是这次使用 GPU 的 **4x 节点**的总结:

![](img/3fb66ac3aa15c2829efc587b34e4a3ce.png)

具有 4 个节点的数据块—每个节点 1 个 GPU

让我们在 4x 节点上运行相同的基准测试，看看会发生什么:

![](img/3ebca941790ba586c406baad903d3082.png)

带 GPU 的 **4x 节点**上的 Spark NLP 图像分类流水线—预测 34742 幅图像

这一次花了将近 2 分钟( **118 秒**)来完成对我们数据集中所有 **34K 图像**的分类。为了更好地理解单节点中的拥抱脸与多节点集群中的 Spark NLP 相比意味着什么，让我们将这一点形象化:

![](img/84891134ecfeaf980b7232373e1ba02f.png)

**Spark NLP** 比 **4x 节点**抱脸**快 458%**

> 与拥抱脸相比，性能提升了 458%。我们刚刚通过使用带有 4 个节点的 Spark NLP 使我们的流水线速度提高了 5.6 倍。

## 在具有 8 个节点的 GPU 上扩展 Spark NLP

接下来，我将调整集群的大小，使我的数据块中有 **8x 个节点**，总结如下:

![](img/77521154e009d3705a5db3dc39bbc607.png)

8 个节点的数据块—每个节点 1 个 GPU

提醒一下，每个 AWS 实例( **g4dn.8xlarge** )有 1 个**英伟达 T4 GPU 16GB** (15GB 可用内存)。让我们重新运行基准测试，看看我们能否发现任何改进，因为任何分布式系统中的横向扩展都有开销，而且您不能只是不断添加机器:

![](img/aeb6607ac9fb397733291825b4f2f964.png)

带 GPU 的 **8x 节点**上的 Spark NLP 图像分类流水线—预测 34742 幅图像

在我们的 Databricks 集群中，使用 **8x 节点**完成对 **34K 图像**的分类几乎花了一分钟( **61 秒**)。看来我们还是设法提高了性能。让我们将此结果与之前单节点中的拥抱脸与多节点集群中的 Spark NLP 的结果进行比较:

![](img/e64eb088cf338667db3046f90d824965.png)

**Spark NLP** 比 **8x 节点**抱脸**快 980%**

> 带 **8x 节点**的 Spark NLP 在 GPU 上几乎比抱脸快**11 倍(980%)** 。

## 在 10 倍节点的 GPU 上扩展 Spark NLP

类似于我们在 CPU 上的多节点基准测试，我想再次调整 GPU 集群的大小，使其拥有 **10x 个节点**，并在最终节点数量方面与它们相匹配。该组的最终总结如下:

![](img/6ad166e9d533ea5b3c5401004be95ff0.png)

具有 10 个节点的数据块—每个节点 1 个 GPU

让我们在这个特定的 GPU 集群中运行最后一个基准测试(零代码更改):

![](img/0d5d896cb9e02b9dc726f572a43c9c92.png)

使用 GPU 在 **10x 节点**上的 Spark NLP 图像分类流水线—预测 34742 幅图像

不到一分钟( **51 秒**)就完成了对超过 **34743 张图片**的预测分类。让我们将它们放在一起，看看我们如何在 Databricks 的 Spark NLP 管道中扩展来自 Hugging Face 的视觉转换器模型:

![](img/c5c46c078177659e1dfd73ba14c72020.png)

**Spark NLP** 比 **10x 节点**抱脸**快 1200%**

> 我们完了。

> 我们设法通过使用数据块中的 **Spark NLP** 在 **10x 节点**上扩展我们的**视觉转换器**模型。与 GPU 上的拥抱脸相比，我们的流水线现在快了 13 倍，性能提高了 1200%。

让我们总结一下所有这些基准测试，首先比较 CPU 和 GPU 之间的改进，然后通过在 GPU 上使用 Spark NLP，我们的流水线可以从拥抱 Face CPUs 到 Databricks 上的 10 倍节点快多少。

# 将所有这些整合在一起:

## 数据块:单节点和多节点

![](img/022eb831e0a0981de02648e83c123ea2.png)![](img/e8b8f4f578efe14305eaeb13849f9973.png)

Spark NLP 比 CPU 上的拥抱脸快 11 倍(1000%)

> **火花 NLP** 🚀在 **10x 节点上**带 CPU**比抱脸**快 1000% (11x 倍)🤗卡在带有 CPU 的单个节点中

![](img/005af23f850808c5c6b767af93e47b21.png)![](img/1986dbddc6d6544efe434616129a185e.png)

Spark NLP 比 GPU 上的拥抱脸快 13 倍(1192%)

> **火花 NLP** 🚀在 **10x 节点上**用 GPU**比抱脸**快 1192% (13x 倍)🤗用 GPU 卡在单个节点

我们的 AWS CPU 实例和 AWS GPU 实例的价格差异呢？(我的意思是，多付出就多收获，对吧？)

![](img/5bb02d507a090493858999d011459a83.png)

**带 CPU 的 AWS m5d.8xlarge** 与带 1 个 GPU 和类似规格的**AWS****g4dn . 8x large**

好的，所以价格看起来差不多！考虑到这一点，如果您从卡在单台机器上的**CPU**上的**拥抱面**移动到带有**10x GPU**的 **10x 节点**上的 **Spark NLP** ，您会获得哪些改进？

![](img/1670eec0bbfbfa9cf845c2dcdd3e9082.png)

**GPU 上的 Spark NLP** 比 CPU 上的拥抱脸快 25 倍(2366%)

> 火花 NLP🚀在 10 倍于 GPU 的节点上，比拥抱脸快 2366%(25 倍)🤗在具有 CPU 的单个节点中

## 最后的话

*   本着完全透明的精神，GitHub 上的 [**提供了所有笔记本及其日志、截图，甚至是带有数字的 excel 表格**](https://github.com/JohnSnowLabs/spark-nlp-workshop/tree/master/tutorials/blogposts/medium/scale-vision-transformers-vit-beyond-hugging-face)
*   缩放 Spark NLP 需要零代码更改。从单个节点数据块到 10 个节点运行基准测试意味着只需在同一个笔记本中重新运行相同的代码块
*   请记住，这两个库提供了许多最佳实践，可以针对不同的用例在不同的环境中优化它们的速度和效率。例如，在 Apache Spark 中，我没有谈到分区及其与并行性和分布的关系。有许多 Spark 配置可以微调集群，尤其是平衡 CPU 和 GPU 之间的任务数量。现在的问题是，有没有可能在我们用于基准测试的相同环境中提高它们的速度？答案是百分之百！我试图为两个库保留默认值和现成的特性，以利于大多数用户的简单性。
*   你可能想把拥抱脸和其他基于 DL 的 Pythonish 库打包到 Spark UDF 中来缩放它们。这在一定程度上是可行的，因为我自己曾经这样做过，现在仍然这样做(当没有本地解决方案时)。在 UDF 中包装这种基于 transformer 的模型时，我不会详细讨论内存过度使用、可能的序列化问题、更高的延迟和其他问题。我只想说，如果您使用 Apache Spark，请使用在 Apache Spark 上本地扩展所需特性的库。
*   在这篇文章中，我特意提到了 PyTorch 上的拥抱脸和 TensorFlow 上的 Spark NLP。这是一个很大的差异，因为在 PyTorch 和 TensorFlow 之间通过拥抱面孔进行的每个基准测试中，PyTorch 过去是，现在仍然是推理的获胜者。在拥抱脸中，PyTorch 的延迟要低得多，而且似乎比变形金刚中的 TensorFlow 快得多。事实上，Spark NLP 使用完全相同的 TensorFlow，并且在拥抱脸方面在每个基准测试中都领先于 PyTorch，这是一件大事。要么是拥抱脸的张量流被忽略了，要么是 PyTorch 只是在推理上比张量流快。不管是哪种情况，我都迫不及待地想看看当 Spark NLP 除了 TensorFlow 之外，还开始支持 TorchScript 和 ONNX 运行时会发生什么。
*   ML 和 ML GPU Databricks 运行时安装了拥抱脸，非常好。但这并不意味着拥抱脸在数据砖块中很容易使用。拥抱脸的 Transformer 库不支持 DBFS(Databricks 的原生分布式文件系统)或亚马逊 S3。正如您在笔记本中看到的，我必须下载数据集的压缩版本，并提取它们以供使用。这并不是 Databricks 和其他生产平台中的用户真正的做事方式。我们将数据保存在分布式文件系统中，并实施了安全措施，其中大部分都足够大，无法通过个人计算机下载。我必须下载我在 DBFS 已经有的数据集，压缩它们，上传到 S3，公开它们，然后在笔记本上重新下载。如果拥抱脸可以支持 DBFS/S3，这个相当乏味的过程就可以避免了。我认为 Databricks 预装拥抱脸很有吸引力，但这不仅仅是简单地在每个节点上运行`pip install transformers`而不是 100%兼容。

# 参考

**维特**

*   [https://arxiv.org/pdf/2010.11929.pdf](https://arxiv.org/pdf/2010.11929.pdf)
*   [https://github.com/google-research/vision_transformer](https://github.com/google-research/vision_transformer)
*   [图像识别中的视觉变压器(ViT)——2022 指南](https://viso.ai/deep-learning/vision-transformer-vit/)
*   [https://github.com/lucidrains/vit-pytorch](https://github.com/lucidrains/vit-pytorch)
*   [https://medium . com/mlearning-ai/an-image-is-worth-16x 16-words-transformers-for-image-recognition-at-scale-51f 3561 a9f 96](https://medium.com/mlearning-ai/an-image-is-worth-16x16-words-transformers-for-image-recognition-at-scale-51f3561a9f96)
*   [https://medium . com/nerd-for-tech/an-image-worth-16x 16-words-transformers-for-image-recognition-at-scale-paper-summary-3a 387 e 71880 a](https://medium.com/nerd-for-tech/an-image-is-worth-16x16-words-transformers-for-image-recognition-at-scale-paper-summary-3a387e71880a)
*   [https://gareemadhingra 11 . medium . com/summary-of-paper-an-image-worth-16x 16-words-3f 7 F3 ACA 941](https://gareemadhingra11.medium.com/summary-of-paper-an-image-is-worth-16x16-words-3f7f3aca941)
*   [https://medium . com/analytics-vid hya/vision-transformers-bye-bye-convolutions-e 929d 022 E4 ab](https://medium.com/analytics-vidhya/vision-transformers-bye-bye-convolutions-e929d022e4ab)
*   [https://medium . com/synced review/Google-brain-uncovers-re presentation-structure-differences-between-CNN-and-vision-transformers-83b 6835 db BAC](https://medium.com/syncedreview/google-brain-uncovers-representation-structure-differences-between-cnns-and-vision-transformers-83b6835dbbac)

**抱紧脸**

*   [https://hugging face . co/docs/transformers/main _ classes/pipelines](https://huggingface.co/docs/transformers/main_classes/pipelines)
*   [https://huggingface.co/blog/fine-tune-vit](https://huggingface.co/blog/fine-tune-vit)
*   [https://huggingface.co/blog/vision-transformers](https://huggingface.co/blog/vision-transformers)
*   [https://huggingface.co/blog/tf-serving-vision](https://huggingface.co/blog/tf-serving-vision)
*   [https://huggingface.co/blog/deploy-tfserving-kubernetes](https://huggingface.co/blog/deploy-tfserving-kubernetes)
*   [https://huggingface.co/google/vit-base-patch16-224](https://huggingface.co/google/vit-base-patch16-224)
*   [https://huggingface.co/blog/deploy-vertex-ai](https://huggingface.co/blog/deploy-vertex-ai)
*   [https://huggingface.co/models?other=vit](https://huggingface.co/models?other=vit)

数据块

*   [https://www . data bricks . com/spark/getting-started-with-Apache-spark](https://www.databricks.com/spark/getting-started-with-apache-spark)
*   [https://docs.databricks.com/getting-started/index.html](https://docs.databricks.com/getting-started/index.html)
*   [https://docs . data bricks . com/getting-started/quick-start . html](https://docs.databricks.com/getting-started/quick-start.html)
*   看最好的[数据+AI 峰会 2022](https://www.databricks.com/dataaisummit/)
*   [https://www . data bricks . com/blog/2020/05/15/shrink-training-time-and-cost-using-NVIDIA-GPU-accelerated-xgboost-and-Apache-spark-on-data bricks . html](https://www.databricks.com/blog/2020/05/15/shrink-training-time-and-cost-using-nvidia-gpu-accelerated-xgboost-and-apache-spark-on-databricks.html)

**火花 NLP**

*   [Spark NLP GitHub](https://github.com/JohnSnowLabs/spark-nlp)
*   [Spark NLP 研讨会](https://github.com/JohnSnowLabs/spark-nlp-workshop) (Spark NLP 示例)
*   [火花 NLP 变压器](https://nlp.johnsnowlabs.com/docs/en/transformers)
*   [Spark NLP 车型轮毂](https://nlp.johnsnowlabs.com/models?edition=Spark+NLP)
*   [Spark NLP 3 中的速度优化&基准测试:充分利用现代硬件](https://www.johnsnowlabs.com/watch-webinar-speed-optimization-benchmarks-in-spark-nlp-3-making-the-most-of-modern-hardware/)
*   [Spark NLP 中的硬件加速](https://nlp.johnsnowlabs.com/docs/en/hardware_acceleration)
*   [通过 API 服务 Spark NLP:Spring 和 LightPipelines](https://medium.com/spark-nlp/serving-spark-nlp-via-api-spring-and-lightpipelines-64d2e6413327)
*   [通过 API 服务 Spark NLP(1/3):微软的 Synapse ML](https://medium.com/spark-nlp/serving-spark-nlp-via-api-1-3-microsoft-synapse-ml-2c77a3f61f9d)
*   [通过 API (2/3)服务 Spark NLP:FastAPI 和 LightPipelines](https://medium.com/spark-nlp/serving-spark-nlp-via-api-2-3-fastapi-and-lightpipelines-218d1980c9fc)
*   [通过 API 服务 Spark NLP(3/3):数据块作业和 MLFlow 服务 API](https://medium.com/spark-nlp/serving-spark-nlp-via-api-3-3-databricks-and-mlflow-serve-apis-4ef113e7fac4)
*   [Spark 3.0 上的 GPU 利用 Scala 中的深度学习](https://aws.amazon.com/blogs/opensource/leverage-deep-learning-in-scala-with-gpu-on-spark-3-0/)
*   [开始使用 GPU 加速的 Apache Spark 3](https://www.nvidia.com/en-us/ai-data-science/spark-ebook/getting-started-spark-3/)
*   [阿帕奇 Spark 性能调优](https://spark.apache.org/docs/latest/sql-performance-tuning.html)
*   GPU 上可能的额外优化:[Apache Spark 配置的 RAPIDS 加速器](https://nvidia.github.io/spark-rapids/docs/configs.html)******