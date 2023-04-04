# DA 组合项目-ETL/数据库设计/ MySQL(第四部分)

> 原文：<https://blog.devgenius.io/da-portfolio-project-etl-database-design-mysql-part-iv-359fc03b9d89?source=collection_archive---------6----------------------->

![](img/7bc197ebb622d3b48339b50b9b908736.png)

[安德鲁·尼尔](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

在最后一部分中，我们设计了数据库并将数据加载到其中。今天，我们将继续使用 SQL 进行 EDA。所以，启动你的 MySQL 工作台，让我们开始吧。

在继续之前，查询已经在我的项目文件夹上链接 [***这里***](https://github.com/Armonia1999/Database-design-ETL-MySQL) 。我建议在一个单独的标签中打开它们并浏览它们，我已经有很多解释了，所以试试吧。

提醒:一篇很长的文章在等着你。

要开始查询我们的数据库，我们需要首先使用它，因此我们的查询将应用于它的表:

![](img/50f43fb7c7418dec829dd2b3e76e1b32.png)

现在让我们看一下我们的表:

![](img/e4a9a53f9915bb83f1a8a08a776b88ca.png)

我们限制在 20 个，因为我们不需要看到绝对的一切，浪费时间和空间。

我们的公民表看起来像什么，例如:

![](img/a618d7e20280aef213e113c9c42a7fa9.png)

如此感人的一幕…

接下来，我们的表格有多少行和列？

![](img/710bce543ffc86117b451483c2fe8cb1.png)

对 citizens_CH 的结果是什么？让我们运行它:

![](img/f16de2e942b3eb70c43bafd0eb53eece.png)

天哪，我们有 43k+行，有多少列？如果你运行它，它应该是 8。

现在，我们通常还会检查是否有空值或重复值，但是因为我们收集了自己的数据，所以我们知道事实上我们没有这些值:)

我想改变一些东西，我们的表 citizens 和 citizens_CH 在一列中有全名，让我们把它们分成姓和名。在做不必要的改变之前，我们需要确定我们能做到:

![](img/4efea085c7bdaf4a77886b945eeb5ced.png)

鼓点，有用吗？

![](img/24250d8b79e502d85de54b472f6fe067.png)

是的，是的，噪音。现在让我们把它添加到我们的表中。

![](img/29563b59329a2cf0b330f25b135c0847.png)

因此，在上面的行中，我们直接在 full_name 列后面添加了列 surname 和 given_name，这样看起来更美观，默认值是一个空字符串。是时候填充它们了:

![](img/fac9c1f522f16fb4c7fbb984797ee6f8.png)

我们会得到这个:

![](img/7ac2c1767bfc80be2d788d70d80e3242.png)

就像梦里的场景一样。

从现在开始我们将以提问的形式继续，这样会更有趣。

![](img/fcfc74df2413a20d43c2a64056b7e7f3.png)

这是我们得到的结果:

![](img/329462653aa7c03145cd62bac0b7b893.png)![](img/cc8b5115a3fa5cc7fdc732a690bb021b.png)

首先，让我们运行第一个查询:

![](img/b11207d4997c8231bbadf9218f88c886.png)

所以我们有 14 个联盟，第二行是看看我们的教派表中有多少个联盟。但是为什么只有门派 ID 小于 31 的地方？如果你还记得当我们使用 Python 设计数据库时，我们不想使用我们收集的所有 90 多个教派，我们只将我们的公民划分为 30 个教派，证据在这里:

![](img/59b7be306b6af2d491b08b636827e739.png)

所以这就是为什么我们想知道我们的教派属于多少联盟？

![](img/a7a95be0b69e7bc0e0f953075504765a.png)

你看到了吗？在所有联盟中，当我们选择随机教派与随机联盟结盟时，只有 12 个被使用。还有另外两个基本不属于任何教派。是的，我们只有 30 个教派:

![](img/db84623a82a9f5d0285aed0e8655746f.png)

Okie dokie，问题 3:

![](img/1fcc48534df3675cab7e882be984b6fa.png)

那就是:

![](img/6c504338a95940d43404bb3897335fdf.png)

哇，穿制服真漂亮。NEXTTT:

![](img/d89d9303c4a3f6ee8f306a807ec01cb5.png)

那就是:

![](img/cf929c3ebf301b7133cfe35beff05d24.png)

嗯，有意思。让我们看看第五个问题:

![](img/1eb4e953b50f340adfcd88c4b6619b13.png)

多少？

![](img/123c7a1c90ae9f345c7399771c910ae8.png)

荣跃森派？？？有点傻。

![](img/c95686e5e1b41859462b6d7b2eef24ca.png)

好的，兄弟姐妹们，哪个联盟最强？

![](img/64c0018534097250fae0f27a66f2fe49.png)

很好，我就知道我能指望驯兽师。这让我们想到:

![](img/68edc63b7a0598d38a1d44a9f5b9ef70.png)

是的，这是陈诚想知道的一些信息:

![](img/2c5a5d34ed6fd47d42e3027a505a5233.png)

我没想到这个名字会有任何降低。

![](img/a2fe1787f7e483aa4263dbe0602e3f48.png)

好吧，别让我们等着，什么事？

![](img/adabb284f9c8643ff9ba2391bce412f0.png)

> Qin 钦 / 欽 Meaning: **to respect, to admire, to venerate, by the emperor himself**.

![](img/cc4ba6882c8af24484c25e7a98cbea60.png)

…

![](img/fa702467a093f3e4ceac3b7b586b75b1.png)

酷毙了。现在让我们继续讨论紧迫的问题:

![](img/91b1ca9c66a7cbd91ad700706116cae5.png)

是的，那是我们需要的调味汁。

![](img/1a7f1c2007c0d7bcf1c6a5d7cd252ffb.png)

我们所有的眼睛都在看着你，亲爱的，我在看着你..

![](img/d27198175caa4a203c0ebf07a4c5b6eb.png)![](img/85a2af1882c8c83509fe493ced0d9f80.png)

咻让我颤抖吧..

![](img/b1d56bb328118cc7bf1291a47b2dd1a0.png)![](img/efc1f696bb33462fa92873383ff61d18.png)

第三，我又见面了..太多的巧合..

情节转折:

![](img/5cc41ffc4558949710790435d2af6b9d.png)

好的，让我们看看我们能做些什么。一些窗口函数应该可以做到这一点。

![](img/97d22d3ca82f797c25e65483b938354e.png)

有用吗？

![](img/eb0f58b9f61b90cd2107720f5286acac.png)

看起来是这样。让我们更改表格并分配它们:

![](img/0d161cddd7abec2506f350518b1de128.png)![](img/aca24a1e5b4ac04a86184d0466e8d6f1.png)![](img/ed46e34f3b566d264565da1d559c89b2.png)

好了，现在我们做到了，我们的教派表看起来怎么样？

![](img/ce285cf8f0cd2fef301eaa567adb10ee.png)

好看。

![](img/c5596aa8d93c2e8e7ee6fc6d568971f2.png)![](img/c652575bf9af809188cf76ec37c191d8.png)

完美。没有胭脂门主。现在让我们将他们在公民表中的排名改为 10:

![](img/1b69b8fefcfa9aff5cb5eb4c1e5dc9ca.png)

我们已经接近终点了，至少我们是这样认为的！

![](img/3205b02a0a15de221f9c84f848e71bac.png)

好，先加他为公民:

![](img/953180cb3ec498d3959e99d24df09aac.png)![](img/21e5a0784b920ecb9bdb37ee71378f63.png)

我们知道我们有 9979 个公民，所以他的 ID 是 9980。既然我们已经把他添加为公民和门派掌门，那就让所有胭脂人都追随他的门派吧。

![](img/e8e0156ca4afca86403275dfa536034e.png)

好吧，让我们给他一些武器:

![](img/da66560e445feb7da4970b90fa2bda09.png)

呵呵，既然他寻求扩张，那我们也创建一个他唯一门派所属的联盟吧:

![](img/a73f1e765a7f518f763079ee0d900920.png)

今天到此为止。也许下一次我们可以进行教派间的战争，让它变得更有趣，但不是今天。我想做的最后一件事是选择我想要的数据，并将其导出为外部 txt 文件，然后从那里加载到 excel 并构建一个仪表板。我们就是这样做的:

![](img/52f481fce8a516a8559632c7c8b0912c.png)

最终文件已经在我的 Github 上了，你可以直接构建一个仪表板。

先睹为快？

![](img/0b03bf3ff850be628c297cd1e8375987.png)

你只要看到左下方的万惠 Logo 就知道我付出了多少努力。我们还有一个过滤器，可以看到某个门派的具体信息。点击这里的链接[](https://public.tableau.com/app/profile/armonia8525/viz/WanhuiSociety/WanhuiDashboard)*，你可以体验一下它是如何工作的:)*

*希望你喜欢这个项目！这是最后一部分，我们将分开，直到我找到一个新的项目哈哈。*

*感谢您的阅读和…*

*安静点。*