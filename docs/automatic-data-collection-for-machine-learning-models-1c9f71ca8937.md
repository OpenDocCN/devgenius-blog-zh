# 机器学习模型的自动数据收集

> 原文：<https://blog.devgenius.io/automatic-data-collection-for-machine-learning-models-1c9f71ca8937?source=collection_archive---------17----------------------->

![](img/3dcc099ef1ad100bcbd285964722f63f.png)

[自动图像分类生成器](https://github.com/serpapi/automatic-images-classifier-generator)允许你对机器学习模型进行自动化数据收集、自动化训练和测试。该工具由 [SerpApi 的 Google Images Scraper API](https://serpapi.com/images-results) 提供支持，绕过了手动数据输入的需要，并通过在其工作流程中提供预处理功能来减少人为错误。

本周我们将讨论如何为机器学习模型的训练和测试过程自动获取数据，并制作一个自动化脚本来收集最佳模型的信息。通过这种方式，我们可以收集关于如何优化模型的见解，并为哪些场景做出决策，以创建识别图像的初级人工智能的 line 工具版本。我只测试了代码的功能，并没有完全运行它，因为这是一个耗时的过程。但是它应该会让您了解如何在您的业务流程中利用它。

有关工具状态及其创建方式的更多信息，您可以滚动到页面底部。

# 自动化数据收集的好处是什么？

该软件正在使用 [SerpApi 的 Google Images API](https://serpapi.com/images-results) 对用于培训的图像进行自动数据收集，并通过绕过必要的手动过程对机器学习模型进行验证测试。有关实时自动数据收集系统的更多信息，用于以其他格式创建数据集，如 [SerpApi 的谷歌学术刮刀 API](https://serpapi.com/google-scholar-api) 用于案例研究的自动化，或其他用例，您可以前往[使用 SERP 数据构建机器学习模型](https://serpapi.com/use-cases/machine-learning-and-artificial-intelligence)页面。

[SerpApi 的抓取引擎](https://serpapi.com/status)通过快速、易于理解和完整的 Api 提供必要的自动化数据捕获系统。您可以[注册申请免费积分](https://serpapi.com/users/sign_up)。您也可以前往[定价](https://serpapi.com/pricing)页面，了解您心目中项目的成本。

在我们深入探讨如何为机器学习模型的不同元数据简化自动数据捕获过程之前。我想让读者了解一下如何以不同的方式使用[自动图像分类生成器](https://github.com/serpapi/automatic-images-classifier-generator)和 [SerpApi 的 Google Images Scraper API](https://serpapi.com/images-results) 。
虽然[生成器](https://github.com/serpapi/automatic-images-classifier-generator)不支持图像以外的格式，但是可以利用图像格式的纸质表格或纸质文档来进一步增强文本数据。以下是一个查询示例:

[游乐场链接查询](https://serpapi.com/playground?q=Images+with+Plato+Quotes&location=Austin%2C+Texas%2C+United+States&gl=us&hl=en&tbm=isch&no_cache=true&engine=google&google_domain=google.com&newPara=stick+highlight+lr+async+as_qdr+safe+engine+google_domain+si+form+filter+chips)

使用诸如“带有柏拉图语录的图像”的查询，您可以通过利用光学字符识别(OCR)、智能字符识别(ICR)来增强关于名人语录的数据集，并为社交媒体上的自动语录发布机器人创建流程自动化服务。

在没有给出明确模板的情况下，通过利用 SerpApi 提供的自动数据采集方法，您可以创建各种产品，例如改进条形码扫描仪的分类模型、旧的纸质文档管理工具、可以取代 RFID 阅读器库存或光学标记识别的下一代图像识别应用程序、可以取代手动数据采集的医疗保健行业自动数据采集工具、减少政府干预私人数据的公开个人数据采集软件、提供比利用不同形式数据采集方法的链接更多数据的 QR 码扫描仪等。SerpApi 可以提供的数据类型有很多可能性。

# 自动化模型数据分析

因为数据收集过程是通过 SerpApi 的 Google Images Scraper API 完成的，所以我们通过使用如下查询为已经预处理的图像收集数据:

[游乐场链接查询](https://serpapi.com/playground?q=dog+imagesize%3A500x500&location=Austin%2C+Texas%2C+United+States&gl=us&hl=en&tbm=isch&no_cache=true&engine=google&google_domain=google.com&newPara=stick+highlight+lr+async+as_qdr+safe+engine+google_domain+si+form+filter+chips)

数据质量的提高直接影响到非结构化算法的交叉比较，并减少了对人工干预的需求。
下面的细节就是一个很好的例子。我们正在创建一个具有多个 Conv2d 和 Maxpool2d 层的 CNN(卷积神经网络)。我们想要计算第一个完全连接的层的输入大小。为了对矩形图像进行这种处理，我们需要分别计算高度和宽度的空间分辨率。稍后在发电机上展开的东西。现在，我们将使用以前收集的 500x500 图像，并对每个卷积层和池层应用方形内核。下面是嵌入了公式的简单易懂的函数:

让我们声明我们想要测试的不同模型，在列表中对这些图像进行分类。现在，我只放一个模型进去:

让我们在另一个列表中声明我们想要测试的不同优化器:

现在，我们将只测试 AdamW，唯一的参数变化将是学习率。

接下来，我们将声明学习率的范围和步进间隔:

学习率的范围从 0.001 到 1.0，增量为 0.001。

还有，我们需要声明一个损失函数列表。我们只声明一个:

我们还需要一个计数器来计算我们将要采取的每个迭代动作:

我们可以从模型中取出下一部分。但是 output_size 代表模型中最后一个 Conv2d 层的输出大小。这将有助于计算完全连接的线性输入大小。

我们将把 500x500 的图像尺寸减小到 32x32，以节省处理能力。同样，我们可以从训练字典声明中获取。它将被用作 calculate_fully_connected 函数的第二个参数。

让我们声明一个空数组来存储自动训练命令字典:

让我们开始遍历不同的列表。由于除了 lr_range 之外，每个列表中只有一个项目，因此该迭代将使用损失函数 PoissonNLLLoss 并使用所描述的 CNN 模型来测试优化器 AdamW 在特定时期大小上的最佳学习率。

让我们使用计数器给每个模型一个不同的名称:

下一步是计算完全连接的图层的输入大小。请注意，这不是要使用的大小，而是寻找它的变量。我们将使用我们在模型列表中声明的 CNN 模型和图像大小。由于我们使用 32x32 的图像，以及 5x5 和 2x2 的内核，我们可以安全地计算一个数字，并将其用于高度和长度:

然后，我们需要用计算值更改完全连接层的输入大小:

如您所见，最终结果将是计算出的数字的平方乘以输出大小。

现在我们需要用我们正在迭代的变量来声明训练字典:

让我们将训练词典添加到 training_dicts 中，以便将它们收集到一个列表中:

最后，我们递增计数器:

对于下一部分，您需要运行自动图像分类器生成器，以便为自动训练进行必要的调用:

对于每个训练字典，我们将向训练端点发送一个请求来训练模型，检查它是否在 find_attempt 完成了训练，使用来自测试端点提供的标签的 100 个随机图像来测试它，然后再次检查测试是否结束。最后，我们将把每个自动化过程的所有训练和测试数据存储在一个名为 results 的列表中。

最后，为了找到最有效的设置，我们需要做的就是最精确地检查过程。最后，我们将为用户打印最准确的设置:

你可以在下面的要点中找到完整的代码:

**结论**

我感谢读者的关注，也感谢塞尔帕皮的[精英们的支持。在接下来的几周里，我们将针对需要各种图像分类模型的不同类型的任务分析不同的模式，讨论如何将旧模型的学习经验转移到临时创建的新模型，并利用损失数据来衡量训练表现。最后，我们将以命令行工具的形式将其最小化，使其对公众有用。](https://serpapi.com/team)

*原载于 2022 年 9 月 1 日*[](https://serpapi.com/blog/automatic-data-collection-for-machine-learning-models/)**。**