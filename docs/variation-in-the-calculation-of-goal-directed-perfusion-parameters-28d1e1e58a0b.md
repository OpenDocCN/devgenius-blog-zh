# 目标导向灌注参数计算中的变化

> 原文：<https://blog.devgenius.io/variation-in-the-calculation-of-goal-directed-perfusion-parameters-28d1e1e58a0b?source=collection_archive---------11----------------------->

## 分析 R 中的秒数

所以，这是我为需要统计帮助的人做的一个项目，帮助他们处理高密度的临床数据。准确的说是灌注数据。现在，这个项目的目标是看看某些测量心肺功能的方法是否会导致相同的临床决策。这意味着在用于确定临床相关性的公式中包含或排除特定参数。你不需要了解研究领域就能理解我所做的，但为了简单起见，我会添加一些图表来显示某些肺部参数、能量产生和乳酸盐之间的关系。临床灌注师的主要任务是跟踪患者的饱和水平(以及更多),以确定他/她处于有氧能量产生的状态。

![](img/94b99ed0921e338188dfe8f312d0a8a6.png)

感兴趣的参数是:

1.  氧气供应( *DO2* )
2.  耗氧量( *VO2*
3.  二氧化碳生产( *VCO2*
4.  氧提取率( *VO2* / *DO2* )
5.  呼吸系数( *VCO2* / *VO2* )

结合更多的指标，你可以跟踪一个人的能量产生和乳酸盐的存在。

![](img/36e6069d6d047b657a4b4332e3ecbb70.png)![](img/5e5f385fdc77f553488ea473badfcc0d.png)![](img/77854f82dad719ac249e003c436bff98.png)![](img/a561b940bded10553d783ae70d8e16cc.png)![](img/c37027d9e7a4176aaeec04bd2998cdc1.png)

这项研究感兴趣的公式。 **RQ** 表示呼吸系数， **BPTS** 表示体温(37℃)、环境压力和水蒸气饱和的气体。 **BSA** 指体表面积。

因为这是真实的临床数据，我不能与你分享，但我会尽我所能描述这些数据。尤其是它的粒度，因为这是这些数据最有趣的地方。我们正在处理秒钟在这里。对我来说，如何处理这种数据，是这篇博文的目的。希望对你有启发，你会喜欢。

让我们开始玩吧！

我加载了很多库，因为我想看看几个东西，在 R 中，你可以通过几种方式来看几个东西。因此，大量的图书馆。

然后我就要加载数据了，这比平常要麻烦得多，所以我需要预先指定包含的类型。我还必须转换数据，以便以更简洁的方式查看它。

这是我用来加载数据的代码，转换时间线以使它们同时开始(嗯，基本上，我只是锚定所有时间序列使它们重叠)，并应用临床阈值。这些阈值不是我选择的，而是临床决定的，临床灌注师可以据此采取行动。

![](img/12f21978b930a5d970c4c4a73476e130.png)

在这个概述中，您可以看到几乎每个感兴趣的参数都有两种方法。在这项研究中，这两种方法之间的区别是令人感兴趣的。

然而，对于这篇文章来说，我们感兴趣的不是这种区别，而是数据收集的粒度。我们在这里谈论的是以秒为单位的数据，而不是分钟、天、周、月或年。秒。

![](img/b3f46749f3e21fe933b4015d47436475.png)

如你所见，我们有 44 个病人和他们的测量数据。当然，将它们绘制在彼此之上并没有真正的帮助，因为它们不是同时开始的。所以我需要同步它们。

![](img/de44ed962f21dd76fc73b310ccff36fd.png)

现在，它们是同步的，但是对于**流**度量来说，看起来并不太令人印象深刻。

![](img/523a7ca4ff43eab37325b638af809e89.png)

每位患者的原始数据。

![](img/aa3211a8ab4273c28f1026a29c7e844d.png)![](img/6cb6a163a6c2b92a4253393349f8b1d0.png)

将它们分别标绘显示出这些数据有多好。虽然不是对每个患者，当然也不是对每个指标(这个显示的是 **VO2cdi** )，但是在这个粒度级别查看临床数据仍然很有趣。

![](img/b207f6ad957b462ee3883bb48cf0b35b.png)

以及在基础数据之上覆盖箱线图的图。有很多事情正在进行，如果信号存在的话，破译潜在的变化并找到它是很重要的。

![](img/1584b475a6fd317ec708d0d65b720c0d.png)![](img/2fa2c4694e8ed8ea63808280758dab43.png)

时间序列转换，但并没有真正做很多，或至少提供了很多信息。

现在，时间变量在相同的尺度上，但是正在发生的事情没有在相同的维度上发生，这使得它们不可比较。换句话说，我无法比较 44 名患者的曲线。所以，我做的是建立一个变量，显示经过的时间。你可以看到我开始在这里绘制数据(*，你可能已经看到我在前面的图中进行了尝试，但并不是每个指标都很有趣*)。从每个患者身上，我们将获得 124 个测量点。

![](img/b615231b482a93524a53d9a3c3d6c1dd.png)![](img/f21ffa5c341c877440465d66289f9aac.png)![](img/b2430adb70dc7a75fbe22fc36b6ffb3b.png)

相同的数据，但以线和点的形式显示，VO2cdi 的模式，随着时间的推移，每个患者。

请记住，我在开始时说过，这项研究的重要性在于评估在用于确定某种形式的临床相关性的测量中添加特定参数的重要性。因此，基于文献和这项研究的目的，我建立了变量来显示何时测量超出了其临床相关的界限。这些是**而不是**异常值，尽管人们可以称之为异常值。然而，不使用一些统计异常检测，我们将只使用由文献确定的临床阈值来标记它们。

![](img/125c1e94b18984f18a013e63eb6e43b4.png)

不要忘记，这些刻度是免费的，我是根据原始数据(而不是经过的时间)绘制的。**红色**表示界内，**蓝色**表示界外。你可以看到，几乎每个患者都超出了这个变量及其临床相关性所确定的界限。

![](img/ec4b51a4a847edb50667758266712c06.png)![](img/9ace7bb43622c8c18bd2814bae0f4ca9.png)![](img/b2ea7cefac159dc816ae1acee356d2ae.png)![](img/f7635eccc567626b881d3b02f268f25d.png)

当然，我们可以开始结合某些测量方法及其临床相关参数

现在，真正的研究是通过测量不同的参数来确定差异。尤其是在临床相关性方面，因为如果测量值的差异改变了临床决策，它就会变得非常成问题。

下面，你会看到两种测量 VO2 的方法，以多种方式描述。目的是分析使用其中一种药物对临床决策的影响。

![](img/d587fba0866a3ebadfcaa320454c7047.png)

使用两种方法显示 **VO2** 值分布的箱线图。清楚地看到方法 2 ( **深绿色**)如何提供比方法 1 ( **红色**)高得多的值。这是一种相互作用，因为每个患者的差异并不相同。

![](img/eb6b5d6e180049ef916246d0a69a6e3e.png)

并且该图显示了每个患者的差异分布。我们知道每个病人有 124 个观察值，没有缺失数据出现。所以，如果没有相互作用，所有的变化都是一样的。但事实并非如此，这意味着数据和值本身的变化会影响差异。

```
g1<-ggplot(dataset, aes(x=ElapsedTime, 
                    group=factor(ID))) + 
  geom_line(aes(y=VO2i_methode1, col="red"), alpha=0.7) +  
  geom_line(aes(y=VO2i_methode2, col="blue"), alpha=0.7) +
  scale_y_continuous(name = "VO2 method 1 (red) & 2 (darkgreen)")+
  theme_bw()+ 
  theme(legend.position = "none")
g2<-ggplot(dataset, aes(x=ElapsedTime, 
                    group=factor(ID), 
                    y=VO2i_methode1-VO2i_methode2, 
                    fill=as.factor(ID))) +
  geom_smooth(col="black", alpha=0.5)+
  scale_y_continuous(name = "Difference VO2 method 1 & 2")+
  theme_bw()+ 
  theme(legend.position = "none")
gridExtra::grid.arrange(g1,g2,ncol=2)
```

![](img/21cb93dae8e254008bf2d1cead70cc29.png)

在左侧， **VO2** 根据两种方法的 **ID** 测量经过的时间。向右，一个时髦的平滑差异的方法。

展示任何一种差异的另一种方式是以热图的形式绘制它们。现在，如果我使用 *ElapsedTime* 创建一个热图，我会得到一个有很多间隙的热图。因此，我将切割数据，然后绘制差异。

```
options(scipen=999)
cutTime<-cut(as.numeric(dataset$ElapsedTime), breaks=25, dig.lab=6, labels=FALSE);cutTime
ggplot(dataset, aes(x=as.factor(cutTime), 
                    y= ID, 
                    fill=VO2i_methode1-VO2i_methode2)) +
  geom_tile()+
  scale_fill_viridis_c(option="B")+
  labs(x="Time cut in pieces", 
       y="ID", 
       title="Difference for V02 between method 1 and 2 per ID")+
  theme_bw()+ 
  theme(legend.position = "none")
```

![](img/9ce8c6ccd0f6ebd0b1aece0fcdffca79.png)

如您所见，不同的时间和 ID 并不相同。在某些情况下，我们似乎没有分歧，但对大多数人来说，分歧是不稳定的。因此，临床决策很可能会受到影响。

GGplot 有一种奇怪的方式来尝试添加两个 y 轴:要么你通过*网格*或[gridExtra](https://cran.r-project.org/web/packages/gridExtra/index.html)进行某种形式的组合，要么你在 [*GGplot*](https://ggplot2.tidyverse.org/reference/sec_axis.html) 中添加一个变换来获得第二个轴。 [*晶格*](https://cran.r-project.org/web/packages/lattice/lattice.pdf) 有一个更直接的方法，通过 [*xyplot*](https://homerhanumat.github.io/tigerstats/xyplot.html) 创建对象，然后将它们合并在一起。

![](img/24ae3df5dd879e0e944767ad44c9c38a.png)![](img/0ccd8422f9cb18881ceaef2e06d2be9d.png)![](img/990ca727b170c6b03a97957453041f08.png)

方法 1、2 和通过三个物体的区别由晶格制造。

![](img/f5e2767bfb8fe3f17dc6ffa273d336ae.png)

在 lattice 中，这些对象可以完美地合并。只看方法的不同。那是相当严重的。

现在，如果您想检查一个变量对指标的影响，您可以应用蒙特卡罗模拟来了解这种影响。我们要做的是，用这两个公式，看看它们的结构，然后自己处理变量。从这个练习中，我们可以通过改变组成结果的组件来估计结果是如何变化的。所以，让我们来看看对于我们已经多次使用的 VO2 如何工作。

*VO2* 的标准公式是:

![](img/5958613601ed02f335cc2d4ca45c87e4.png)

VO2i = gendexeerde 耗氧量(ml min-1m-2)；Q =动脉血流量(L 分钟-1)；Ht =血细胞比容(%)；SvO2 血氧饱和度=动脉和静脉饱和度(-)；PaO2 en PvO2 =动脉和静脉的
氧分压(mmHg)；BSA =体表 Dubois 公式(m2)。

方法 1 和 2 的区别在于包含/排除了 *PaO2* 和 *PvO2* ，因此分子的后半部分。让我们通过应用蒙特卡罗模拟来看看这个变量的影响。

```
mean(dataset$Flow)   # 4.9
mean(dataset$Hct)    # 30
mean(dataset$SaO2)   # 98
mean(dataset$SvO2)   # 74
mean(dataset$PO2art) # 132
mean(dataset$PO2ven) # 39VO2_1 <- function(Q, Ht, SaO2, SvO2, PaO2, PvO2, BSA) {
  VO2 <- Q * (((Ht/2.94)*1.36*(SaO2-SvO2)+0.003*(PaO2-PvO2))/BSA)*10
  return(VO2)
}
VO2_2 <- function(Q, Ht, SaO2, SvO2, BSA) {
  VO2 <- Q * (((Ht/2.94)*1.36*(SaO2-SvO2))/BSA)*10
  return(VO2)
}
VO2_1(4.9,0.30,98,74,132,39,2.06) 
VO2_2(4.9,0.30,98,74,2.06)
```

对于方法 1 和方法 2，我们最终得到的 *VO2* 是 85 和 79，四舍五入。方法 2 不包含 *PaO2* 和 *PvO2* 。这些是要模拟的函数，看看它们的影响。为了做到这一点，我将把其余的保持在最低限度。我确信那也会有影响，但是为了争论的缘故，让我们保持这种方式。继续模拟:

```
summary(dataset$PO2art) # range: 43 - 240
PaO2<-seq(0,240,by=1)   # zero because I want to see influence of nothing
summary(dataset$PO2ven) # range: 33 - 48
PvO2<-seq(0,48,by=1)  # zero because I want to see influence of nothing 
matVO2_1<-matrix(data=NA, 
                 nrow=length(PaO2), 
                 ncol=length(PvO2));matVO2_1
for (i in 1:length(PaO2)){
  for (j in 1:length(PvO2)){
    Q=4.9
    Ht=0.3
    SaO2=98
    SvO2=74
    BSA=2.06
    matVO2_1[i,j] <- Q * (((Ht/2.94)*1.36*(SaO2-SvO2)+0.003*(PaO2[i]-PvO2[j]))/BSA)*10
  }
}
colnames(matVO2_1)<-PvO2
rownames(matVO2_1)<-PaO2
df<-reshape2::melt(matVO2_1,
                   value.name = "VO2", 
                   varnames=c('PvO2', 'PaO2') )
dim(df)
head(df)
```

![](img/1201d845e80d4ecfaa9a7c99c701a996.png)

提取的数据集。

```
ggplot(df, aes(x=PaO2, 
               y=PvO2, 
               z=VO2))+
  geom_contour_filled()+
  labs(title="Influence of changing PaO2 and PvO2 on VO2",
       fill="VO2")+
  theme_bw()
```

![](img/c83e67985de93ed59463f18d2e1ea5ec.png)

并且所获得的图显示了 **PaO2** 和 **PvO2** 之间的相互作用。可以肯定的是，它们的存在确实影响了 **VO2** 的度量，从而也影响了临床决策。

从上面可以清楚地看出，这两个值中任何一个的存在与否都会影响结果 *VO2* 。事实上，这个数据集中包含的大多数度量都是相互影响的。这提供了另一个机会，对数据进行时间序列分析，并寻找协整→一个时间序列对另一个时间序列的时间相关影响。我在之前已经应用了这样的方法[查看新冠肺炎的数据以及封锁等措施的影响。](/the-effect-of-covid-measures-on-covid-outcomes-a-vector-autoregressive-moving-average-exogenous-dd7fa6c3774f)

首先，从小处着手，寻找不同的方法来更好地掌握数据。

![](img/7604b88777737d435f4f70783353c1b1.png)

显示 **VO2** 测量值的平均差值的图，以及拟合模型。实际上并没有向我们展示任何东西，除了差异可能确实很大，但也不一定如此！顺便说一下，这是整个时间序列。

```
dataset%>%dplyr::filter(ID=="1")%>%
  summarise_by_time(Tijdstip,.by="minute",
                    VO2i_verschil_mean=mean(VO2i_verschil,na.rm=TRUE))%>%
  plot_time_series_regression(.date_var = Tijdstip, 
                              .formula = VO2i_verschil_mean~ as.numeric(Tijdstip) +
                                minute(Tijdstip), 
                              .interactive = FALSE, 
                              .facet_ncol=2)
```

![](img/e28b2fa2eae356750ab1eb35b8be6192.png)

单个人的时间序列对象

让我们创建一个包含多个时间序列的时间序列对象。从那个物体，我们可以画出一系列非常好的东西。为了让数据更容易理解，现在我将为一个人创建这样一个对象。

```
mymts<-dataset%>%
  dplyr::filter(ID=="1")%>%
  dplyr::select(VO2i_methode1,DO2i_methode1,RQ_methode1)%>%ts(.)
mymts
```

![](img/41ce11e2fc398fa01068ac2abf9cad76.png)

要使用的数据集。

现在让我们绘制来自对象的数据。巧妙之处在于绘图函数会将对象识别为多变量时间序列，因此如果您要求绘制数据，它会干净利落地完成。

```
plot(mymts)
theme_set(theme_bw())
autoplot(mymts) +
  ggtitle("Time Series Plot of the `mymts' Time-Series") +
  theme(plot.title = element_text(hjust = 0.5)) #for centering the text
```

![](img/525260913e5dd37686b323488190408c.png)![](img/5b1f5ccbd34bb7514cb727aa731cd362.png)

左边的情节比右边的好得多，这很有趣，因为右边的叫做自动情节。

时间序列数据看起来很酷，但有很多警告，假设，并认为你可以和需要检查。在适用的例子中，我已经写了一些这样的假设[这里](https://medium.com/mlearning-ai/predicting-the-feed-consumption-of-a-swine-farm-81ce4f703f3a)，这里[这里](/time-series-analysis-of-an-egg-farm-cd150137f684)，这里[这里](https://pub.towardsai.net/time-series-analysis-in-sas-8b6c03b39fd4)。

从 *mymts* 对象，我们可以对每个时间序列分别应用[增强的 Dicky Fuller](https://en.wikipedia.org/wiki/Augmented_Dickey%E2%80%93Fuller_test) 测试，以满足整合数据的需要。就我个人而言，我不喜欢 p 值驱动的测试，但是因为它经常被使用。ADF 测试测试了[零假设](https://en.wikipedia.org/wiki/Null_hypothesis)，即[单位根](https://en.wikipedia.org/wiki/Unit_root)出现在[时间序列](https://en.wikipedia.org/wiki/Time_series) [样本](https://en.wikipedia.org/wiki/Sample_(statistics))中。[替代假设](https://en.wikipedia.org/wiki/Alternative_hypothesis)根据使用的测试版本不同而不同，但通常是[平稳性](https://en.wikipedia.org/wiki/Stationarity_(statistics))或[趋势-平稳性](https://en.wikipedia.org/wiki/Trend-stationary_process)。这是针对更大更复杂的时间序列模型的迪基-富勒测试的扩充版本。测试中使用的增强 Dickey-Fuller(ADF)统计量是一个负数。越是否定，就越是强烈的拒绝在某个置信水平上存在单位根的假设。所以，你想看到的是，如果一个负值，或者一个显著的 p 值，显示了一个随机过程的缺失——一个单位根。

```
apply(mymts, 2, adf.test)
```

![](img/d38aae2cb1ca80a49dc92615430383d6.png)

DO2i _ methode1 变量是唯一的变量，根据 ADF 测试，它具有单位根。通常有帮助的是整合数据，这意味着你将从时间序列中提取一个 d 阶滞后函数。

虽然我们看到一个变量可以从积分中获益，但我会让它保持原样，看看它如何长期发挥作用。这就把我带到了下一个阶段，那就是向量自回归模型的应用。我很懒，所以我将使用一个自动选择功能，使用最大 10 个时间序列单位的滞后，并使用 AIC 来查找在给定现有数据的情况下表现最佳的滞后值。就像我说的，这是懒惰，但开始我会说这是一个好的开始。

```
VARselect(mymts,type = "none",lag.max = 10)
var.a <- vars::VAR(mymts, lag.max = 10,ic = "AIC",type = "none")
summary(var.a)
serial.test(var.a)
plot(var.a)
```

![](img/c9708203a2a5417f610bceb807a4891d.png)

您可以看到使用了四个选择标准:AIC、总部、SC 和 FPE。由于我们使用 AIC 来确定我们使用的滞后变量，因此所选的滞后变量将为 2。

因此，利用这个价值，我将建立风险值模型。由于我们将三个时间序列整合在一起，并开始进行协整，我们将得到许多结果，这使得这类模型不容易运行。

```
var.a <- vars::VAR(mymts,
                   lag.max = 2, 
                   ic = "AIC", 
                   type = "none") 
summary(var.a)
serial.test(var.a)
plot(var.a)
causality(var.a, #VAR model
          cause = c("VO2i_methode1"))
```

![](img/b096320cdc9056ca3bf8fc02826847fe.png)

这三个模型中的每一个模型的总结结果是它们自身和最大滞后为 2 的其他时间序列的函数。调整后的 R-squares 非常大，暗示着严重的过度拟合。

![](img/a701339152e96051d6e20a643209ea9d.png)

和三个模型的相关矩阵。VO2 和 **RQ** 高度相关，这不足为奇。 **RQ** 是 **VO2** 的函数。

接下来是串行测试，它计算串行相关误差的多变量组合和 [Breusch-Godfrey](https://en.wikipedia.org/wiki/Breusch%E2%80%93Godfrey_test) 测试。breu sch-Godfrey 检验是对回归模型中[误差](https://en.wikipedia.org/wiki/Errors_and_residuals_in_statistics)的[自相关](https://en.wikipedia.org/wiki/Autocorrelation)的检验。它利用了在[回归分析](https://en.wikipedia.org/wiki/Regression_analysis)中考虑的模型的[残差](https://en.wikipedia.org/wiki/Errors_and_residuals_in_statistics)，并从这些残差中得出测试统计。[零假设](https://en.wikipedia.org/wiki/Null_hypothesis)是到*p*为止没有任何阶的[序列相关](https://en.wikipedia.org/wiki/Serial_correlation)

![](img/81b4bb3c9da34d46c08a84b2eb4518d2.png)

根据这个测试，没有序列相关性。就像我之前说的，这些测试有价值(可以有价值),但 p 值临界值不应该真正使用。

![](img/c5dcf31317c068a33713d92c9128822f.png)![](img/789e4695befa9dbadfcf84edf03e0720.png)![](img/bce97e444c4919ef6d765570afb5e155.png)

最好仔细看看这些图，尤其是显示自相关和部分自相关值的 ACF en pACF 值。界限内的一切都很好，这是我希望看到的。因为如果我不这样做，我将不得不整合数据，添加预测或直接模拟残差。

从理论上讲，我们可以从 VAR 模型中了解潜在的因果关系。如果你记得布拉德福德·希尔因果关系标准，你就会知道时间(或暂时性)是这些标准中的一个。由于我们正在研究时间序列内部和之间的变化，我们可以看到一个时间序列的变化是否与另一个时间序列有足够大的交叉相关性。我们可以测试这种关系是否只是单向的，我们可以减去时间序列本身的自相关性。

使用的特定测试是[格兰杰因果关系](https://en.wikipedia.org/wiki/Granger_causality)测试，其中你必须指定哪些变量被假设以因果方式相互影响。我在新冠肺炎数据上做了两次——一次是这里的，一次是这里的，尽管后者使用的是不同的方法。除了我们永远无法确定完全的因果关系这一事实之外，这项工作至少有一些价值，可以表明这种关系可能会向哪个方向发展。

如果通常通过对 *X* 的[滞后值](https://en.wikipedia.org/wiki/Lag_operator)(也包括 *Y* 的滞后值)的一系列 [t 检验](https://en.wikipedia.org/wiki/T-test)和 [F 检验](https://en.wikipedia.org/wiki/F-test)可以表明，这些 *X* 值提供了关于未来值的[统计上显著的](https://en.wikipedia.org/wiki/Statistical_significance)信息，则称时间序列 *X* 为格兰杰原因 *Y*

![](img/db63aa448b403beedfec6dd246108ae4.png)

[https://en.wikipedia.org/wiki/Granger_causality](https://en.wikipedia.org/wiki/Granger_causality)

我要做的是指定时序 *DO2* 和 *RQ* 的“原因”是 *VO2* 。然后，该模型将通过滞后值来查看它是否能够建立预测因果关系，这意味着您想要发现 *VO2* 的值是否对 *DO2* 和 *RQ* 具有足够的预测能力，但反之则不成立。

![](img/ef8e617f337dc8b6a91439a432586bd1.png)

在这两种情况下，零假设都被拒绝，这意味着 **VO2** 确实对 **DO2** 和 **RQ** 有影响。再一次，考虑到公式是如何组成的，这并不奇怪。这确实意味着一个用于预测 RQ 的模型很可能在包含自身滞后的情况下做得很好，但也很可能包含 VO2 的滞后。

![](img/1a7317a7203cbaf15daa9c0460b8a0d6.png)

每个时间序列的预测。预测窗口没那么大，但是预测区间很宽。这些都不是有帮助的预测。

过去几年都是关于深度学习，深度学习和在有数据的地方应用神经网络的深度学习。除了 neurals 需要大量数据来处理的事实之外，它们肯定不会自动击败旧模型。这不是终结者，旧型号并没有过时。此外，通过非常简单的功能将引擎盖下的一切自动化，使用和生产这种复杂的模型变得越来越容易。让我给你举个例子来说明这有多容易。

从多个时间序列对象中，我们可以使用[预测](https://www.rdocumentation.org/packages/forecast/versions/8.16/topics/nnetar)包中的 [nnetar](https://www.rdocumentation.org/packages/forecast/versions/8.16/topics/nnetar) 函数在第一个时间序列上拟合一个神经网络。时间序列会自我预测，这也是时间序列的部分设计目的。

```
fit = nnetar(mymts[,1]);fit
nnetforecast <- forecast(fit, h = 20, PI = T)
plot(nnetforecast)
```

![](img/9449880853dfa8b8965ad92d9305223f.png)

时间序列拟合的神经网络模型。没有调整，只是标准功能，以获得一个网络系综。

![](img/e907b279ca4bbe0f161e544601a41fb6.png)

预测也一样恐怖。

通过添加预测器，可以增强神经网络。所以让我们说，这样模型就不会阻塞自己，我们让 *DO2* 预测 *VO2* 。

```
mymts[,1:2]
fit2 = nnetar(mymts[,1], xreg = mymts[,2])
nnetforecast2 <- forecast(fit2, h = 20, xreg = mymts[,2], PI = T)
plot(nnetforecast2)
```

![](img/d9fb051b6632631553dd0af1d9e8ef5d.png)

已经变得很不一样了。预测线仍然是上一个值的前置进位，而且很大，但是仍然有一些变化。好吧，即使事情就是这样。

到目前为止，我们只处理了一个时间序列。单个 *ID* 。但我们有更多的数据，应该使用这些数据，同时包含数据的时间序列性质。突出我们的一个好方法是通过特定的 GGplot 函数。例如，在下面的例子中，我想突出显示数据集中不同指标 *VO2cdi* 的四个最高时间序列。

```
ggplot(dataset, aes(ElapsedTime, VO2cdi, color=ID)) +
  geom_line(stat="identity") +
  ylab("VO2 cdi") +
  gghighlight(max(VO2cdi) > 220,
              max_highlight = 4,
              use_direct_label = TRUE) +
  theme_minimal() +
  theme(legend.position = 'none')
```

![](img/930c10a1454b4ce184e0451330a3c9c9.png)

由于我们有 44 个受试者的数据，并且每个受试者有 124 个观察值(都是等距的)，让我们看看是否可以添加聚类技术来最大限度地利用数据。我们能在一个特定的结果中，通过观察病人找到模式吗？虽然我们已经解决了主要问题，即参数对 *VO2* 的影响，但我想进一步研究这个数据集并进行实验。当然，过多的实验弊大于利，尤其是事后来看，但无论如何让我们来看看。

下面是对 *VO2cdi* 进行聚类的代码

![](img/768b6ca5d330457f3f50d87857e00698.png)

聚类模型，查看 VO2cdi 上的时间序列数据的聚类

![](img/f44add909a7218b83478f95d68a0ecef.png)

你可以看看模型的一部分。记住，这些数据完全是在 ID 层面上争论的，你可以在 **fit$labels** 看到。

![](img/e6e2ee44d26bed22c0cce3e3abd8117d.png)![](img/bd5d66b80e1c52a04c65d4031a5597af.png)

同样的情节，但旋转。显示哪些 id 聚集在一起。不确定这个结果是否有效。尽管使用了欧几里德距离，我们在这里仍然讨论时间序列，并且尽管测量是同步的，所以每个 ID 有 124 个测量(不考虑时间)，但是没有考虑序列本身的自相关。

我有四个组，所以我能做的是将这些标签与原始数据合并，并绘制属于某个组的每个 *ID* 的外观。下面，您可以看到 cluster group = 2 的时间序列。

![](img/5c3badffb63b796ee68d48bb2f2a3a1d.png)

时间序列和样条曲线覆盖在聚类 2 中每个 id 的顶部。

我们当然可以显示所有聚类的所有数据，并查看聚类算法(欧几里德距离)是如何进行分割的。

```
ggplot(joined_clusters, 
       aes(Meting, 
           VO2cdi, 
           col=as.factor(cluster), 
           group=ID)) +
  geom_line()+
  theme_minimal() +
  labs(y="VO2 cdi",
       x="Measurement",
       col="Cluster")
```

![](img/d38d98301fe3b168d7fae237d1c5a783.png)

对时间序列数据进行聚类的另一种方法是使用 [kml](https://cran.r-project.org/web/packages/kml/kml.pdf) 包。这个包是专门设计用来对时间序列数据进行聚类和寻找聚类的。让我们将 klm 包应用于不同的结果， *Hb* ，然后再看看它会对 *VO2cdi* 结果产生什么影响。

![](img/5679aa434353c35d5bf352bab4ebe195.png)

为了让 **kml** 工作，数据需要有一个特定的结构，在这个结构中指定时间数据和 **ID** 数据。

![](img/9abdbb45f7fcee2fc15b5b511e8f8cde.png)

然后，您可以让算法完成它的工作。你既可以让它选择指定多少个聚类，也可以指定你想要多少个先验聚类。

![](img/6371a3b7fcb796b30b2c39ee390359af.png)

这是我们的结果，六个聚类，考虑到我们要求模型运行聚类过程的多次迭代，这可能是一个非常乏味的过程。

让我们重复同样的方法，但是对于 *VO2cdi* 来看聚类技术是否显示相同的结果。

![](img/b75640442d7996d8c536263466f66821.png)![](img/043ff7d1ddb75768e79d73365830f6ee.png)

现在我获得了三个集群，而不是之前练习中查看 **VO2cdi** 时的四个集群。但是，如果您比较这些图表，您可能已经发现四个集群可能有点太多了。所以，我可以忍受三个。

最后但同样重要的是，让我们看看是否可以聚类一个多元时间序列对象。现在，我可以从一开始就开始做的事情之一，就是看看哪些变量有很大的相关性，然后在对象中把它们加在一起。

![](img/924fb9360939580612fa61a896815fa1.png)

显示数据集中每个变量组合的相关图的代码。进行选择，但是要小心——时间序列对象的相关性分析应该比这里处理得更好。这里，相关性度量将不考虑集群内的差异。此外，它还是一个筛选工具，可以绘制更多的图来进行检查。

![](img/2c6bb5e37e46bf62a0fab5d7a24abd6c.png)

例如， **Hb** 和 **VO2cdi** 之间的相关性。该图显示了每个 **ID** 的明显差异，以及变量如何聚集。我还不确定具体的关系是什么，但是这里好像储存了信息。

让我们回到旧的 *mymts* 对象，它包含三个交织在一起的指标。我要做的是建立一个更符合时间序列分析本质的聚类模型。为此，我将部署[*ForeCA*](https://cran.r-project.org/web/packages/ForeCA/index.html)*包和 *foreca* 函数。 *Foreca* 代表 [**可预测成分分析(ForeCA)**](http://proceedings.mlr.press/v28/goerg13.pdf) ，一种针对时变信号的新型降维技术。基于一种新的可预测性度量，ForeCA 找到了一种将多变量时间序列分离成可预测空间和正交白噪声空间的最佳变换。这份报纸在统计数据上很重，在某些方面肯定超出了我的能力范围，但是我确实想给你一个可用的概念。请记住，我们开始提出的问题不需要这种分析。我们可以在几段前就开始。但在处理多变量时间序列方面，它确实显示了一个可用的好的补充。*

*让我们来构建一个包含七个序列的多元时间序列。*

```
*mymts<-dataset%>%
  dplyr::select(VO2i_methode1,
                Hct,
                SaO2,
                PCO2art,
                SvO2,
                DO2i_methode1,
                RQ_methode1)%>%
  ts(., frequency=31557600)
plot(ts(mymts))*
```

*![](img/6371781679fb8fb5d09ecf975bd9f1e4.png)*

*请记住，这些时间序列是所有相邻的患者组成的七个极长的时间序列！*

*现在来看时间序列的成分分析。这个函数本身当然非常简单，所以我认为它揭示了潜在的数学原理。例如，在一句话中，我制作的多变量时间序列对象被缩减为两个部分。这就是我想要的。哦，欧米伽值是多少？这是衡量可预测性的标准。你希望那个尽可能大。*

*![](img/5449aa59143caaf1129908fd132688e5.png)*

*这就是我得到的，不好看。组件 1 的欧米伽分数远低于最大欧米伽分数。*

*![](img/61c6058f7d949f52c7c843dbe45bdcd8.png)**![](img/c96b72cf436b3d1a955a9a6de0853f6c.png)*

*在此图中，您可以看到算法的频谱密度部分，试图通过查看时间序列中的振荡来找到聚类。你可以看到每种成分的欧米伽值。*

*让我们分成三类。在你滚动之前，我会警告你——滚动不成功。*

```
*mod.foreca <- foreca(mymts, 
                     n.comp = 3, 
                     plot = TRUE, 
                     spectrum.control=list(method="pspectrum"))
mod.foreca
plot(mod.foreca)
biplot(mod.foreca)
summary(mod.foreca)
mod.foreca$scores <- ts(mod.foreca$scores, 
                        start = start(mymts),
                        freq = frequency(mymts))
plot(mod.foreca$scores)
round(cor(mod.foreca$scores), 3)
spec <- mvspectrum(mod.foreca$scores, "pspectrum");plot(spec)*
```

*![](img/c1745fd0f9a5271980afc3abb3bf491b.png)*

*如您所见，数据的聚集并没有真正的帮助。*

*![](img/bdbbfd8ae793cb1846aa60733239757a.png)*

*每个组件的可预测性。*

*![](img/515fa190667eeb5c7733826c70deb818.png)**![](img/5019308ab6205daeb71b8812fb4405dc.png)**![](img/e049bda955c7f086de2ae4fac3016640.png)*

*并且包含更多的迭代也没有任何作用。似乎真的没有那么多信号，虽然模型说没有一个分量是由白噪声组成的(这纯粹是随机性)。*

*在汇总统计数据和双标图下方。老实说，我喜欢数字，但我更喜欢图表，下面的图表给我展示的比它旁边的数字更多。也就是说，聚类是如此之少，如果你看到了最初的时间序列，这也许并不奇怪。*

*![](img/c14737455ac53529eed01ba79eb73d4b.png)**![](img/7e9dec6d89b0f4b9569687a8fe92f8b9.png)*

*如你所见，在文本中，右边的图形中，没有任何聚集。*

*![](img/3873b0bb06ab71249f4a796f6fcb1190.png)*

*可预测的组件。*

*![](img/b4686b0cf8cd5d28a46b63f363a58c0c.png)*

*和相关矩阵。哎哟，不好。虽然你希望集群是正交的，但如果它们如此整齐干净，因此为零，我会非常惊讶。*

*下图显示了多元时间序列的估计谱。这三个集群似乎遵循相同的趋势。不确定这个情节有多大帮助。老实说，我觉得整个包有点奇怪，除了双标图，这是非常熟悉的从偏最小二乘法得到的图。我想，这只是我缺乏了解！*

*![](img/132f92c15b21f0d3408c4f9ae0d62c02.png)*

*好了，我希望你喜欢这篇博文。在几秒钟内处理时间序列数据是很好的，也很有挑战性，但最重要的是你要尽可能地坚持最初的研究问题。我偏离了这一点，但是分析数据不仅仅是工作，也应该是有趣的:-)*

*如果有什么不对劲，请告诉我！*