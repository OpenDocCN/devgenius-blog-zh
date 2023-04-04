# 如何为 AWS 上的无服务器工作负载选择正确的计算节省计划

> 原文：<https://blog.devgenius.io/how-to-pick-the-right-compute-savings-plan-for-serverless-workloads-on-aws-33d351b573c7?source=collection_archive---------20----------------------->

![](img/84daa6516e6503b2e3c19b0c2be4c9ef.png)

迈克尔·朗米尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> 这篇文章最初发表在 [bahr.dev](https://bahr.dev) 上。
> [注册邮件列表](https://dev.us19.list-manage.com/subscribe/post?u=60149d3a4251e09f826818ef8&id=ad766562ce)，让新文章直接进入你的收件箱！

[计算节省计划](https://aws.amazon.com/savingsplans/faq/)是一种灵活的方法，通过承诺每小时的支出来降低您的 AWS 账单。然而，当我们考虑免费层、不同的工作负载、可用预算和您未来几年的计划时，找到正确的承诺可能会很棘手。本文描述了无服务器工作负载的可用选项，如何选择正确的计划以及如何改进现有计划。

# 先决条件

这篇文章适用于你，如果你愿意在 AWS Lambda 或 Fargate 上每月花费至少 30 美元至少一年的话。低于这个数额，很可能你会多付。只有当你超过每小时 0.10 美元或每月 72 美元时，AWS 才会开始给出建议。

为了有效地使用本指南，[您应该已经启用成本浏览器至少 2 个月了](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/ce-enable.html)。

本文重点介绍一个**单一账户**。如果你使用 AWS 组织，你仍然可以应用你在这里学到的东西，但是[检查关于多账户设置的文档](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html):

> *储蓄计划将首先应用于拥有该计划的帐户的使用，然后应用于 AWS 组织中其他帐户的使用。*

为了简化本文，我将忽略 EC2 和 DynamoDB。如果你对 DynamoDB 和 EC2 的低价感兴趣，可以看看 [DynamoDB 保留容量](https://aws.amazon.com/blogs/aws/dynamodb-price-reduction-and-new-reserved-capacity-model/)和 [AWS 的储蓄计划指南](https://docs.aws.amazon.com/savingsplans/latest/userguide)。

# 放弃

本文中的数字可能是错误的，因为它们是基于我对公开文档的理解。从一个小的储蓄计划开始，然后增加额外的计划以避免超额支付。

# 储蓄计划如何存钱？

有了储蓄计划，你就可以在使用电脑前支付费用，作为交换，你购买 Lambda 电脑的[费用在**降低了 17%，在**降低了 52%。在本章中，我们将把每小时承诺称为预付费计算。****](https://aws.amazon.com/savingsplans/pricing/)

购买后，储蓄计划会为您提供预付费计算包，Lambda 和 Fargate 等计算服务可以使用这些包。超出预付金额的任何额外计算将按常规按需费率定价。

![](img/83f5c2f5f22dd06152f198118a5f0963.png)

当计算使用量与预付金额相匹配时，除了已经支付的费用之外，您不会产生任何额外支出。实现这种完美的契合非常困难，因为在无服务器环境中，计算使用情况往往会有所不同。

![](img/6aa3a03c4e5479dd5626d26e6bd966ab.png)

当您的服务使用的计算量超过您已预付的计算量时，额外的计算量将按按需费率收取。这是自动发生的，你不需要再做什么了。

![](img/83c06ac2347271f7351d7973a9ee09b9.png)

当您的服务使用的计算量少于您预付的计算量时，剩余的计算量不会转移到下一个小时，而是会被丢弃。不管用不用，你都付出了承诺。如果您有不规则的工作负载(例如夜间工作)，或者免费层涵盖了您的大量支出，则可能会出现这种情况。

![](img/9c4d064f59017dbcb0529fea6fcdf050.png)

节约计划将显示为 AWS 账单上计算支出的减少。

![](img/83b765a9ed420a648e1e341ed7495e25.png)

下面你会看到一张我最近电脑支出的照片(过滤到λ)。虽然由于免费层，最初几天不会产生任何支出，但随后几天会显示支出和节省(底片中的红色条)。我的储蓄计划大约每天 0.6 美元。

![](img/de1b322f9ca3b26a94a1259ebb49c11e.png)

请注意，这并不意味着我每天节省 0.6 美元。相反，我提前购买了计算机，然后获得了比按需购买 17%的折扣。现在预付电脑降低了我的日常开支。

# 有哪些选择？

当选择一个储蓄计划时，有两个主要问题需要回答:你承诺多长时间，你想什么时候支付？

你可以选择 1 年或 3 年的期限。时间越长，您的 Fargate 费率就越优惠。对于 Lambda 来说，期限的长短似乎与定价无关。至于付款方式，您可以选择“不预付”、“部分预付”和“全部预付”。预付越多，您的价格就越优惠。

这些前期术语是什么意思？

*   无需预付:每月费用
*   部分预付:50%在你购买储蓄计划时支付，其余的按月支付
*   全部预付:购买计划时一次性付款

以下是 2020 年 6 月 28 日美国东部 1 号航班可能的储蓄率概述。请在购买前查看您所在地区的价格。

**λ**

**Fargate**

# 我如何选择正确的计划？

如果你的计算支出超过每小时 0.10 美元[，AWS 将为你提供节省计划的建议](https://console.aws.amazon.com/cost-management/home#/savings-plans/recommendations)，其中也考虑了可变的使用模式。在你自己计算之前，先看看那些数字。我的花费太低了，不值得推荐。

选择正确储蓄计划的关键是了解你最近和未来的支出。一旦你知道你经常花多少钱，我建议你先选择一个小数目。根据经验，你可以选择 50%的小时支出。只有当你掌握了小额储蓄计划效果如何的数据后，你才应该考虑投入更多。

# 了解你目前的支出

从[打开 AWS 成本浏览器](https://console.aws.amazon.com/cost-management/home)开始。如果您以前没有使用过，现在[启用成本浏览器](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/ce-enable.html)。

在图的顶部，选择 Daily 作为粒度，过滤服务以仅显示“Lambda”和“EC2 容器服务”(= Fargate)。如果您可以选择每小时，那就更好了，但是请注意[每小时的粒度会产生额外的费用](https://aws.amazon.com/about-aws/whats-new/2019/11/aws-cost-explorer-supports-hourly-resource-level-granularity/)。

![](img/5ee46f7258a0f10a73641b5036fb1b7a.png)

“每小时的承诺是储蓄计划费率，而不是按需消费。 [(AWS 文档)](https://docs.aws.amazon.com/savingsplans/latest/userguide/sp-purchase.html#purchase-sp-direct)”。这意味着当你看到每小时 1 美元的支出时，相应的每小时 17%的储蓄承诺是`$1 * (1-17%) = $0.83`。

让我们假设每小时的最低消费是 1 美元，或者每月 720 美元。我们正在寻找一年的承诺。所有的前期投资给了我们 17%的最优惠利率。每小时 0.83 美元的承诺使我们得到 0.83 美元 x 24 x 30 x 12 = 7，171 美元的单笔付款**。因此，**与按需费率(8640 美元)相比，我们节省了 1469 美元。****

当选择部分预付时，我们将承诺`$1 * (1-15%) = $0.85`，预付 3，672 美元，然后每月再支付 612 美元。计算节省了 15%,与按需费率相比，我们节省了 1，296 美元。

正如您在上面的图片中看到的，我的计算支出大约是每天 1 美元(不是一小时)，但我有 6 天是免费的。储蓄计划几乎毫无意义。

# 储蓄规划师

[储蓄规划师](https://savings.bahr.dev/)帮你找到合适的储蓄率。你上传你最近的成本报告，输入你的储蓄利率以及每小时的承诺。然后网站会告诉你它希望你存多少钱。

我买了一个储蓄计划，当我比较 AWS 上的利用率报告时，数字甚至比储蓄计划员的预期还要好。

该网站只是前端(没有数据发送到任何服务器)，并在 GitHub 上[开源。](https://github.com/bahrmichael/savingsplanner)

**自由层考虑**

*如果免费等级的费用一天都不够，那么您可以跳过这一部分。*

对于每月 30 美元或每小时 0.04 美元的计算机支出，我们可以认为每小时 0.04 美元的节省计划是合理的。但是，如果我们有空闲层覆盖的天数，我们将超额承诺每小时 0.04 美元或每天 0.96 美元。根据我的理解，这意味着在前 6 天，我将比按需定价多支付 0.04 x 24 x 6 = 5.76 美元，在剩下的 24 天，我将节省 24 x 24 x 0.04 x 12% = 2.76 美元。总的来说，我多付了 3 美元。

您可以使用以下公式来确定某项承诺对您是否有意义:

> *hourly _ commitment * 24 * days _ not _ covered _ by _ free _ tier * savings _ rate—hourly _ commitment * 24 * days _ covered _ by _ free _ tier = total _ savings*

# 从小处着手，不断重复

不要把你的支出平均到一个小时的水平，然后把它作为你每小时的承诺。如果你不是每个小时都花同样的钱，那么我建议你从一个较小的储蓄计划开始，当你发现还有改进的空间时，再买一个。

虽然我们根据以前的支出选择每小时的承诺，但我们将**承诺未来的支出**。如果你不确定你会在未来 1 到 3 年内花掉这笔钱，那么在考虑储蓄计划时请谨慎。

假设您每个月有以下支出:

![](img/06b1e744f444b15aa212d10652acdcbe.png)

根据过于简单的公式`hourly_commitment * 24 * days_with_spending_above_commitment * savings_rate - hourly_commitment * 24 * days_with_spending_below_commitment`,我们可以通过承诺每小时 0.04 美元(每天 0.96 美元)的全额预付来节省 5 美元。

一旦我们实施了储蓄计划，我们的剩余支出如下:

![](img/6acab4d1bdec336d01e1572568dedbf0.png)

再增加一个 0.04 美元的储蓄计划，我们现在就能额外节省 0.72 美元。这不是储蓄，而是过度承诺！因此，我们要么不买第二个储蓄计划，要么考虑降低时薪。

储蓄计划可以帮助你根据成本导出找到合适的金额。

请记住，你可以随时购买额外的储蓄计划，但撤销现有的计划是不可能的。从小处着手，迭代！

# 我如何购买储蓄计划？

你了解储蓄计划是如何运作的，以及什么样的小时承诺对你有意义？很好！[现在转到 AWS 控制台](https://console.aws.amazon.com/cost-management/home)中的成本管理。在左侧导航栏中，单击购买储蓄计划。在这里，您可以选择 1 年或 3 年的期限，指定您的小时承诺，并决定您是否要支付全部预付，部分预付或不预付。

将储蓄计划添加到您的购物车中，并仔细查看订单:

*   我选择了正确的期限长度吗？
*   我会用完每小时的承诺还是会多付钱？
*   我对预付款满意吗？

准备好购买后，点击提交订单。

就是这样。现在，您的计算机使用费率将会降低。请务必在接下来的几天内查看利用率报告。

如果您选择全部或部分预付，您将很快在成本浏览器中看到一个大峰值。别担心，那只是储蓄计划的预付款。

# 那么我实际上存了多少钱呢？

几天后，你可以[访问利用率报告](https://console.aws.amazon.com/cost-management/home#/savings-plans/utilization)。这一页将显示你用储蓄计划存了多少钱，以及是否有超额承诺。

![](img/232057a7aff5bff4ddc4cff903e3cc5e.png)

在上面的示例中，我们始终处于 100%的利用率，除了每月初的几天适用空闲层。最终，我们仍然节省了大约 15%的计算费用。

在接下来的几天里，您应该还会注意到，随着预付费计算的自动应用，与 Lambda 和 Fargate 相关的支出在成本浏览器中略有下降。我建议你等几个星期再考虑购买另一个储蓄计划。使用我们上面遵循的相同方法，但请记住，您的支出现在已经降低，您应该在分析您的下一个储蓄计划时，只考虑自最近购买储蓄计划之日起的支出。

看看官方文件是怎么说监控储蓄计划的。

# 资源

*   [储蓄计划发布公告](https://aws.amazon.com/blogs/aws/new-savings-plans-for-aws-compute-services/)
*   [上周在 AWS 谈储蓄计划](https://www.lastweekinaws.com/blog/aws-begins-sunsetting-ris-replaces-them-with-something-much-much-better/)
*   [储蓄规划师](https://savings.bahr.dev/)
*   [AWS 的储蓄计划指南](https://docs.aws.amazon.com/savingsplans/latest/userguide)
*   [监控储蓄计划](https://docs.aws.amazon.com/savingsplans/latest/userguide/sp-monitoring.html)
*   [DynamoDB 预留容量](https://aws.amazon.com/blogs/aws/dynamodb-price-reduction-and-new-reserved-capacity-model/)

你喜欢这篇文章吗？让我在推特上知道，让我注册邮件列表，让新文章直接进入你的收件箱！