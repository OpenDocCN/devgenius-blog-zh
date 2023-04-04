# 使用方法链接。

> 原文：<https://blog.devgenius.io/use-method-chaining-9abd458a100d?source=collection_archive---------14----------------------->

## 你如何实现 BDD 风格的功能测试？

## 使用 BDD 的自动化 UAT 的替代方案。

![](img/fb0c230a4cb81ac50674e6699c793b05.png)

来自[佩克斯](https://www.pexels.com/photo/body-of-water-near-building-498705/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[玛格丽特](https://www.pexels.com/@margerretta?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

编写 BDD 风格的自动化功能测试是有利且容易的。我知道是因为我已经用黄瓜实现了它。
如果执行正确，Cucumber 会为您提供业务问题(特征文件)和技术解决方案(底层测试验证的产品)的同步视图。这可以帮助工程和业务团队很好地沟通。
Cucumber 还可以帮助工程师从“我如何测试这段代码？”的心态中转移出来到“用户将如何与这个系统互动，我们期望由此产生什么样的行为？”

有时，某个项目可能不需要黄瓜或等效的协作工具。比如，商业利益相关者不把它作为沟通渠道。在这种情况下，拥有这样的工具可能会成为一种开销。

您可以选择将您的方法链接在一起，以清楚地描述您正在验证的场景的意图。干净编码的原则——代码必须易于阅读、理解和维护——也应该用于自动化功能测试。

最近，我与 Cypress 合作实现了自动化功能，我按照以下步骤实现了这一点:

*   给方法起一个不言自明的名字。
*   导出单独命名的函数。
*   将其作为命名文件导入。

这 3 点，如果正确实现，可以帮助实现 BDD 风格的测试。

看看这个要点:

您可以在中看到命名的方法

```
upload.ts
wait.ts
details.ts
```

这些文件的所有方法在规范文件中都被命名为 imports。
Spec 文件现在有了一个自我解释的测试用例/规格:

*   测试登录并等待主页加载。
*   在主页上，它加载一个详细信息页面(对于一个项目)并等待它。
*   然后上传两个文件，等待两个文件都上传成功。等待超时为 120 秒。
    (该方法支持一次上传多个文件。)
*   然后执行验证-这两个文件是否已经成功上传。

就是这样，就是这么简单！我喜欢使用这种策略。也让我知道你的经历吧！