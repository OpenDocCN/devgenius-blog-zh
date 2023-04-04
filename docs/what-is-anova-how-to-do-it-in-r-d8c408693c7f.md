# 什么是方差分析？R 里怎么做？

> 原文：<https://blog.devgenius.io/what-is-anova-how-to-do-it-in-r-d8c408693c7f?source=collection_archive---------2----------------------->

ANOVA(方差分析)是一种统计方法，用于检验两组或更多组的平均值之间是否存在显著差异。它用于确定不同组的平均值之间是否存在显著差异，或者组之间的差异是否仅仅是由于随机机会。例如，方差分析可用于比较不同组在测试中的平均分数，以查看他们的表现是否有显著差异。方差分析包括比较每组内的方差和组间的方差。如果组间方差远大于组内方差，则各组均值之间可能存在显著差异。

方差分析有不同的类型，包括单向方差分析、双向方差分析和重复测量方差分析，具体取决于要解决的具体研究问题和研究设计。

1.  单向方差分析:这种类型的方差分析比较两个或多个独立组的平均值。例如，研究人员可能使用单向方差分析来比较三个不同教室的学生在一次测试中的平均分数。
2.  双向方差分析:当有两个独立变量(也称为因子)时，这种类型的方差分析比较两个或更多组的平均值。例如，研究人员可以使用双向方差分析来比较不同教室的学生在一次测试中的平均分数，同时考虑性别(男性对女性)和教学方法(讲座对讨论)的影响。
3.  重复测量 ANOVA:这种类型的 ANOVA 用于在多次测量相同受试者(例如，干预前后)时比较两组或更多组的平均值。重复测量方差分析用于确定各组的均值是否随时间推移而存在显著差异。
4.  混合方差分析:当同时存在受试者内变量(如时间)和受试者间变量(如治疗)时，这种类型的方差分析用于比较两组或更多组的平均值。混合方差分析允许研究人员检查受试者内部和受试者之间变量的相互作用。

在 R 中，有几个函数可用于执行不同类型的方差分析。以下是主要功能的总结:

1.  **单向方差分析:**`aov`函数可用于在 r 中进行单向方差分析，例如:

```
aov_result <- aov(y ~ x, data=mydata)
```

其中`y`是因变量，`x`是自变量。ANOVA 测试的结果可以使用`summary`功能获得:

```
summary(aov_result)
```

2.**双向方差分析:**`aov`功能也可用于在 r 中进行双向方差分析。例如:

```
aov_result <- aov(y ~ x1 + x2, data=mydata)

or 

model <- aov(y ~ x1*x2, data =mydata)
```

其中`y`为因变量，`x1`为第一自变量，`x2`为第二自变量。ANOVA 测试的结果可以使用如上所示的`summary`功能获得。

3.**重复测量方差分析:**`ez`包中的`ezANOVA`函数可用于在 r 中执行重复测量方差分析。例如:

```
# Load the data
data <- read.csv("experiment_data.csv")

# Fit the model
library(ez)
model <- ezANOVA(data=data, dv=response, within=factor, between=NULL, type=3, return_aov=TRUE)

# View the summary of the model
summary(model)
```

在本例中，`response`是响应变量(即因变量)，而`factor`是重复测量(即受试者内因素)。`ez`包中的`ezANOVA`函数用于拟合模型，而`summary`函数用于查看模型摘要。

或者，您可以使用`aov`函数并将`Error`项指定为重复测量。例如:

```
# Load the data
data <- read.csv("experiment_data.csv")

# Fit the model
model <- aov(response ~ factor + Error(subject/(factor)), data=data)

# View the summary of the model
summary(model)
```

在本例中，`response`是响应变量，`factor`是重复测量，`subject`是主题标识符。`aov`功能用于拟合模型，`summary`功能用于查看模型摘要。`Error`术语用于指定重复测量。

4.**混合方差分析:**`nlme`包中的`lme`函数可用于在 r 中执行混合方差分析。例如:

```
library(nlme)
aov_result <- lme(y ~ x1 + x2, random = ~ 1 | subject, data=mydata)
```

其中`y`为因变量，`x1`和`x2`为固定效应，`subject`为随机效应，`1`指定截距。ANOVA 测试的结果可以使用`Anova`功能获得:

```
library(car)
Anova(aov_result)
```

**R 农业设计分析代码:**

1.  **完全随机设计:**完全随机设计(CRD)是一种实验设计，其中将处理(即自变量)随机分配给实验单元(例如，地块、动物、人)。在 CRD 中，不试图控制可能影响响应变量(即因变量)的潜在混杂变量。CRD 的主要优点是简单、易于实现，并且比其他实验设计需要更少的资源。然而，主要的缺点是它倾向于更大的可变性，这使得检测治疗之间微小但真实的差异变得更加困难。CRD 可用于各种研究领域，包括农业科学、生物学和心理学。当目的是测试治疗的总体效果，而不是比较特定的组或调查变量之间的相互作用时，它特别有用。为了分析来自 CRD 的数据，可以使用 ANOVA。在 R 中，`stats`包中的`aov`函数可用于将 ANOVA 模型与来自 CRD 的数据进行拟合。例如:

```
# Load the data
data <- read.csv("agriculture_data.csv")
```

```
# Fit the model
model <- aov(yield ~ treatment, data=data)
# View the summary of the model
summary(model)
# Perform a post-hoc test to compare the means of the treatment groups
library(agricolae)
TukeyHSD(model)
```

在本例中，`yield`为响应变量(即因变量)，而`treatment`为多水平的自变量(如不同施肥处理)。使用`aov`函数来拟合模型，使用`agricolae`包中的`summary`和`TukeyHSD`函数来查看模型摘要，并进行事后检验，以比较治疗组的平均值。

**2。随机完全区组设计:**随机完全区组设计(RCBD)是一种实验设计，其中将处理(即独立变量)随机分配给每个区组内的实验单元(如地块、动物、人)。一个区块是一组在某些方面相似的实验单元(例如，它们有相似的土壤类型、相似的年龄等)。).使用区块的目的是控制任何可能影响响应变量(即因变量)的潜在混杂变量。RCBD 的关键特征是在每个区块内随机分配处理，这有助于减少区块内的可变性并提高处理效果估计的精确度。这使得检测治疗之间微小但真实的差异变得更加容易。

RCBD 可用于各种研究环境。当存在可能影响响应变量且无法完全控制的已知变异源时，这种方法特别有用。在 R 中，`stats`包中的`aov`函数可用于将 ANOVA 模型与来自 RCBD 的数据进行拟合。例如:

```
# Load the data
data <- read.csv("agriculture_data.csv")
```

```
# Fit the model
#you can replace the "+" sign with "*" for interaction effect
model <- aov(yield ~ treatment + block, data=data)

# View the summary of the model
summary(model)
# Perform a post-hoc test to compare the means of the treatment groups
library(agricolae)
TukeyHSD(model)
```

在本例中，`yield`为响应变量，`treatment`为多水平因子(如不同施肥处理)，而`block`为阻断变量。`aov`函数用于拟合模型，而`agricolae`包中的`summary`和`TukeyHSD`函数用于查看模型摘要并执行事后测试，以比较治疗组的平均值。

3.**析因设计:**析因设计是一种同时研究多个因素(即自变量)的实验设计。每个因素都有多个级别，所有可能的级别组合都经过测试。这使得研究人员可以调查每个因素的主要影响以及这些因素之间对响应变量(即因变量)的相互作用。

```
# Load the data
data <- read.csv("agriculture_data.csv")
```

```
# Fit the model
model <- aov(yield ~ treatment1 * treatment2, data=data)
```

```
# View the summary of the model
summary(model)
```

```
# Perform a post-hoc test to compare the means of the treatment groups
library(agricolae)
TukeyHSD(model)
```

在本例中，`yield`是响应变量，`treatment1`和`treatment2`是多层次的因子(如不同的施肥和灌溉处理)。`aov`函数用于拟合模型，而`agricolae`包中的`summary`和`TukeyHSD`函数用于查看模型摘要并执行事后测试，以比较治疗组的平均值。

4.**裂区设计:**裂区设计是一种实验设计，其中一个因素(即主要情节因素)应用于整个情节水平，而另一个因素(即次要情节因素)应用于次要情节水平。主要情节因素通常是应用于整个情节的处理，而次要情节因素是应用于部分情节的处理。

裂区设计的主要优点是，它允许研究者研究主要情节因素和次要情节因素的主要影响，以及这些因素之间的相互作用，同时控制任何可能影响响应变量(即因变量)的潜在混杂变量。

```
# Load the data
data <- read.csv("agriculture_data.csv")
```

```
# Fit the model
model <- aov(yield ~ main_plot*subplot + Error(subplot), data=data)
or 
model <- aov(yield ~ main_plot*sub_plot, data= data)
```

```
# View the summary of the model
summary(model)
```

```
# Perform a post-hoc test to compare the means of the treatment groups
library(agricolae)
TukeyHSD(model)
```

**事后测试**

在进行全面测试(例如 ANOVA)后，可以使用不同的事后测试来比较特定组的平均值。事后检验，也称为多重比较检验或多重比较程序，是一种统计检验，用于在进行 ANOVA 检验后比较特定组的平均值。事后检验的目的是确定哪些组之间存在显著差异，并确定这些差异的性质和程度。当有多个组或治疗水平进行比较时，通常使用事后检验，ANOVA 表明组间存在显著差异。如果没有事后检验，就不可能确定哪些特定群体之间存在显著差异。有几种不同类型的事后检验可用，使用哪种合适的检验取决于研究问题、数据特征和检验假设。一些常见的事后测试包括 Tukey 的 HSD 测试、Dunnett 的测试、Scheffe 的测试和 Fischer 的 LSD 测试。

1.  **Tukey 的 HSD(诚实显著差异)测试**:来自`stats`或`agricolae`包的`TukeyHSD`函数可用于在 r 中执行 Tukey 的 HSD 测试

```
# Load the data
data <- read.csv("agriculture_data.csv")
```

```
# Fit the model
model <- aov(yield ~ treatment, data=data)
# Perform the Tukey's HSD test
library(stats)
TukeyHSD(model)
```

2.**邓尼特测试**:要在 R 中执行邓尼特测试，可以使用`multcomp`包中的`dunnett.test`功能。例如:

```
# Load the data and the package
data <- read.csv("experiment_data.csv")
library(multcomp)

# Perform Dunnett's test
dunnett.test(response ~ group, data=data, control="control")
```

3.**舍夫测试**:要在 R 中执行舍夫测试，可以使用`multcomp`包中的`glht`函数。例如:

```
# Load the data and the package
data <- read.csv("experiment_data.csv")
library(multcomp)
```

```
# Fit the ANOVA model
model <- aov(response ~ group, data=data)
# Perform Scheffe's test
summary(glht(model, linfct=mcp(group="Scheffe")))
```

在这个例子中，`response`是响应变量(即因变量)，而`group`是分组变量(即有多级的自变量)。`aov`函数用于拟合 ANOVA 模型，而`glht`函数用于执行 Scheffe 检验。`linfct`参数用于指定要执行的测试类型(即 Scheffe 测试)，而`summary`函数用于查看测试结果的汇总。

4.**费歇尔最小显著差异(LSD)测试**:来自`agricolae`包的`lsd.test`函数可用于在 r

```
# Load the data and the package
data <- read.csv("experiment_data.csv")
library(agricolae)

# Fit the ANOVA model
model <- aov(response ~ group, data=data)

# Perform Fischer's LSD test
lsd.test(model, trt)
```

_____

如果这篇文章对你有帮助，你可以给我买杯咖啡:[https://www.buymeacoffee.com/sauravdastsk](https://www.buymeacoffee.com/sauravdastsk)