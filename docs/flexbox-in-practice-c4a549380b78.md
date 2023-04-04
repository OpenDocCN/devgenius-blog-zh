# 实践中的 Flexbox

> 原文：<https://blog.devgenius.io/flexbox-in-practice-c4a549380b78?source=collection_archive---------3----------------------->

![](img/252f3bee44da57e9b10834b1edec0c46.png)

照片由 [KOBU 机构](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

CSS 开发教程

正如在这篇文章中所承诺的，你和我将使用我们在上一篇文章中学习的 flexbox 属性。我将做 3 个假设:

1.  你有一个移动设备。笔记本电脑或手机。是的，你可以用手机编码。如果你不相信我，点击[这里](https://www.youtube.com/watch?v=7UhlMB7B1jg)。
2.  你知道怎么打字。如果没有，在你的键盘上，简单地按下与屏幕上显示的符号相似的按钮
3.  您有一个自己熟悉的代码编辑器。我将使用 VS 代码。
4.  你对 HTML 和 CSS 有基本的了解

# 我们开始吧

开始之前，我们首先要按照以下 3 个步骤来搭建我们的游乐场:

1.  创建一个新的目录(文件夹)并命名为“练习”
2.  在这个目录中创建一个新文件，并将其命名为“index.html”
3.  最后，在同一个目录中创建一个新文件，并将其命名为“style.css”

好了，终于到了编码时间:

首先，在我们的 HTML 文件“index.html”中。让我们将下面的代码输入其中。

![](img/1fb4ece9d837fe5aa25e6ec871256ea7.png)

*   上面，我们制作了一个基本的 HTML 文件，在链接到我们之前创建的“style.css”文件的`head`标签中有一个`link`标签。然后在`body`标签中，我们制作了一个容器`div` 标签，其中有 5 个项目也是`div`标签。

现在来看我们的 CSS 文件，主要的内容将在这里发生

*   第二，在我们的 CSS 文件中。我们从输入基本重置开始。那就是:

![](img/24d74f60e4a93cc534f51718633993db.png)

*   接下来，我们给我们的容器一个漂亮的背景颜色和漂亮的填充。

![](img/aaeb2de08556abc0ddf55a677882ca36.png)

*   之后，我们给我们的项目很好的填充，空白，背景色，颜色和字体大小

![](img/06d2289a72ce292ae61cc9e635d74288.png)

*   因此，从上一篇[文章](https://tariyekorogha.medium.com/what-is-css-flexbox-f0ef19726282)中，我们了解到，为了让 flex 容器能够访问它的 flexbox 功能，您必须将其显示设置为 flex。

![](img/7c329cc1111e527420370f37f5662ca1.png)

在将容器的显示属性设置为 flex 之前

……………………….👇👇👇👇👇👇👇👇👇……………………………

![](img/a8e1aa2b95a122ac4f857ca78e85cc77.png)

*   我们之前学过的[默认容器和项目属性](https://miro.medium.com/max/1400/1*3ekKP-fAzePQwfPR_ImYNA.png)立即生效。

………………👇👇👇👇👇👇👇👇👇👇👇👇………………………

![](img/09a554e46d2f9698852e011d5b532d64.png)

从容器属性开始

*   将 [*伸缩方向*](https://miro.medium.com/max/1400/1*sNU-u7Bl2GkQClkcc_LO5g.png) 设置为**行**即主轴方向从左到右
*   [*柔性缠绕*](https://miro.medium.com/max/1400/1*ARQzw_lFZafwi2yXTFryKw.png) 设置为 **nowrap**
*   [*justify-content*](https://miro.medium.com/max/1400/1*eHm0NgBmV0AaZudRUPIgSw.png)被设置为 **flex-start** 即在 flex 容器的开始处排列的 flex 项目
*   [*对齐-项目*](https://miro.medium.com/max/1400/1*CeLaQhE4_wKhNOigM6ic1g.png) 设置为**拉伸**
*   [*对齐-内容*](https://miro.medium.com/max/1400/1*Q5aTc5Dm8UF1wDW1ur5MHg.png) 设置为**拉伸**

我们将使用其中的 3 个容器属性，我们要检查的第一个属性是

1.  *伸缩方向*:默认值为**行**，伸缩项从伸缩容器的起点开始从左到右依次排列。

![](img/34fd846a05ef5f0ef2cc0049b8f47d49.png)

*   让我们看看如果将它的值改为**行反转**会发生什么

![](img/64144b21de36bc83fb6c5b8c0844258a.png)

……………….👇👇👇👇👇👇👇👇👇👇👇👇………………………..

![](img/41157fd848c32de717a0dbe727e4175a.png)

将“弯曲方向”设置为“行反转”后

*   主轴的方向现在是从右向左。flex 项现在从 flex 容器的末尾开始。

![](img/f731eafffc5858ffc006276daaf162aa.png)

*   接下来，让我们看看如果将它的值更改为**列**会发生什么

![](img/c51bf623986a908a5b507fea034e5b1d.png)

…………….👇👇👇👇👇👇👇👇👇👇👇👇👇👇…………………

![](img/a41668c7b660892d079e5d5929069fe8.png)

将“伸缩方向”设置为“列”后

*   啊，flex 项目的主轴从上到下，一个叠一个。(这与我们在将容器的*显示*设置为 **flex** 之前开始时的项目完全相同)

![](img/5a4dadfd7df062a237a95faef3d691a7.png)

*   最后，让我们看看如果将它的值改为**列-反转**会发生什么

![](img/b322a12f4f236337a583f45e4e1693e4.png)

…………….👇👇👇👇👇👇👇👇👇👇👇👇👇………………………..

![](img/fee9a1ad82e5c14cd2327c61beb4092b.png)

将“伸缩方向”设置为“列反向”后

*   flex 项目的主轴方向现在是从底部到顶部，仍然堆叠在彼此的顶部。

![](img/223a5e4e09b6da0a139088f10063ba5b.png)

我不知道你怎么想，但我觉得这太酷了。让我们转移到另一个属性

*2。justify-content* :如果您还记得上一篇文章，这个属性用于控制项目应该如何沿着主轴定位。这个属性的[默认值是 **flex-start** ，这就是为什么 flex 项目从 flex 容器的开始处开始。](https://miro.medium.com/max/1400/1*eHm0NgBmV0AaZudRUPIgSw.png)

![](img/0f266e5fa85c8a22103cbeafd19e9040.png)

*   让我们把数值改成**中心:**

![](img/8581480e7548ed21b2f9d836d80bc853.png)

……….👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇……………………

![](img/234bc0a2ed4ffcdc6fc4d3c4a47dce7b.png)

啊啊，物品被自动放置在容器的**中心**。

**注意**:项目之间的空间是因为我们之前定义的“*边距* :30px”而存在的。如果我们把它移走，容器之间就没有任何空间了。

![](img/afb3fff95e87af9eb49d3cd0b182ba34.png)

……….………….👇👇👇👇👇👇👇👇👇👇👇👇…………………….

![](img/30ba789aa1bc72963054051f22f56738.png)

因此" **center"** 值的作用是将伸缩项放在容器的中心

*   好的，让我们把这个值改成**空格-在**之间

![](img/c3ada4b79fc514ff3ae3231e63f42d75.png)

…………………👇👇👇👇👇👇👇👇👇👇👇👇…………………..

![](img/bc43e6332523ea6832a0c81376589aa5.png)

哇，伸缩项之间的空间**均匀分布**。你能相信 flex box 会自动为我们完成所有这些计算吗？🤯

![](img/8b32787e400f16452043ffca7d47060c.png)

另一件很酷的事情是，所有这些都是有反应的。如果你改变了浏览器的宽度，顺便说一下，这些项目也会自动调整。

![](img/8fa3d21f88e186b9e21c414fbc402c09.png)

**奖励**:再次试着去掉边缘，看看会发生什么

*   让我们尝试一下**空格周围的**值。

![](img/da612edc451d6a37cce5aa16cfd69231.png)

………………..👇👇👇👇👇👇👇👇👇👇👇👇👇👇…………………

![](img/7a1448f795fd3f33962f0fc94e4c36c8.png)

所以，虽然上图看起来和之前的图没什么区别。此值在每个伸缩项的左侧和右侧放置相同的空间。这意味着任何两个项目之间的空间是第一个或最后一个项目前面空间的两倍

![](img/aab29788060188942d9a04b4d6ffb0f1.png)

**注意**:我的箭头和我的文字不匹配，因为我们增加了页边空白。试着去掉边距。

*   下一个值是**空间均匀分布的**值。

![](img/18c577f57438c3d60ed2f4a641002f01.png)

……………👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇……………

![](img/196e4ce3c490c2c06b7992e6660cd883.png)

因为我们看不到，实际上上面的图片看起来和其他的没有什么不同😂。但是这个值只是确保任何 flex 项目前面和之间的空间总是相同的。

![](img/4d8f1b0440f582a93247c9c195893c1c.png)

**注意**:我的第一个和最后一个箭头还是和我说的不一致，因为我们加了边。试着去掉空白

**快速问答**:btw**等间距**和**等间距**有什么细微的区别？

*   最后，让我们试试值**挠性端**

![](img/a5d5b50ebdf87134fbd29148958428be.png)

……………👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇……………

![](img/41de77b9f556285cf6e1ef59dc219f29.png)

伸缩项位于容器右侧的末端。只是为了好玩，让我们看看如果我们将*弯曲方向*设置为**行反向**会发生什么

![](img/260bf98dd91f579741daca7a6698e9b4.png)

……………👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇……………

![](img/84d8242843aea650ea9bb2e31b79e15e.png)

很好，希望你能明白为什么上面的图片是这样的。如果没有重读关于[](https://miro.medium.com/max/700/1*sNU-u7Bl2GkQClkcc_LO5g.png)*的要点*

**3。align-items* :该属性定义项目如何沿横轴对齐。*

*![](img/bd2b14af1d3ed90d48e3fb29d3c8d133.png)*

*将我们的代码设置回第一阶段，只需将显示设置为 flex，就可以让我们的项目完全对齐。因此，为了实现这一点，我们需要将其中一个项目变大。因此，在项目 2 上创建一个名为“i2”的新类，并将其高度设置为 200 像素*

*![](img/7446d26f0c7cee978d5d107fe856aa56.png)*

*第一步*

*![](img/6a58757620a5ca14d5e586024861dedd.png)*

*第二步*

*有一个立竿见影的变化，所有的物品也长高了*

*![](img/0213876094fa9ced2600afac17cbc457.png)*

*这是因为 *align-items* 的[默认值被设置为**拉伸**，在**拉伸**时，就会发生这种情况](https://miro.medium.com/max/1400/1*CeLaQhE4_wKhNOigM6ic1g.png)*

*   *让我们试试**中心**。即将*校准项目*的值设置为**居中***

*![](img/ed73434202b4e2523d368a4d9bbdd566.png)*

*……………👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇……………*

*![](img/8ef810c188e5106e591c83f5a05d4234.png)*

*酷，这一次使用**中心**相对于[横轴](https://miro.medium.com/max/1400/1*YwfH9TyTOgyQU1wDSh3f5w.png)和最大项目将柔性项目设置到容器的**中心**。*

*   *让我们试试**弹性启动***

*![](img/0bd9d5186535116ae3bdd539f1868476.png)*

*……………👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇……………*

*![](img/cab3d90384a37b2e015c1f10b59d6a3d.png)*

*好了，现在所有的 flex 项目都在 flex 容器[的顶部相对于横轴](https://miro.medium.com/max/1400/1*YwfH9TyTOgyQU1wDSh3f5w.png)很好地对齐了。对于**挠性端也是如此。***

*![](img/2bb3ef70c2e0c596661d9f876eda30f2.png)*

*……………👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇👇……………*

*![](img/688790498e00e51edf05b2af701ad8a0.png)*

> *别忘了所有这些都是沿着 [**横轴**](https://miro.medium.com/max/1400/1*YwfH9TyTOgyQU1wDSh3f5w.png) 发生的*

*   *最后一个值称为**基线。**为了理解这个值，我们必须增加任何项目**的*字体大小*。***

*![](img/57a19415dc619ec9b3c1235eda4ca739.png)*

*第一步*

*![](img/81e1fddb196d3081fa1b37e419eb90f0.png)*

*第二步*

*![](img/f5d25520948720b84fbb819eefce1985.png)*

*现在我们来试试。*

*![](img/ad4e8e15db57490bc8bf94738c4b58d5.png)**![](img/928edfe0f33316bb6e139850f666c1db.png)*

*啊啊，它将 flex 项目上的文本对齐一行。所以如果我们沿着文本画一条假想的线，我们会看到它都是对齐的。所以在…*

# *结论*

*这就包含了我希望你和我一起处理的属性，因为它们是最常用的 flexbox 属性。剩下的你可以自己去查。这里使用这些链接来阅读它们( [*justify-content*](https://www.w3schools.com/cssref/css3_pr_justify-content.asp) ， [*align-items*](https://www.w3schools.com/cssref/css3_pr_align-items.asp) ， [*align-content*](https://www.w3schools.com/cssref/css3_pr_align-content.asp) )。别担心，这不是结束，这只是暂时的告别。在下一篇文章中，我们将讨论 flex 项目的属性和值。那里见**👈(此外，如果您有任何问题，请随时在**[**Twitter**](https://twitter.com/MyhnTari)**上问我，或者如果您需要认真的辅导，您可以在这里联系我**[](https://www.teacheron.com/tutor-profile/4Vsh)****)*****

# ***参考***

1.  ***[“高级 CSS 和 Sass”作者 Jonas Schmedtmann](https://www.udemy.com/course/advanced-css-and-sass/?utm_source=adwords&utm_medium=udemyads&utm_campaign=LongTail_la.EN_cc.ROW&utm_content=deal4584&utm_term=_._ag_77879424134_._ad_535397245863_._kw__._de_c_._dm__._pl__._ti_dsa-1007766171312_._li_1010294_._pd__._&matchtype=&gclid=Cj0KCQjwtvqVBhCVARIsAFUxcRsvicV6qzlz9NYn6o1JhNWmJFEh07YAWqSrVbESYKTejoMGqpU3tQ0aAgm8EALw_wcB)***