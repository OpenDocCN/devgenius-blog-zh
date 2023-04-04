# 小猪的生长模型

> 原文：<https://blog.devgenius.io/growth-modeling-in-piglets-534766dcad1f?source=collection_archive---------13----------------------->

## 在 R 中使用混合模型

这是我多年前在一个商业数据集上做的一个项目的帖子。因为它是商业的，我不能与你分享，但我可以展示我如何处理这个小猪试验模型的请求。

从我的许多帖子中，你已经可以看出我将使用统计模型对这个数据集进行建模，最好是[混合模型](https://medium.com/mlearning-ai/introduction-to-mixed-models-in-r-9c017fd83a63)，因为它允许我对固定和随机效应进行建模，后者可以通过方差-协方差矩阵进行最佳处理。

让我们开始吧，我将带您了解整个过程。

```
rm(list = ls())#### LIBRARIES ####
library(lme4)
library(ggplot2)
library(rms)
library(plyr)
library(reshape2)
library(boot)
library(sjPlot)
library(sjstats)
library(sjmisc)
library(interval)
library(AICcmodavg)
require(parallel)  
library(gridExtra)
library(coefplot) 
library(coda)      
library(aods3)     
library(plotMCMC) 
library(bbmle)     
library(nlme)
library(merTools)
library(RLRsim) 
library(pbkrtest)
library(multcomp)
library(lsmeans)
library(multcompView)
library(lattice)
library(splines)
library(lmtest)
library(car)
library(corrplot)
library(PerformanceAnalytics)
library(eqs2lavaan)
library(mgcv)
library(gamm4)
library(robustlmm)
library(influence.ME)
library(readxl)
library(sfsmisc)#### DATA IMPORT & MANAGEMENT ####
Marc <- read_excel("")
colnames(Marc)[2]<-"Trt"
colnames(Marc)[3]<-"Number"
colnames(Marc)[4]<-"D7"
colnames(Marc)[5]<-"D21"
colnames(Marc)[6]<-"D34"
colnames(Marc)[7]<-"D42"
colnames(Marc)[8]<-"FCR7"
colnames(Marc)[9]<-"FCR21"
colnames(Marc)[10]<-"FCR34"
colnames(Marc)[11]<-"FCR42"
GUISSONA<-Marc
Marc
GUISSONA<-as.data.frame(GUISSONA)
GUISSONA<- reshape(GUISSONA,
                    varying  = list(c("D7", "D21", "D34", "D42"),c("FCR7","FCR21","FCR34","FCR42")),
                    v.names = c("Weight","FCR"),
                    timevar = "Time",
                    times = as.numeric(c("7", "21", "34", "42")),
                    direction = "long")
GUISSONA$id<-NULL
GUISSONA<-GUISSONA[order(GUISSONA$Pen),]
row.names(GUISSONA)<-NULL
GUISSONA<-GUISSONA[!is.na(GUISSONA$Weight)|!is.na(GUISSONA$FCR),]
dim(GUISSONA)
GUISSONA$Trt<-as.factor(GUISSONA$Trt)
GUISSONA<-as.data.frame(GUISSONA)
class(GUISSONA)
GUISSONA$Time_center<-scale(GUISSONA$Time, center=TRUE, scale=FALSE)
GUISSONA$Time_fact<-as.factor(GUISSONA$Time)
head(GUISSONA)
```

![](img/160c8ce27c5a569c5b29c250fb95fcc3.png)

分层数据—纵向。

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(7,21,34,42))
myy<-scale_y_continuous(breaks = seq(0, 3, by = 0.25))
ggplot(GUISSONA, aes(x=Time, y=Weight, colour=Trt, shape=Trt))+
  myx+
  myy+
  stat_summary(fun.y=mean, geom="point", lwd=2)+
  stat_summary(fun.y=mean, geom="line", lwd=1)myy<-scale_y_continuous(breaks = seq(0, 3, by = 0.25))
ggplot(GUISSONA, aes(x=as.factor(GUISSONA$Time), y=Weight, colour=Trt)) +
  geom_boxplot() +
  theme_bw() +
  myy+
  labs(title = "BW over time", y="BW", x="Time")
```

![](img/d6d04be74c9e9a3f2de9ceee7d7a1983.png)![](img/9683c385947f6d0e023238a304c85f54.png)

哇，在这个数据集中寻找治疗差异将是一个相当大的挑战。有时候，建模在你开始之前就已经完成了。

```
sjp.poly(GUISSONA$Weight,GUISSONA$Time, 1)
sjp.poly(GUISSONA$Weight,GUISSONA$Time, 2) 
sjp.poly(GUISSONA$Weight,GUISSONA$Time, 3)
sjp.poly(GUISSONA$Weight,GUISSONA$Time, 4)
```

![](img/c61584381db42237152e1dc9a33378fe.png)![](img/c9fbb12d73c0d76abc4aecbecdab5d9f.png)![](img/bd5328ad48f1e06daf4bb737eca5957c.png)![](img/1bcd01c4ffadc39b4d5e93571c8820eb.png)

老实说，线性拟合看起来足够了。

```
xyplot(GUISSONA$Weight~GUISSONA$Time|GUISSONA$Trt,
       panel = function(x, y, ...) {
         panel.grid()
         panel.xyplot(x, y)
         panel.lmline(x, y, lty = 2, col="black")
         panel.loess(x, y, lty = 2, col="red")
         panel.abline(0, 0)
       } )
```

![](img/f0f43404568437ee175717db8e415017.png)

治疗方法看起来非常相似。

```
xyplot(GUISSONA$Weight~GUISSONA$Time|GUISSONA$Pen,
       panel = function(x, y, ...) {
         panel.grid()
         panel.xyplot(x, y)
         panel.lmline(x, y, lty = 2, col="black")
         panel.abline(0, 0)
       } )
```

![](img/4d89f77f18e8a2be6a3323656e59606b.png)

所有的笔似乎都有相同的反应。也许测量日应该有不同的位置？

```
bwplot(GUISSONA$Weight~GUISSONA$Trt)
bwplot(GUISSONA$Weight~as.factor(GUISSONA$Pen)) 
bwplot(GUISSONA$Time~GUISSONA$Weight)
bwplot(GUISSONA$Weight~as.factor(GUISSONA$Time)) 
bwplot(GUISSONA$FCR~as.factor(GUISSONA$Trt)|GUISSONA$Time_fact)
```

![](img/24fea566bd250d62075d720a4e2fc7b0.png)![](img/3d78fdc98939de57860a5c129255782c.png)![](img/adbc2eae2fc0a5a0f7cb4b31eca3e8e1.png)![](img/480e5446a5e73dc9b2b962006972063d.png)

随着时间的推移和重量的增加，差异也会增加。一个普通的故事。

```
qqp(GUISSONA$Weight)densityplot(~GUISSONA$Weight|GUISSONA$Time,scales=list(relation="free"))scatterplotMatrix(~ Time + Weight + Trt,
                  span=0.7, id.n=0, data=GUISSONA)
```

![](img/c42428d847b57ef3ed8e35f437473067.png)![](img/6748e9fdd81939ce59ed82055fbf1ff2.png)![](img/8acc33fe638760f976c29dd2c904fef8.png)

重量看起来正常，时间肯定有一些峰值，需要使用数据中包含的变量来处理。除了已经知道的，其余的并没有显示出令人印象深刻的相关性

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(7,21,34,42))
myy<-scale_y_continuous(breaks = seq(0, 3, by = 0.25))
ggplot(GUISSONA, aes(x=Time, y=FCR, colour=Trt, shape=Trt))+
  myx+
  myy+
  stat_summary(fun.data=mean_se, geom="pointrange", lwd=0.75)+
  stat_summary(fun.y=mean, geom="line")myy<-scale_y_continuous(breaks = seq(0, 3, by = 0.25))
ggplot(GUISSONA, aes(x=as.factor(GUISSONA$Time), y=FCR, colour=Trt)) +
  geom_boxplot() +
  theme_bw() +
  myy+
  labs(title = "FCR over time", y="BW", x="Time")
```

![](img/9f9c09a100702a6567455c43e7ea2314.png)![](img/0d6f066e2fdfb7b152e160ab1d127699.png)

FRC 在不同的治疗中表现更好。

```
histogram(~GUISSONA$FCR|GUISSONA$Trt, breaks=10)
histogram(~GUISSONA$FCR|GUISSONA$Time, breaks=10)
```

![](img/048e29caa8cf81867110c03ef65281ce.png)![](img/ee4050e9e382fd0dda5a2de11c8c2f4e.png)

不是很有帮助的情节。

```
xyplot(GUISSONA$FCR~GUISSONA$Time|GUISSONA$Trt,
       panel = function(x, y, ...) {
         panel.grid()
         panel.xyplot(x, y)
         panel.lmline(x, y, lty = 2, col="black")
         panel.loess(x, y, lty = 2, col="red")
         panel.abline(0, 0)
       } )xyplot(GUISSONA$FCR~GUISSONA$Pen|GUISSONA$Trt, # no relationship between pen & FCR
       panel = function(x, y, ...) {
         panel.grid()
         panel.xyplot(x, y)
         panel.lmline(x, y, lty = 2, col="black")
         panel.loess(x, y, lty = 2, col="red")
         panel.abline(0, 0)
       } )xyplot(GUISSONA$FCR~GUISSONA$Number|GUISSONA$Trt, # no relationship between FCR and number of chicks in a pen
       panel = function(x, y, ...) {
         panel.grid()
         panel.xyplot(x, y)
         panel.lmline(x, y, lty = 2, col="black")
         panel.loess(x, y, lty = 2, col="red")
         panel.abline(0, 0)
       } )
```

![](img/41ef4f8ed7ca2fe2b1252432b7b853a9.png)![](img/3fd4c935c3325b9e2bf0dc91c687ca65.png)![](img/5069b0cf177e63fe80dfcafdb0c185a1.png)

将很难发现治疗差异，或包括方差成分。

```
qqp(GUISSONA$FCR)
densityplot(~GUISSONA$FCR|GUISSONA$Time, scales=list(relation="free"))
bwplot(GUISSONA$FCR~as.factor(GUISSONA$Time)) # equal variance
bwplot(GUISSONA$FCR~as.factor(GUISSONA$Trt)|GUISSONA$Time_fact) # equal variance over time, not big differences
```

![](img/a6c316f3f6830ac9563521a89b49edd4.png)![](img/b685952c797b1ad7646e4ab25d3f4488.png)![](img/740d42f5bf2d58919b78de8a0de6259c.png)![](img/9435523e46de4309c98fa3d1408d41be.png)

```
fitlm<-lm(Weight~ns(Time,2)*Trt, data=GUISSONA)
boxcox(fitlm,lambda=seq(-2,2,by=0.1),plotit=T)
bc1<-boxcox(fitlm,lambda=seq(-2,2,by=0.1),plotit=T)
which.max(bc1$y)
LV<-bc1[1]$x[as.numeric(which.max(bc1$y))]
LV
```

![](img/dc7f9f670cfa3e814c376fa63b8ac0fb.png)

Box-Cox 变换暗示了对数变换。

```
densityplot(~(GUISSONA$Weight^LV-1/LV)|GUISSONA$Time, scales=list(relation="free"))
```

![](img/ddfded5464a507a5098cb67b36f49d87.png)

看起来很好，但不能全部正常化。对于右下角的时间空间，转换并没有消除添加预测值的需要。或者使用一种[混合](/mixture-component-zero-inflated-and-hurdle-models-44c5e6fe5d7f)型号。

```
fitbc<-lm((Weight^LV-1/LV)~ns(Time,2)*Trt, data=GUISSONA)
par(mfrow=c(1,2))
plot(fitlm, which=2)
plot(fitbc, which=2) # not much of an improvement, but improved
```

![](img/acd09b99b675d7c90a827281aee9e852.png)

毫无疑问，Box-Cox 变换确实有所帮助。

```
summary(fitbc)
```

![](img/0a5cd9ad1bda1a83e7717210ce8d2555.png)

这就是使用 Box-Cox 回归模型的样子。治疗对体重没有任何作用，但我们已经从图表中知道了这一点。老实说，这个模型甚至没有必要。

```
summary(fitlm)fit.0<-lmer(Weight~1+
            (1|Pen),
          data=GUISSONA)
summary(fit.0) fit<-lmer(Weight~Time_center*Trt+
            (Time_center|Pen),
          data=GUISSONA) # almost no variance in slope 
summary(fit)
plot(fit)
Anova(fit, type=3, test.static="F")
```

![](img/b15a192ce529103b85c1fe510d9f848d.png)

混动车型表现出奇的差。

![](img/e23cc765f7c8dbf1d1843ce0b1a2c29f.png)

还是那句话，治疗不增加任何东西。

```
fit2<-lmer(Weight~Time_center*Trt+
            (1|Pen),
          data=GUISSONA)
anova(fit2, fit) # no random slope needed
Anova(fit2, type=3, test.statistic="F")
```

![](img/3fdee4216f1046fe8f67506d02974fa6.png)

我也是。

```
fit3<-lmer(Weight~Time_center+Trt+
             (1|Pen),
           data=GUISSONA) 
summary(fit3)
anova(fit3, fit2, fit) 
```

![](img/3c5ae17a273480791bfde060113f6387.png)

我也是。没有一种新型号是真正的改进。它只是在一个可以很好地模拟数据的模型中添加预测因子，但在没有治疗差异的情况下不会产生治疗差异。

```
qqp(resid(fit3))
qqp(resid(fit2))
```

![](img/c37ffcb7cd8ca08665ed52f764ca4fa6.png)

两个模型的残差几乎没有区别。

```
xyplot(Weight ~ Time_center|Pen, data = GUISSONA, 
       strip = FALSE, aspect = "xy", pch = 16, cex=0.5, grid = TRUE,
       panel = function(x, y, ..., subscripts) {
         panel.xyplot(x, y, ...)
         ypred <- fitted(fit)[subscripts]
         panel.lines(x, ypred, col = "black")
         ypred2 <- fitted(fit2)[subscripts]
         panel.lines(x, ypred2, col = "blue")
         ypred3 <- fitted(fit3)[subscripts]
         panel.lines(x, ypred3, col = "red")
        },
       xlab = "Time (centered)", ylab = "BW (kg)")
```

![](img/0339d2791e3c07417e58888f2decf982.png)

可以使用线性回归对数据进行建模

```
influence.weight<-influence(fit3, "Pen")
plot(influence.weight, 
     which="cook", 
     cutoff=4/length(unique(GUISSONA$Pen))) # No influential pens
```

![](img/d0cd17017cc6f837e1001af8add8d949.png)

没有影响力的小猪。

```
t.test(GUISSONA$Weight[GUISSONA$Time==7&GUISSONA$Trt=="1"], 
       GUISSONA$Weight[GUISSONA$Time==7&GUISSONA$Trt=="2"])
t.test(GUISSONA$Weight[GUISSONA$Time==21&GUISSONA$Trt=="1"], 
       GUISSONA$Weight[GUISSONA$Time==21&GUISSONA$Trt=="2"])
t.test(GUISSONA$Weight[GUISSONA$Time==34&GUISSONA$Trt=="1"], 
       GUISSONA$Weight[GUISSONA$Time==34&GUISSONA$Trt=="2"])
t.test(GUISSONA$Weight[GUISSONA$Time==42&GUISSONA$Trt=="1"], 
       GUISSONA$Weight[GUISSONA$Time==42&GUISSONA$Trt=="2"])
```

![](img/d2ec1c245d4f3f358e892eb89759b19f.png)

没有显著的 t 检验。顺便说一下，像这样在嵌套数据集上进行 t-test 是一个大禁忌。我只是想看看我是否能找到任何差异，并测试过程本身，但当然完全是胡说八道。

```
wilcox.test(GUISSONA$Weight[GUISSONA$Time==7&GUISSONA$Trt=="1"], 
            GUISSONA$Weight[GUISSONA$Time==7&GUISSONA$Trt=="2"], conf.int=TRUE)wilcox.test(GUISSONA$Weight[GUISSONA$Time==21&GUISSONA$Trt=="1"],           GUISSONA$Weight[GUISSONA$Time==21&GUISSONA$Trt=="2"],conf.int=TRUE) wilcox.test(GUISSONA$Weight[GUISSONA$Time==34&GUISSONA$Trt=="1"],            GUISSONA$Weight[GUISSONA$Time==34&GUISSONA$Trt=="2"],conf.int=TRUE) wilcox.test(GUISSONA$Weight[GUISSONA$Time==42&GUISSONA$Trt=="1"],            GUISSONA$Weight[GUISSONA$Time==42&GUISSONA$Trt=="2"],conf.int=TRUE)
```

![](img/c0e6cca463963f5a0fa7c165a3bc2598.png)

T 检验的非参数版本。往往非常不必要，甚至听起来相当不可行。

让我们做一个基线变化分析。我知道，可能是一个失败的原因，但无论如何让我们多做一点。

```
Marc$D21_7<-Marc$D21-Marc$D7
Marc$D34_7<-Marc$D34-Marc$D7
Marc$D42_7<-Marc$D42-Marc$D7
# Day 21 - Day 7 
fit_D21_D7<-lm(D21_7~D7*Trt, weights = Number, data=Marc)
summary(fit_D21_D7)
par(mfrow = c(3, 2))
plot(fit_D21_D7)
influencePlot(fit_D21_D7)
avPlots(lm(D21_7~D7*Trt, data=Marc))
crPlots(lm(D21_7~D7+Trt, data=Marc))
```

![](img/ec11d7ec7fdb01fcaf08c6963f8d5c79.png)

一个试图用第 7 天治疗预测第 21 天的模型。

![](img/2aa2751d3b92bc50a0c78548f97f2817.png)

残差看起来不错，一个大的 x 级异常值。

![](img/18d196e6afead5f9c4c90a5f20fbf554.png)

现在，这是一个附加变量图或部分回归图。它显示了一个变量保持所有其他变量不变的影响。这显示了当试图预测 D21_7 时治疗*D7 之间的关系。

![](img/07920cd22d8aec704436aa17c1d08cb6.png)

component_residual 图是部分残差图。就像附加变量图是分量残差图一样，这是一种显示两个变量之间关系的方法，同时保持其余变量不变。残差好看！

```
# Day 34 - Day 7 
fit_D34_D7<-lm(D34_7~D7*Trt, weights = Number, data=Marc)
summary(fit_D34_D7)
plot(fit_D34_D7)
influencePlot(fit_D34_D7)
avPlots(lm(D34_7~D7*Trt, data=Marc))
crPlots(lm(D34_7~D7+Trt, data=Marc))
```

![](img/218c460074c687328964e164b6f75ea8.png)![](img/702c259c4bb8d4ec4a00e2ff29457fb1.png)![](img/34fe9aa07977e14c5fa238ce603c71a0.png)

```
# Day 42 - Day 7 
fit_D42_D7<-lm(D42_7~D7*Trt,weights = Number, data=Marc)
summary(fit_D42_D7)
plot(fit_D42_D7)
influencePlot(fit_D42_D7)
avPlots(lm(D42_7~D7*Trt, data=Marc))
crPlots(lm(D42_7~D7+Trt, data=Marc))
```

![](img/cae9fdb6652c0262ba1c5f5174e6f8c2.png)![](img/e575dd3b001694ac8d3afffbc2531320.png)![](img/d4f81870be15cb2403af78b77b1789a3.png)

好的，所以我们从基线分析做了一个改变，这似乎暗示了治疗的差异(强调暗示)。让我们回到 box-cox 变换，并尝试找到一种方法来复制我们在上面看到的。

```
par(mfrow=c(2,1))
fitlm<-lm(FCR~ns(Time_center,2)*Trt, data=GUISSONA)
boxcox(fitlm,lambda=seq(-2,2,by=0.1),plotit=T)
bc1<-boxcox(fitlm,lambda=seq(-2,2,by=0.1),plotit=T)
which.max(bc1$y)
LV<-bc1[1]$x[as.numeric(which.max(bc1$y))];LV
fitbc<-lm((FCR^LV-1/LV)~ns(Time,2)*Trt, data=GUISSONA)
fitlog<-lm(log(FCR)~ns(Time,2)*Trt, data=GUISSONA)
par(mfrow=c(1,3))
plot(fitlm, which=2)
plot(fitbc, which=2) 
plot(fitlog, which=2)
summary(fitbc)
summary(fitlm)
```

![](img/0b7fe7444f55657772a01e1fdd09b5a7.png)![](img/4980799c17a6306dedee048aa639fa07.png)

转变似乎是必要的，而且一旦实施，似乎会有所帮助。不过，这真的没有那么令人信服。但是当你看到治疗之间完全没有区别时，你还能指望什么呢？

让我们转到 FCR。在最早的情节中就有差异的暗示。我要去看看是否真的是这样。

```
## LMER
fitlmer<-lmer(log(FCR)~ns(Time_center)*Trt+(Time_center|Pen), data=GUISSONA)
plot(fitlmer) # bad fit 
summary(fitlmer) # absolutely no variance in the data
plot_model(fitlmer)
bwplot(resid(fitlmer)~GUISSONA$Trt)
bwplot(resid(fitlmer)~GUISSONA$Pen)
bwplot(resid(fitlmer)~GUISSONA$Time_fact)
```

![](img/87027efda0d5579dbda5dba30bf94ac1.png)![](img/04a944a14fc17a4ed447748a6ad01187.png)

治疗差异的暗示确实存在。

![](img/3e7354d0a8b57827c565cd9f045d0ed6.png)![](img/b5c0000eb543cf6f2883fbf93c569a49.png)

```
## LM
fitlog<-lm(log(FCR)~ns(Time_center,2)*Trt, weights=Number, data=GUISSONA)
summary(fitlog)
par(mfrow=c(3,2))
plot(fitlog)
Anova(fitlog, type=3)
influencePlot(fitlog)
```

![](img/57ee98260a92a278cbef972c8a29a379.png)

尽管模型还有许多不足之处。

这里我们也要改变基线。也许这将创造一个更稳定的模式。然而，如果混合模型不运行，我也不会真的赌基线变化模型，但还是让我们做这个练习。

```
Marc$FCR21_7<-Marc$FCR21-Marc$FCR7
Marc$FCR34_7<-Marc$FCR34-Marc$FCR7
Marc$FCR42_7<-Marc$FCR42-Marc$FCR7

fit_FCR21_FCR7<-lm(FCR21_7~FCR7*Trt, weights = Number, data=Marc)
summary(fit_FCR21_FCR7)
par(mfrow=c(3,2))
plot(fit_FCR21_FCR7)
influencePlot(fit_FCR21_FCR7)
par(mfrow=c(3,1))
avPlots(lm(FCR21_7~FCR7*Trt, data=Marc,id.n=3, id.cex=0.75))
par(mfrow=c(3,1))
crPlots(lm(FCR21_7~FCR7+Trt, data=Marc,span=0.7))
dev.off()
qqPlot(fit_FCR21_FCR7, labels=row.names(Marc), id.n=3)
influenceIndexPlot(fit_FCR21_FCR7, vars=c("Cook", "hat"), id.n=3)
spreadLevelPlot(fit_FCR21_FCR7)
ncvTest(fit_FCR21_FCR7)
summary(powerTransform(fit_FCR21_FCR7))
```

![](img/9874cf707056742ed81fbc19d526da7e.png)![](img/150aedb3e97de23dbcbfa17a9e7bc5dd.png)![](img/4f5b0978a7af4073f9f34ae798350dab.png)

看起来更好，但没有治疗差异。

![](img/025e5823b766fdb302f76e987edddcdc.png)![](img/5253155511d6d3a41168b3422e2a1c54.png)![](img/51029028ca494e41352cba5a01c147dc.png)![](img/e3760cb21cc6222be02a1428fd5e5b83.png)

```
# Day 34 - Day 7 
fit_FCR34_FCR7<-lm(FCR34_7~FCR7*Trt, weights = Number, data=Marc)
summary(fit_FCR34_FCR7)
par(mfrow=c(3,2))
plot(fit_FCR34_FCR7)
influencePlot(fit_FCR34_FCR7)
par(mfrow=c(3,1))
avPlots(lm(FCR34_7~FCR7*Trt, data=Marc,id.n=3, id.cex=0.75))
par(mfrow=c(3,1))
crPlots(lm(FCR34_7~FCR7+Trt, data=Marc, span=0.7))
dev.off()
qqPlot(fit_FCR34_FCR7, labels=row.names(Marc), id.n=3)
influenceIndexPlot(fit_FCR34_FCR7, vars=c("Cook", "hat"), id.n=3)
spreadLevelPlot(fit_FCR34_FCR7)
ncvTest(fit_FCR34_FCR7)
summary(powerTransform(fit_FCR34_FCR7))
```

![](img/30325bf8339dacdeca1a6679601988b8.png)![](img/4756e51e4d3ea0456762abc23bc5a091.png)![](img/f8419e7f5037a488714543fc235f586d.png)![](img/ba8cb4fa5940ba943dafc8099c88e231.png)![](img/db020b9e582134ab59af2510f5873074.png)![](img/f0018f401be7a7c44bbdc8b11b4f77e0.png)![](img/4c50d47a8e4f45b8fd15bb8d7882d4c1.png)

```
# Day 42 - Day 7 
fit_FCR42_FCR7<-lm(FCR42_7~FCR7*Trt, weights = Number, data=Marc)
summary(fit_FCR42_FCR7)
plot(fit_FCR42_FCR7)
influencePlot(fit_FCR42_FCR7)
avPlots(lm(FCR34_7~FCR7*Trt, data=Marc,id.n=3, id.cex=0.75))
crPlots(lm(FCR34_7~FCR7+Trt, data=Marc, span=0.7))
qqPlot(fit_FCR42_FCR7, labels=row.names(Marc), id.n=3)
influenceIndexPlot(fit_FCR42_FCR7, vars=c("Cook", "hat"), id.n=3)
spreadLevelPlot(fit_FCR42_FCR7)
ncvTest(fit_FCR42_FCR7)# ROBUST REGRESSION
# Day 21 - Day 7 
fit_RR_FCR21_FCR7<-rlm(FCR21_7~FCR7*Trt, weights = Number, data=Marc)
summary(fit_RR_FCR21_FCR7)
f.robftest(fit_RR_FCR21_FCR7, var = "Trt2")
plot(fit_RR_FCR21_FCR7)
influencePlot(fit_RR_FCR21_FCR7)
avPlots(lm(FCR21_7~FCR7*Trt, data=Marc))
crPlots(lm(FCR21_7~FCR7+Trt, data=Marc))# Day 34 - Day 7 
fit_RR_FCR34_FCR7<-rlm(FCR34_7~FCR7*Trt, weights = Number, data=Marc)
summary(fit_RR_FCR34_FCR7)
f.robftest(fit_RR_FCR34_FCR7, var = "Trt2")
plot(fit_RR_FCR34_FCR7)
influencePlot(fit_RR_FCR34_FCR7)
avPlots(lm(FCR34_7~FCR7*Trt, data=Marc))
crPlots(lm(FCR34_7~FCR7+Trt, data=Marc))# Day 42 - Day 7 
fit_RR_FCR42_FCR7<-rlm(FCR42_7~FCR7*Trt, weights = Number, data=Marc)
summary(fit_RR_FCR42_FCR7)
f.robftest(fit_RR_FCR42_FCR7, var = "Trt2")
plot(fit_RR_FCR42_FCR7)
influencePlot(fit_RR_FCR42_FCR7)
avPlots(lm(D42_7~FCR7*Trt, data=Marc))
crPlots(lm(D42_7~FCR7+Trt, data=Marc))
summary(rlm(FCR42_7~FCR7*Trt, data=Marc)) # difference in value at day 42 between treatments, setting level at day 7 at mean
```

其余的也什么都没显示。因此，尽管我们从最初的情节中知道了结果，我们还是做了大量的工作。有时候，作为一名研究人员，你必须抵制住诱惑，提前收工。

我希望你喜欢这篇文章！