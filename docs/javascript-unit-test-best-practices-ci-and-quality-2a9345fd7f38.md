# JavaScript 单元测试最佳实践— CI 和质量

> 原文：<https://blog.devgenius.io/javascript-unit-test-best-practices-ci-and-quality-2a9345fd7f38?source=collection_archive---------6----------------------->

![](img/f94077613d120cd2193635e24bd9853b.png)

凯蒂·伯诺斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 作为实时协作文档进行测试

测试告诉我们很多关于我们系统的事情。

他们有大量的描述和检查，告诉我们它能做什么。

还有像故事书和 UI 组件这样的东西告诉我们它们是做什么的。

我们可以用它们查看每个组件。

# 使用自动化工具检测视觉问题

我们可以使用各种工具来检测视觉问题。

它们不会在单元或集成测试中被捕获。

这些工具保存我们应用程序的截图，以便我们可以看到我们应用程序中的任何缺陷。

也有像 [Applitools](https://applitools.com/) 、 [Percy.io](https://percy.io/) 这样的工具，让我们管理截图，并利用他们自己的技术自动检测视觉噪声。

# 获得足够的自信报道

我们可以获得足够的测试覆盖率，使我们对测试充满信心。

大约 80%应该覆盖最重要的部分。

我们可以使用 CI 工具来检查我们是否满足测试覆盖率的阈值。

大多数测试框架可以毫不费力地获得测试覆盖率。

# 检查覆盖率报告

我们应该检查我们的测试覆盖报告，以检查我们的应用程序的哪些部分还没有被测试覆盖。

他们可以突出显示我们代码中要检查的项目。

仅仅拥有测试覆盖率并不能告诉我们应用程序的哪些部分被测试覆盖。

# 测量逻辑覆盖率

我们可以使用突变测试来判断我们的应用程序的哪一部分是真正测试过的，而不是仅仅被访问过。

要做到这一点，我们可以有意识地改变这些值来检查结果。

我们可能会发现我们可能还没有发现的应该失败的案例。

# 防止测试代码问题

为了检查测试代码的问题，我们可以使用测试棉条来检查代码。

有 ESLint 的插件，如 [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha) 或 [eslint-plugin-jest](https://github.com/jest-community/eslint-plugin-jest) 将为我们做这些检查。

例如，当测试没有断言时，它会警告我们。

# 丰富 Linters 并取消有林挺问题的构建

如果我们的代码有林挺问题，那么我们应该阻止它们的生成，因为我们希望林挺问题得到解决。

ESLint 有很多插件，比如 Airbnb 和 Standard.js 插件。

他们可以发现代码问题，并确保格式是每个人都同意的。

还有 [eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security?activeTab=readme) 来检查我们应用中的安全问题。

这将阻止攻击者试图攻击我们的应用程序。

# 缩短反馈回路

如果我们在开发人员的机器上本地运行 CI，我们可以更快地获得反馈。

他们让我们在本地运行我们的管道，这样我们可以获得有用的测试见解。

因此，我们可以编写代码，获得反馈，然后根据需要进行更改。

![](img/346399e1740fb1f9953fada6c6e32ec6.png)

由[赫伯特·戈特施](https://unsplash.com/@hg_photo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用 CI 工具增强我们的测试工作流程，并检查诸如视觉缺陷之类的东西。