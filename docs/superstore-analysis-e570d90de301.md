# 超市分析

> 原文：<https://blog.devgenius.io/superstore-analysis-e570d90de301?source=collection_archive---------1----------------------->

## **探索性数据分析和 R 可视化**

![](img/c8bcf5b683a85c4ce6a5888a5a4a3c6d.png)

[卡格尔](https://www.kaggle.com/datasets/abiodunonadeji/united-state-superstore-sales)

# **简介**

洞察使您能够更好地了解您的业务，并根据数据分析进行优化。这个项目侧重于使用 R Studio 中的 R 编程进行探索性数据分析和可视化。

这个项目是基于客户、销售、利润、产品、品类甚至时间序列分析。

这个项目中使用的数据集是从 [tableau 社区](https://community.tableau.com/s/question/0D54T00000CWeX8SAL/sample-superstore-sales-excelxls)下载的。这是一个美国超市销售的样本数据集

# 加载库并将数据集导入 R Studio。

在我们继续进行分析之前，我们将加载库，导入**超市销售额(Excel)。xls dataset，**数据清理，快速了解我们将要处理的数据。

# 加载库

![](img/d8683949ce1a7f13fd3518e71a305efc.png)

让我们导入我们的 Excel 文件，并将该文件作为 ***超级商店*** 存储在对象中

![](img/b9ed756a3249bfa13f535ca821abd551.png)

# 让我们快速了解我们的变量和数据框架，并做一些数据清理

![](img/929bf3326ba5c975dd7198b4bde816f6.png)

密码

![](img/ec4870045c852ef4c51fd425308350ff.png)

结果输出

![](img/d6e82fce05b609ccd7bdb039c5ac720d.png)

密码

![](img/42e99471ba6e93091e0491c558ecd137.png)

结果输出

从上面显示的结果来看，变量命名是不一致的，我们需要清理它

![](img/19b2bcd02120fe3e3fee9e60752340ff.png)

密码

让我们检查列名，确保变量名现在是一致的

![](img/344077e51e8a9de5af4f3e4b302ea626.png)

检查变量名

![](img/186daeef5f6c17ae9dfe60efb9ad20e0.png)

结果输出

变量名现在是一致的。

![](img/1dc2035f6ce0954c9b699ee0a8ba1c36.png)

密码

![](img/ea35351d573223ece0538fc29f1777e5.png)

结果输出

在数据框中有 9994 行或观察值和 21 列或变量。

![](img/62d6ac3d48635768f81e47779a94a908.png)

密码

![](img/fd51220edfaa520dfff3f24afadcf2a1.png)

结果输出

数据框中没有缺失值

在我们的分析中，我们不需要 **row_id** 和 **country** 变量。现在，我们将继续删除 row_id 和 country 列(变量),因为数据框仅适用于美国

![](img/fd3fd85f461db190928ef60c26c5463d.png)

密码

使用 ***colnames()*** 函数确认删除 **row_id** 和 **country** 变量。

![](img/26c643cbd37aab37692ec9b6cf642765.png)

结果输出

# 分析

超级商场的分析将根据以下内容进行。

1.  产品级别分析
2.  客户层次分析
3.  区域和时间序列分析

# 产品级别分析

![](img/6e6a416b7cb5e7c05a158090104bcc48.png)

产品类别代码

![](img/a7665e3ecbc5e778ec3c9f0beaf399c7.png)

产品类别结果

![](img/e95305e6c495b9fc4e047ea9812282e9.png)

密码

![](img/baee2332f2b970b45fed691b81d875fe.png)

结果输出

![](img/0b330c3fb39ee9c32c15a2c97f5af18b.png)

密码

![](img/8427d89be0780266449b173e3e37ae65.png)

结果

子类别产品分为 17 个。

![](img/30cc8f60853e23539929aeb3720a0283.png)

密码

![](img/e91df53cd20b943175a950a47ab559d0.png)

每个子类别中产品数量的结果输出

![](img/6f1e05902ed2ae3f6003eceb35ab12c1.png)

密码

![](img/d82d955843c9967461f9df84e50bd012.png)

结果输出

从上面的形象化描述中，人们可以很容易地决定从商店购买产品时要注意哪个类别和子类别

![](img/4867dcd0e4fb76794b611681380570ce.png)

密码

![](img/14d00db977bba8727a8a33a958620c43.png)

结果输出

这家商店有各种各样的办公用品，尤其是活页夹和纸张

![](img/ccca4ef51a682aa196f5d2ec5fc64d35.png)

密码

![](img/01e35ea6111230ac9baba8d643598c3f.png)

**结果输出**

与其他产品相比，复印机的利润最高，而椅子和手机的销量也很高。然而，桌子和书架的利润并不显著，因此，这些部门处于亏损状态。

![](img/392f770940be8efab4349b339fb2b0f1.png)

密码

![](img/c9e21f1bcc95d5323d9ed2d229902221.png)

结果输出

![](img/938855de7477a003d30217f6b15cb32a.png)

密码

![](img/388f5b36706b9d34036f064bdc98d5ea.png)

结果输出

![](img/e350c55a0a4f6d16cb969d11f04569f5.png)

密码

![](img/3abf523a3f0819a23a8b7b425b9ab18b.png)

结果输出

居住在美国西部的人们倾向于从超市订购更多的东西。

**为了进一步分析，我们将计算成本和利润百分比变量**

*   **计算成本**

![](img/43a63b14862ae22530102b869ea9da4f.png)

密码

现在，让我们根据成本找出十大产品

![](img/63f3e89001593f2405b7501279822ac9.png)

密码

![](img/a5d0dc058c531482c34bacd84e36eb14.png)

结果输出

*   **计算利润百分比**

![](img/42102d8784a37eb0f32738328428f8a7.png)

密码

现在，让我们看看利润为 100%的类别和产品

![](img/205f849f2de08bfe1b7a0fe884fdd561.png)

密码

![](img/79cc14aedc6a05b3500c30a4f7922760.png)

结果输出

三个品类中有 140 个产品获得了 100%的利润，然而，办公用品有更多的产品获得了 100%的利润，如下图所示

![](img/d426ad9b2333e2a102af96eec0943958.png)

密码

![](img/c8dfca6fb1263a7bebaf5c0e29b7cb5a.png)

结果输出

## 为了进一步分析，我们来看一下客户级别的分析

# **客户层面分析**

![](img/ebd21e5e0275e4b59c86e2b7765d4744.png)

密码

![](img/8a5725fccefd744c2f154148ad7d8363.png)

结果输出

总共有 793 名顾客

![](img/ed00041c953b7459dacc8f7640f9731c.png)

密码

![](img/31a789548fa3fae2149b6d797816219e.png)

结果输出

![](img/e5f4359335c15a217b532141e5ceb4bd.png)

密码

![](img/376fcfb55a2f5f5a29a00fe31fb21d77.png)

结果

这种分布在消费者群体中是最高的。此外，标准等级的船舶模式在所有航段中最高

![](img/0bc950ca40524fe14b79806b55e237b0.png)

密码

![](img/3fba3db630d6e6752a9837071c656994.png)

结果输出

最赚钱的顾客来自印第安纳州，其次是华盛顿。然而，我们可以看到大多数盈利客户来自纽约和密歇根州。

![](img/ffd7f2f9e98403db0e92cf67862b3300.png)

密码

![](img/56e9e89de5c6c0c94eb46aaef600f1b7.png)

结果

# 区域和时间序列分析

![](img/0a7ab12ee5d9c53e30fe03bea60db7e8.png)

密码

![](img/3f6711cf21ef5bf5d1c2966b9acf8fd3.png)

结果输出

西部地区销售额最高，总销售额为 725，458 美元

![](img/6b982ebf5072e07ff76e35f786ba1fd9.png)

密码

![](img/c0988a13f229cd01fabd7bcbe55a2893.png)

结果输出

西部地区的利润最高，总利润为 108，418 美元

## 我们还将计算装运持续时间，从数据帧的 order_date 中提取 year

*   **计算装运持续时间**

![](img/ad16a5135448ae3201d497a3276282c4.png)

密码

![](img/965e84e922a56ca4dd6134feb306bfe4.png)

密码

![](img/90a1c1b0d232ab584664582430b31386.png)

结果输出

从上面的结果来看，使用 ***标准等级发货*** ***模式*** 将产品从商店发货最多需要 7 天或一周时间。

![](img/843b9c784ab8ea4c4f2296e2bc199544.png)

密码

![](img/2359c5daac2823ca1957df5ab6dc6919.png)

结果输出

从上面的“当天发货”模式来看，在订购当天就可以发货，但是，标准等级的发货模式平均需要 5 天

*   **从订单日期中提取年份**

![](img/9e4bcc52312a8379de704eb287485d3d.png)

密码

![](img/b97a4e45348e194c953c4b9ecf48a489.png)

密码

![](img/2bc4fa9b12161250e5fe9767c9220dc4.png)

结果输出

以上显示，销售额和利润逐年增长，导致 2017 年的利润率较高

![](img/9617e3756f46b64d400cb9fc27f85a3f.png)

密码

![](img/6c141a6df492dc92dcfcd929b4a4cacd.png)

结果输出

以上显示，2014-2017 年全年，西部地区的利润总额和销售额最高。然而，在 2015 年，东部的利润和销售额超过了包括西部在内的任何其他地区。

![](img/3a38f8e92012c99fb939942d584661d7.png)

密码

![](img/76e1294301d038a309599453ea13a0fe.png)

结果输出

综上所述，科技产品在 2014、2016 和 2017 年的销量最高。然而，更多的家具产品在 2015 年售出。

![](img/d9e464bd3a8634059bfcdeb1d9da2db0.png)

密码

![](img/c778a9c10e499cf9ae7c45a9f7a56060.png)

结果输出

从 2014 年到 2017 年，办公用品和科技产品的利润持续增长。然而，2015 年和 2017 年家具产品销售利润大幅下降。

![](img/63a01762f6ef3270a1e2e96289e12fe0.png)

密码

![](img/163b4a8b1e929b8db0fd3e4f2d85d7ae.png)

结果输出

所有代码都可以在我的 [***Githup 资源库***中找到](https://github.com/abbeynet77/Superstore-Analysis/blob/main/Superstore%20Project.R)

非常感谢您的阅读。请 [**关注我中**](https://medium.com/@abiodunonadeji) 更多探索性数据分析项目及数据分析相关话题。注意安全！