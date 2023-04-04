# 软件工程师的例外

> 原文：<https://blog.devgenius.io/exceptions-from-a-software-engineer-4a9e8e5611be?source=collection_archive---------15----------------------->

让我们使用以下几点来定义 SDE/SE，这是我目前为止的理解和学习。

> GDTRS
> 
> “软件工程师是这样一种人，他能够‘收集需求’、‘开发’、‘测试’、‘发布’和‘支持’软件产品。
> ——普拉纳渊

父亲要求和设计

![](img/eff152e887b485318045ac5bb93168cf.png)

由 [Med Badr Chemmaoui](https://unsplash.com/@medbadrc?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

运用常识，理解一般问题，然后深入了解实际需求。创建一份设计文件(高级设计和法律设计)并获得高级 SDE 和同事的批准。分解成任务，给出 ETA。

发展

![](img/120756d19505a05cf5df949076e8d2d0.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

选择一种技术来交付成果。遵循坚实的原则，编写可扩展和可维护的代码。为所有公共方法编写适当的“单元测试”。确保保持至少 75%的代码覆盖率。
*在代码不是 TDD 之后编写单元测试

测试

![](img/38c9ee34acddc864ad75dd2588f11373.png)

照片由[克里斯蒂娜@ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

既然您已经在编写代码时介绍了 UT(单元测试),现在是时候编写集成测试了。

在这里，我们将检查各个组件在相互交互时的实际行为。

R elease

![](img/e7550ed0433a93cd0b144296292904f7.png)

[比尔·杰伦](https://unsplash.com/@billjelen?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

您不希望将新出炉的代码部署到生产环境中。最好一次分批部署 25%的代码。观察到任何正常行为的尖峰？回复！

支持:—每个 SDE 都需要待命(支持)一到两周(取决于团队)。

# 缩略语:

![](img/403e3d54e4e4897799c991da3732f37b.png)

劳伦·索德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

1.  SDE:软件开发工程师。
2.  SE:软件工程师。
3.  单元测试。
4.  LLD:低层次设计。
5.  TDD:测试驱动开发。
6.  预计到达时间
7.  固体:

*   单一责任
*   打开–关闭
*   利斯科夫替代
*   界面分离
*   依赖性倒置