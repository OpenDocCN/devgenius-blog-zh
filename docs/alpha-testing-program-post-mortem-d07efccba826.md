# 阿尔法测试程序:事后分析

> 原文：<https://blog.devgenius.io/alpha-testing-program-post-mortem-d07efccba826?source=collection_archive---------12----------------------->

我们正在开发一个新的 web 框架，它与 React & Node.js 集成在一起，同时也是一种语言。正如你可能想象的那样，让人们使用一项新技术并不容易，尤其是当它还处于 Alpha 阶段的时候。另一方面，没有用户和他们的反馈，就不可能知道要构建什么。

这就是我们为 Wasp 运行 Alpha 测试程序的原因——下面是我们学到的东西，以及一路上做得好和做得不好的地方。

![](img/e746c54607a90054dfd46e06949bebb4.png)

# “我当然知道黄蜂！我只是还没来得及尝试一下。”[](https://wasp-lang.dev/blog/2022/11/16/alpha-testing-program-post-mortem#of-course-i-know-about-wasp-i-just-havent-come-around-to-trying-it-out-yet)

虽然我们在 GitHub 上点击了 HN [几个](https://news.ycombinator.com/item?id=26091956) [次](https://news.ycombinator.com/item?id=32098144)的首页，并且即将达到 2000 颗星，但是一个人主演一个回购和实际坐下来用它建立一些东西之间仍然有很大的区别。

通过与人们交谈，我们意识到他们中的许多人听说过 Wasp，认为这是一个不错的想法，但没有尝试过。这些是主要原因:

*   不得不花 30 分钟来浏览我们的构建待办事项应用教程— *“我现在很忙，但下周我会做的。”*
*   构建一个基本的 todo 应用程序并不令人兴奋
*   不知道还能造什么
*   *“该产品仍处于 alpha 版本，因此我将为其添加书签以备后用”*

这些都是显而易见的，可以理解的原因。我必须承认，在尝试新的/未经证实的东西时，我也是一样的——也许更糟。这并不是优先考虑的事情，如果没有帮助我克服所有这些反对意见的推动力，我通常没有动力去完成它。

意识到这一切后，我们明白我们需要给人们一个现在试用 Wasp 的理由，因为那时我们需要反馈，而不是下周。

# 欢迎来到 Wasp Alpha 测试项目！

![](img/27cdba61f1f7737193a821d7a5ebc1a8.png)

我们在这里[玩得有点太开心了](https://wasp-lang.notion.site/CLOSED-Welcome-to-Wasp-Alpha-Testing-program-f3a8a350802341abac87fb7831bb1e60)，但是传送门的粉丝会理解的。

我们很快地为 alpha 测试者在 idea 中创建了一个招生页面(你可以在这里看到它[和](https://wasp-lang.notion.site/Wasp-Alpha-Testing-Program-Admissions-dca25649d63849cb8dfc55881e4f6f82)),并开始分享它。为了克服我们上面提到的障碍，我们对该计划进行了时间限制(*“现在正在进行，一旦开始*”)，并承诺向每个完成教程并填写反馈表的人提供一件 t 恤。

![](img/585c7ee7cc965d952f5189543f4c51cb.png)

来自录取页面的 CTA

很快，第一批应用程序开始陆续出现！对于每个新申请人，我们会跟进如何成功通过 Alpha 测试项目的[说明](https://www.notion.so/CLOSED-Welcome-to-Wasp-Alpha-Testing-program-f3a8a350802341abac87fb7831bb1e60):

*   填写介绍表格(多年经验、首选堆栈等)
*   浏览我们的“构建待办事项应用”教程
*   填写反馈表——哪些是好的，哪些是不好的，等等。

![](img/7e21835e539443ee506bf96810f4ecc8.png)

人们真的很尊重这个最后期限，如果他们不能按时完成，他们会礼貌地要求延长期限。

但是，不久之后，我们在推特上收到了以下消息:

![](img/ecb5b56f2f52b256978147c7fb33e128.png)

我们真的很害怕，我们会得到一吨的人投入最少的努力，而尝试 Wasp 只是为了获得免费的赠品，让我们两手空空，什么也没学到！另一方面，我们没有太多的选择，因为我们没有预先定义反馈的“最低要求质量”。

幸运的是，最终这不是问题，甚至相反——我们确实收到了大量申请，但只有一部分人完成了项目，而那些完成的人留下了非常高质量的反馈！

# 进展如何——测试概况和反馈

## 测试仪简介[](https://wasp-lang.dev/blog/2022/11/16/alpha-testing-program-post-mortem#tester-profile)

我们收到了 210 份申请，其中 53 份完成了项目，完成率为 25%。

我们还调查了申请人的首选堆栈、多年编程经验等:

![](img/79eb3336fc65f5a15302df7294b18eb7.png)

是的，我们喜欢双关语。

## [反馈](https://wasp-lang.dev/blog/2022/11/16/alpha-testing-program-post-mortem#the-feedback)

反馈表评估了测试人员对 Wasp 的总体体验。我们问了他们使用 Wasp 的最佳和最差之处，以及他们希望看到的下一个特性。

![](img/27b324ba81d49fb3d1ce603780480b9b.png)

**不良零件**

我们的测试人员最缺少的是成熟的 IDE 和类型脚本支持。这两个都是测试版，但当时只支持 JS。另外，Windows 有一些安装问题(还不完全支持——最好通过 WSL 使用它)。

![](img/2baafa58613620f4b708578ead170cea.png)

我们已经意识到 TypeScript 支持是一个重要的特性，但并没有确切的感觉——反馈真的很有帮助，帮助我们优先考虑我们的 Beta backlog。

**好零件**

测试人员最喜欢的部分是包含电池的体验，尤其是[认证模型](https://wasp-lang.dev/docs/tutorials/todo-app/06-auth)。

![](img/3407f0babe99923a6b36b713efe66466.png)

# 验:什么不顺利[](https://wasp-lang.dev/blog/2022/11/16/alpha-testing-program-post-mortem#post-mortem-what-didnt-go-well)

## 反馈质量没有阈值

![](img/0c9c59e8525b0980dd9b9c43d527293a.png)

我们没有对反馈表设置任何限制，例如反馈的最短长度。这导致大约 15%-20%的答案是单个单词，如上所述。我不确定是否有一个有效的方法来避免这种情况，或者只是一个统计数字。

## 使用自由文本格式收集地址[](https://wasp-lang.dev/blog/2022/11/16/alpha-testing-program-post-mortem#using-free-text-form-for-collecting-addresses)

我们以前从未想过验证地址可能是运输赃物的一个重要部分，但事实证明确实如此。似乎有很多方法来指定地址，其中一些与我们邮局的预期不同，导致许多货物被退回。

一个理想的解决方案是在调查中使用一个专门的“地址”字段来自动验证它，但结果是 Typeform(我们使用的)还没有实现这个功能，尽管[对它的要求很高](https://community.typeform.com/suggestions-feedback-34/address-field-question-type-2950)。

![](img/7c79a853ed111cc3735b31f84e79406b.png)![](img/c6c64bf719d3f13e4ace9c057b476411.png)

# 阿尔法测试计划的不明显的好处

进展顺利的是，我们得到了许多高质量的反馈，这些反馈引导并强化了我们即将到来的测试版的计划。

另一个很大的好处是，我们终于解决了*“看起来很酷，但我以后可能会尝试一下”*的问题。总的来说，我们的使用量在项目期间上升很快，但即使在项目结束后，基线也显著上升。这是我们没有预见到的二阶效应。

我们的理解是，一旦人们最终尝试了它，他们中的一部分人会直接感受到它的价值，并决定在其他项目中继续使用它。

![](img/3162040e620d8920b3d9a091de8cf8c9.png)

# 总结和展望:Beta[](https://wasp-lang.dev/blog/2022/11/16/alpha-testing-program-post-mortem#summary--going-forward-beta)

我们 Alpha 测试项目的总体结论是，这是一项有价值的工作，为我们带来了有价值的反馈，并对整体使用产生了积极的影响。展望未来，我们将努力专注于确保更高质量的反馈，并优先考虑一对一的沟通，以确保我们完全了解 Wasp 用户的困扰以及我们可以改进的地方。以较小的批量进行测试也可能是有帮助的，这样我们就不会被反馈淹没，可以专注于单个测试人员——这是我们可能在 Beta 中尝试的事情。

如前所述，下一站是 Beta！它将于 11 月 27 日发布— [在此注册](https://wasp-lang.dev/#signup)获取通知。