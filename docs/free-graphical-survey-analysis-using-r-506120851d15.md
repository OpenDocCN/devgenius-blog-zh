# 使用 R 的免费图形调查分析

> 原文：<https://blog.devgenius.io/free-graphical-survey-analysis-using-r-506120851d15?source=collection_archive---------1----------------------->

![](img/174bcd7aa8e3bf7cc3d6704abbe9f031.png)

当我想在我的学士论文范围内对调查进行图形分析时，我遇到了很多程序都不免费的问题，比如 SPSS([https://www.ibm.com/products/spss-statistics](https://www.ibm.com/products/spss-statistics))。我用 limesurvey 做了调查，结果图形没有达到我的预期，尤其是矩阵问题。为此，我使用 RStudio 编写了一个简单的 R 脚本，以图形化方式显示单个答案、多个答案、数字答案和矩阵答案。

我用箱线图展示了数字响应，其他的用柱状图展示。在下文中，我将讨论不同的函数，并更详细地解释它们的参数。您当然可以根据自己的喜好修改代码，添加或删除参数，并为情节添加额外的内容。

# **设立项目**

首先，在 RStudio 中打开一个新文件，并将其命名为，例如“my_r_script.r”。如果你没有 RStudio，可以免费下载 R 和 RStudio。你可以在 https://www.rstudio.com/products/rstudio/download/的[找到如何做的说明。注意，你需要下载 R(](https://www.rstudio.com/products/rstudio/download/)[https://www.r-project.org](https://www.r-project.org))和 RStudio。

在使用这些功能之前，您需要添加 2 个库，这些库包含附加的预定义功能，您可以轻松地将这些功能添加到您的项目中，加载您的调查结果数据并定义您的样本大小。

以下代码片段安装名为 ggplot2 的外部库，这是一个用于以图形方式绘制数据的库。此外，它还初始化下载的 ggplot2 包和 plyr，plyr 是设置矩阵问题结果所需的包。然后，我们将数据以. csv 文件的形式加载到项目中，并定义样本大小 n。

```
## install packages
install.packages("ggplot2")## load libraries
library(ggplot2)
library(plyr)## get data
data <- read.csv("<PATH_TO_YOUR_DATA>")## define constants
# sample size
n <- <YOUR_SAMPLE_SIZE>
```

您的调查数据路径(<path_to_your_data>)可能如下所示:</path_to_your_data>

**" ~/下载/ <文档名称> "**

不要忘记在文件名中添加文件类型，例如。csv”。

# **设置功能**

如上所述，该脚本包含 4 种以图形方式显示答案的不同可能性(单个答案、多个答案、矩阵答案、数字答案)。因此，有 4 种不同的功能，它们相似但不相同。在下文中，每个功能被单独示出并通过示例进行解释。

## **1。单项回答**

单项问题描述只允许一个答案选项的问题。RStudio 中的函数如下所示:

```
chartSingleAnswer <- function (vector, labels, title, question_index, xlab = "", ylab = "Percent %", fill = "") {
  rv <- c()
  for(l in labels) {
    rv = append(rv, length(vector[vector == l]) / n * 100)
  } l <- factor(labels, levels = labels)
  df <- data.frame(l, rv) ggplot(df, aes(x = forcats::fct_rev(l), y = rv)) + geom_bar(stat = "identity", aes(fill=l), position="dodge") + ggtitle(title) + xlab(xlab) + ylab(ylab) + labs(fill=fill) + coord_flip()

  #image_name <- paste(question_index, ".jpg")
  #ggsave(image_name, dpi = 600)
}
```

该函数名为“chartSingleAnswer”，包含 7 个参数。“向量”是一个包含这个问题答案的向量。“标签”是应该显示为图例的标签。请注意，您应该按照答案选项的名称来选择标签。例如，如果是一个是非问题，将标签命名为“是”和“否”。此外，确保标签区分大小写。标题、xlab 和 ylab 参数仅用于标签。“question_index”用于索引问题，以便按时间顺序存储它们。

该函数首先创建一个空向量，然后遍历标签。然后根据标签过滤出答案。基于此，计算长度并除以样本大小 n，然后乘以 100。这得出了回答的百分比。为了清楚地说明，我们创建了因子“l ”,并基于此创建了名为“df”的数据帧。以下 ggplot 函数用于说明。可以简单的复制一下。如果你不理解函数的摘录，在谷歌上键入它们，它们实际上只是为了正确的图形表示。

如果您想在本地保存输出图，取消最后两行的注释。

要执行该函数，您的代码可能如下所示:

```
vector <- data$<COLUMN_NAME>
labels <- c("Yes", "No")
chartSingleAnswer(vector, labels, "<YOUR_TITLE>", "<INDEX>")
```

## 2.多重答案

多选题描述的是回答者可以选择一个或多个答案选项的问题。RStudio 中的函数如下所示:

```
chartMultipleAnswer <- function (vectors, labels, title, question_index, xlab = "", ylab = "Percent %", fill = "") {
  rv <- c()
  for(v in vectors) {
    rv = append(rv, length(v[v == "Yes"]) / n * 100)
  } df <- data.frame(x = labels, percent = rv) ggplot(df, aes(x = forcats::fct_rev(labels), y = percent)) + geom_bar(stat = "identity", aes(fill = labels)) + ggtitle(title) + xlab(xlab) + ylab(ylab) + labs(fill=fill) + coord_flip()

  #image_name <- paste(question_index, ".jpg")
  #ggsave(image_name, dpi = 600)
}
```

该函数类似于单个响应函数，但采用一组向量作为参数。它循环遍历这些向量，并检查哪些向量已经被检查过。看看你的数据集中的标签是什么，在我的例子中，检查过的问题被简单地标记为“是”。这些数字，即肯定的数量，再次除以 n 并乘以 100 以得到百分比，然后加到向量 rv 上。之后，创建一个数据帧，ggplot 函数负责图形表示。如果希望将图形直接保存到本地设备，请再次取消最后两行的注释。

要执行该函数，您的代码可能如下所示:

```
vector1 <- data$<COLUMN1_NAME>
vector2 <- data$<COLUMN2_NAME>vectors_list <- list(vector1, vector2)
labels <- c("Cat", "Dog", "Fish")chartMultipleAnswer(vectors_list, labels, "<YOUR_TITLE>", "<INDEX>")
```

## 3.矩阵答案

矩阵答案描述了以矩阵形式给出答案的问题。例如，“在 1-5 分(其中 1 分非常正确)中，您对以下术语的认同程度如何？”
r studio 中的函数如下所示:

```
chartMatrixAnswer <- function(vectors, labels, levels, types, title, question_index, xlab = "", ylab = "Percent %", fill = "") {
  rv <- c()
  for(vector in vectors) {
    inner_vector <- c()
    for(i in 1:length(levels)) {
      inner_vector = append(inner_vector, length(vector[vector == levels[i]]) / n * 100)
    }
    rv = append(rv, inner_vector)
  } l <- factor(levels, levels = levels)
  df <- data.frame(l, rv) ggplot(df, aes(x = forcats::fct_rev(l), y = rv)) + geom_bar(stat = "identity", position="dodge", aes(fill = types)) + ggtitle(title) + xlab(xlab) + ylab(ylab) + labs(fill=fill) + coord_flip() + guides(fill = guide_legend(reverse = TRUE))#image_name <- paste(question_index, ".jpg")
  #ggsave(image_name, dpi = 600)
}
```

该函数再次采用向量列表(list(vector1，vector2，..))作为参数。这次必须循环两次。这意味着循环遍历向量列表，并根据级别循环每个向量，在我们的例子中，级别是 1-5(根据批准级别)。这导致肯定的百分比取决于同意的程度。再次注意将级别命名为与数据集中的答案相同。之后，我们像往常一样创建一个因子和一个数据框架，然后在 ggplot 的帮助下创建我们的图表。如果您想在本地机器上直接保存图形，请取消最后两行的注释。

要执行该函数，您的代码可能如下所示:

```
vector1 <- data$<COLUMN1_NAME>
vector2 <- data$<COLUMN2_NAME>vectors_list <- list(vector1, vector2)
levels <- c("Very agree", "Agree", "Don't agree")
labels <- c("Fish taste good", "Beans taste good", "Pizza tastes good")
types <- c(rep("Fish taste good", length(levels)), rep("Beans taste good", length(levels)), rep("Pizza tastes good", length(levels)))chartMatrixAnswer(vectors_list, labels, levels, types, "<YOUR_TITLE>", "<INDEX>")
```

## 4.数字回答

数字答案来自以任何数字作为输入的开放式问题。一个例子是“你多大了？”。R-Studio 中的函数是这样的:

```
chartBoxplot <- function(vector, title, question_index, ylab="", xlab="") {
  v <- vector[!is.na(vector)]
  v = data.frame(v) ggplot(v, aes(x="", y=v)) + ylab(ylab) + ggtitle(title) +   xlab(xlab) + geom_boxplot() + coord_flip() #image_name <- paste(question_index, ".jpg")
  #ggsave(image_name, dpi = 600) #summary(vector)
}
```

该函数将一个向量作为参数，其中这是年龄问题的响应向量。向量针对未填写的问题进行调整(N/A =无答案),然后表示为数据帧。然后 ggplot 库会处理剩下的事情。请注意，这里必须使用 geom_boxplot()，而不是使用 geom_bar()，它负责以条形图的形式显示图形。取消对倒数第二行的注释，将图形直接保存在本地机器上。如果您想要数据汇总表，请取消最后一行的注释。这包含像向量的最大值、最小值、平均值和中值这样的信息。

```
data <- data$<COLUMN_NAME>
chartBoxplot(data, "<YOUR_TITLE>", "<Y_LAB>", question_index = "<INDEX>")
```

# 摘要

如果你有一些关于 R 的先验知识，这是一个基于调查结果绘制免费的、清晰的、详细的和可定制的图表的简单方法。如果你想创建图形，ggplot 库是非常有用的，它也提供了许多修改的可能性。如果你想改变颜色或其他视觉元素，只需谷歌一下，你一定会找到你的问题的答案，因为你只需要改变，添加或删除一个参数的 ggplot 功能。

如果你喜欢这篇文章，或者你注意到了别的什么，请随时给我反馈:)