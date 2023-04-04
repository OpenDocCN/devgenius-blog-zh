# 堆栈溢出年度开发者调查 2022

> 原文：<https://blog.devgenius.io/stack-overflow-annual-developer-survey-2022-6d22485f243a?source=collection_archive---------5----------------------->

> 使用 Python，Seaborn，Matplotlib，Pandas

![](img/d8a706ff5b73a3efb026cd0613228e51.png)

这个项目的目标是提供更好地理解开发者社区的见解。对于新的和有经验的开发者来说，这个资源对于理解这个行业的前景是至关重要的。在调查中，您可以根据国家、年龄以及回答是来自学习者还是专业开发人员来筛选回答数据。它也非常适合与不断变化的技术趋势保持同步！

对于新的个人学习者，我认为有必要分享这些统计信息和关键的要点，因为他们最有可能对技术世界是全新的。

用于该分析的数据集取自[](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2F)*。你可以找到很棒的数据集来使用。Kaggle 的社区由来自世界各地的数据科学家和机器学习者组成，他们拥有各种技能和背景。我们将在这个 [*栈溢出开发者调查 2022*](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fdatasets%2Fdheemanthbhat%2Fstack-overflow-annual-developer-survey-2022) 中涵盖所有的内容，并将询问多个问题，如什么是流行的语言，世界各地的开发者获得多少报酬，以及他们使用哪些语言？还有很多其他的东西。请继续关注更多信息。*

*本项目中使用的数据分析工具有 [*Numpy*](https://jovian.ai/outlink?url=https%3A%2F%2Fnumpy.org%2F) 和 [*Pandas*](https://jovian.ai/outlink?url=https%3A%2F%2Fpandas.pydata.org%2F) 包，并对数据进行可视化和浏览: [*Matplotlib*](https://jovian.ai/outlink?url=https%3A%2F%2Fmatplotlib.org%2Fstable%2Fgallery%2Findex) 和 [*Seaborn*](https://jovian.ai/outlink?url=https%3A%2F%2Fseaborn.pydata.org%2F) 。*

*我们开始吧！*

> *1.下载数据集*

*如前所述，我们将从 Kaggle 下载数据集。我们这里用的数据集是 [*栈溢出开发者调查 2022*](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fdatasets%2Fdheemanthbhat%2Fstack-overflow-annual-developer-survey-2022) 。他们在年度调查中提出的问题有助于他们改善 Stack Overflow 社区和为他们服务的平台。*

*他们面临的挑战和机遇是继续扩展和提高帮助所有开发人员的能力，并让他们在自己的社区中感到受欢迎。*

*将数据集导入 Jupyter 有几种选择:*

*   *手动下载 CSV 并通过 Jupyter 的 GUI 上传*
*   *使用 urllib.request 中的 urlretrieve 函数从原始 URL 下载 CSV 文件*
*   *使用一个帮助程序库，例如 opendatasets，它包含一个精选数据集的集合，并提供一个用于直接下载的帮助程序功能。*

*我们将使用 opendatasets helper 库来下载文件。*

*让我们从下载数据开始，列出数据集中的文件。*

*数据集已下载并提取。*

*你可以使用 Jupyter 中的“文件”>“打开”菜单选项浏览下载的文件。数据集似乎包含两个文件:*

*   *`survey_results_schema.csv` -问题列表，以及每个问题的简码*
*   *`survey_results_public.csv` -问题回答的完整列表*

*让我们使用 Pandas 库加载 CSV 文件。我们将使用数据框的名称`survey_raw_df`来表示这是未处理的数据，我们可能会对其进行清理、过滤和修改，以准备用于分析的数据框。*

*正如我们所看到的，该调查有超过 73000 名受访者，提出了大约 75 个问题(尽管许多问题是可选的)。让我们查看数据框中的列列表。*

*似乎问题的短代码已被用作列名。*

*我们可以参考模式文件来查看每个问题的全文。模式文件只包含两列:`Column`和`QuestionText`。我们可以把它作为熊猫系列加载，用`Column`作为索引，用`QuestionText`作为值。*

> *2.数据准备和清理*

*数据准备是通过在处理和分析之前清理和转换原始数据来准备数据的过程。这是处理之前的一个重要步骤，通常涉及重新格式化数据、对数据进行更正以及合并数据集以丰富数据。*

*数据清理是修复或删除数据集中不正确、损坏、格式不正确、重复或不完整的数据的过程。当组合多个数据源时，有很多机会出现数据重复或标签错误的情况。如果数据不正确，结果和算法是不可靠的，即使它们看起来是正确的。*

*虽然调查反馈包含丰富的信息，但我们将把分析限制在以下几个方面:*

*   *调查受访者和全球编程社区的人口统计数据*
*   *编程技能、经验和偏好的分布*
*   *与就业相关的信息、偏好和意见*

*让我们选择包含相关数据的列子集进行分析。*

*让我们从这些列中提取一份数据到一个新的数据框`survey_df`中。我们可以在不影响原始数据帧的情况下继续进一步修改。*

*让我们来查看一些关于数据框的基本信息。*

*大多数列都具有数据类型，因为它们包含不同类型的值或者包含空值(`NaN`)。似乎每一列都包含一些空值，因为每一列的非空计数都低于总行数(73268)。我们需要处理空值，并根据具体情况手动调整每一列的数据类型。*

*只有一列被检测为数字列(`WorkExp`)，尽管其他几列主要是数字值。为了使我们的分析更容易，让我们将其他一些列转换成数字数据类型，同时忽略任何非数字值。非数字被转换为`NaN`。*

*性别栏也允许选择多个选项。我们将删除包含多个选项的值，以简化我们的分析。*

> *3.探索性分析和可视化*

*在我们就调查回答提出问题之前，先了解一下受访者的人口统计数据，如国家、年龄、性别、教育水平、就业水平等，会有所帮助。有必要探究这些变量，以了解该调查在全球编程社区中的代表性。这种规模的调查通常会有一些选择偏差。*

*先从导入`matplotlib.pyplot`和`seaborn`开始。*

## *国家*

*让我们来看看调查中有多少个国家作出了回应，并标出回应数量最多的十个国家。*

*我们可以使用`value_counts`方法确定受访者人数最多的国家。*

*我们可以用条形图来显示这些信息。*

*来自美国和印度的受访者似乎比例过高，这可能是因为调查是用英语进行的，而这些国家说英语的人口最多。我们已经可以看到，该调查可能并不代表全球编程社区——尤其是来自非英语国家的社区。几乎可以肯定，来自非英语国家的程序员人数不足。*

## *年龄*

*受访者的年龄分布是另一个需要考虑的关键因素。我们可以用直方图来形象化它。*

## *性别*

*让我们来看看性别的反应分布。众所周知，女性和非二进制性别在编程社区中的代表性不足，因此我们可能会看到这种不均衡的分布。*

*饼图是可视化分布的好方法。*

*只有大约 8%回答这个问题的受访者认为自己是女性或者是非二元的。这个数字低于编程社区中女性(非二元性别)的总百分比，估计约为 12%。*

## *教育水平*

*计算机科学的正规教育通常被认为是成为程序员的基本要求。然而，网上有很多免费的资源和教程可以用来学习编程。让我们比较一下受访者的教育水平，以便对此有所了解。这里我们将使用一个水平条形图。*

*似乎超过一半的受访者拥有学士或硕士学位，所以大多数程序员似乎都受过一些大学教育。然而，仅从这张图表中还不清楚他们是否拥有计算机科学学位。*

*该图目前显示了每个选项的受访者人数。我们将修改它来显示百分比。*

*似乎超过 40%的回答者拥有学士学位，所以大多数程序员似乎都受过大学教育。*

## *雇用*

*自由职业者或合同工是程序员的常见选择，所以比较全职、兼职和自由职业者的细分会很有趣。让我们可视化来自`Employment`列的数据。*

*`DevType`字段包含关于回答者所担任角色的信息。由于该问题允许多个答案，所以该列包含由分号`;`分隔的值列表，这使得直接分析有点困难。*

*让我们定义一个助手函数，它将包含值列表的列(如`survey_df.DevType`)转换成一个数据框，每个可能的选项对应一列。*

*在`dev_type_df`中，可以选择的每个选项都有一列作为响应。如果回答者选择了一个选项，相应列的值为`True`。否则就是`False`。*

*现在，我们可以使用列汇总来确定最常见的角色。*

*正如所料，最常见的角色名称中包含“开发人员”。让我们将数据可视化，并检查对社区做出贡献的前 10 个角色的百分比。*

> *4.提问和回答问题*

*通过研究数据集的各个列，我们已经获得了关于回答者和编程社区的一些见解。让我们提出一些具体的问题，并尝试使用数据框操作和可视化来回答这些问题。*

*Q1:2022 年最流行的编程语言是什么？*

*要回答这个问题，我们可以使用`LanguageHaveWorkedWith`列。与`DevType`类似，回答者在这里被允许选择多个选项。*

*首先，我们将把这一列分割成一个数据框，其中包含选项中列出的每种语言的一列。*

*似乎共有 42 种语言包含在选项中。让我们汇总这些数据，以确定选择每种语言的受访者的百分比。*

*我们可以用一个水平条形图来表示这一信息。*

*也许毫不奇怪，Javascript & HTML/CSS 名列榜首，因为 web 开发是当今最受欢迎的技能之一。这也恰好是最容易开始的领域之一。SQL 是使用关系数据库所必需的，所以大多数程序员经常使用 SQL 也就不足为奇了。Python 似乎是其他开发形式的流行选择，击败了 Java，后者是二十多年来服务器和应用程序开发的行业标准。令人惊讶的是，TypeScript 已经获得了第五名，毫无疑问它越来越受欢迎了。*

*Q2:在印度，开发人员最常用的语言是什么？*

*正如我们所看到的，结果没有太大的差别，尽管 JAVA 已经跃升到第五位，将 TypeScript 甩在后面，这意味着与我们在世界范围内获得的结果相比，印度人在 JAVA 上做了更多的工作。*

*问题 3:在数据科学相关领域工作的受访者中，最常用的语言是什么？*

*正如我们从图中看到的，所有开发人员社区使用的语言和只有数据科学家和机器学习专家使用的语言存在巨大差异，毫无疑问，Python、SQL 和 R 占据了前 3 名。*

*问题 4:在接下来的一年里，人们最有兴趣学习哪种语言？*

*为此，我们可以使用`LanguageWantToWorkWith`列，进行与问题 1 类似的处理。*

*JavaScript 和 Python 再次占据前 2 名，TypeScript 紧随其后。人们更热衷于使用 web 框架，Python 是对数据科学和机器学习更感兴趣的人的选择。*

*q5:2022 年数据库最流行的语言是什么？*

*要回答这个问题，我们可以使用`DatabaseHaveWorkedWith`列。与`DevType`类似，回答者可以在这里选择多个选项。*

*[](https://jovian.ml/itssouravshrivas/stack-overflow-annual-developer-survey-2022-exploratory-data-analysis/v/27&cellId=129) [## itsouravshrivas/stack-overflow-annual-developer-survey-2022-explorative-data-analysis

### 与它的 souravshrivas 合作编写 stack-overflow-annual-developer-survey-2022-explorative-data-analysis 笔记本。

jovian.ml](https://jovian.ml/itssouravshrivas/stack-overflow-annual-developer-survey-2022-exploratory-data-analysis/v/27&cellId=129) 

这说明 MySQL 仍然是数据库的首选，其次是 PostgreSQL 和 SQLite。

> 推论和结论

我们从调查中得出了许多推论。以下是其中一些的摘要:

*   根据调查受访者的人口统计数据，我们可以推断该调查在一定程度上代表了整个编程社区。然而，它在非英语国家的程序员和女性&非二元性别中的反应较少。
*   编程社区并不像它应该的那样多样化。虽然情况正在改善，但我们应该做出更多的努力来支持和鼓励那些在年龄、国家、种族、性别或其他方面代表性不足的社区。
*   虽然大多数程序员拥有大学学位，但是相当大比例的人只有学士学位，这确实向我们表明，为了消除疑虑或进行专业编程，没有必要拥有硕士学位。
*   社区中很大一部分人是全职工作者，其次是学生。
*   Javascript & HTML/CSS 是 2022 年使用最多的编程语言，紧随其后的是 SQL & Python。
*   JavaScript 和 Python 是大多数人有兴趣学习的语言——因为它是一种简单易学的通用编程语言，非常适合各种领域。
*   数据科学或机器学习人员大多使用 Python、SQL 和 R，因为它们涉及对现实世界问题的更深入分析。
*   来自印度的程序员和来自世界各地的程序员使用的语言没有太大的区别。印度社区似乎更多地使用 JAVA 而不是 TypeScript。
*   MySQL、PostgreSQL 和 SQLite 是数据库的三大语言。

> 参考

查看以下资源，了解有关本笔记本中使用的数据集和工具的更多信息:

*   堆栈溢出开发者调查 2022:[https://survey.stackoverflow.co/2022/](https://jovian.ai/outlink?url=https%3A%2F%2Fsurvey.stackoverflow.co%2F2022%2F)
*   熊猫用户指南:[https://pandas.pydata.org/docs/user_guide/index.html](https://jovian.ai/outlink?url=https%3A%2F%2Fpandas.pydata.org%2Fdocs%2Fuser_guide%2Findex.html)
*   对 EDA 有帮助:[https://real python . com/pandas-python-explore-dataset/# visualizing-your-pandas-data frame](https://jovian.ai/outlink?url=https%3A%2F%2Frealpython.com%2Fpandas-python-explore-dataset%2F%23visualizing-your-pandas-dataframe)
*   Matplotlib 用户指南:[https://matplotlib.org/3.3.1/users/index.html](https://jovian.ai/outlink?url=https%3A%2F%2Fmatplotlib.org%2F3.3.1%2Fusers%2Findex.html)
*   Seaborn 用户指南&教程:[https://seaborn.pydata.org/tutorial.html](https://jovian.ai/outlink?url=https%3A%2F%2Fseaborn.pydata.org%2Ftutorial.html)
*   `opendatasets` Python 库:[https://github.com/JovianML/opendatasets](https://jovian.ai/outlink?url=https%3A%2F%2Fgithub.com%2FJovianML%2Fopendatasets)
*   Stack Overflow 开发者调查 2022 数据集:[https://www . ka ggle . com/datasets/dheemanthbhat/stack-Overflow-annual-Developer-Survey-2022？select = survey _ results _ schema . CSV](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fdatasets%2Fdheemanthbhat%2Fstack-overflow-annual-developer-survey-2022%3Fselect%3Dsurvey_results_schema.csv)*