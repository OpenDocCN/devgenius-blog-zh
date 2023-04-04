# 🏎让您的 Jest 测试速度提高 10 倍

> 原文：<https://blog.devgenius.io/make-your-jest-tests-upto-10x-faster-d751b3428ded?source=collection_archive---------0----------------------->

![](img/b70ad191b3644c330c72dd25566db281.png)

中等规模的 react 项目的单元测试需要等待 30 分钟，这令人沮丧。为了关注速度，我们决定采取一些措施来提高速度。

像我们所做的一样，我们开始寻找不同的方法来提高 jest 在 google 上的性能。这篇文章列出了我们所有的观察和结果。它可能适合您的配置，也可能不适合，但是我建议通读这篇文章并尝试每种方法，看看是否有什么不同。

所有优化的黄金法则是衡量。是的，衡量一下目前有什么。这包括我们需要跟踪的所有度量，并在此列出。

*   是时候运行整个套件了。
*   十大最慢的套房及其时间。
*   消耗的总内存。

# # 1 在测试中进行网络调用

因此，当我开始寻找测试缓慢的原因时，我从团队得到的一个观察结果是，它很古怪，它可能在 react 组件内部进行 API 调用以进行测试。这激起了我的好奇心，这是真的。测试用例进行 API 调用，并等待它们的响应，这是测试缓慢的一个原因。

最好的单元测试是每次运行测试时都给出相同结果的测试。如果他们依赖于网络，那么这是根本不可能实现的。

下一步是寻找如何避免这种情况，这就是我们遇到 jest-offline 的原因。我们开始实现这一点，但是下一个问题是许多测试用例正在进行 API 调用，所以我们必须识别所有这些并手动模拟调用，这需要一些时间，但是值得努力。

现在强制执行这个规则，如果我们不模仿任何调用，它就直接测试失败。

# #2 可能不仅仅是网络通话。

当发现网络调用可能是一个问题时，接下来的事情就是我们在寻找类似的问题。幸运的是，我们又遇到了一个问题，这改进了我们的测试运行时间。

单元测试应该独立而精确地运行，但是对我们来说，我们用 Redux Provider 包装测试用例，它是用来自`redux-persist`的 persistReducer 配置的。redux persist 的主要目的是跨不同的选项卡&会话持久化数据。然而，对于 UT 来说，这正好相反，所以去掉这个就从所有的测试中去掉了一个 IO 操作，节省了大量的时间。

**要注意的事情**

*   清理不必要的 IO 操作，如本地存储等。
*   测试用例中的非触发定时器。
*   组件的不必要包装
*   哨兵包装器→UT 不需要
*   分析包装器→UT 不需要。

# #3 使用 MaxWorkers=50%来获得最佳性能。

受下面提到的文章的启发，你可以在那里阅读更多内容。

[https://dev . to/vantanev/make-your-jest-tests-up-to-20-fast-by-changing-a-single-setting-i36](https://dev.to/vantanev/make-your-jest-tests-up-to-20-faster-by-changing-a-single-setting-i36)

# #4 切换到纱线+节点 V16 + Jest 28

Jest 有很多性能改进。因此，我们升级到 28。NodeJS 也是如此。这一改变大大加快了我们的性能。

# #4 修复最慢的测试案例

每个优化的第一步都是识别错误，因此识别最慢的测试也同样重要。也许最慢的十个测试可能会占用超过 80%的执行时间。谁知道呢！

使用`jest-slow-test-reporter`来识别最慢的测试，然后开始改善它们的性能。确保你的大多数测试在 100 毫秒左右或以下。最慢的可以达到 300 毫秒。超过这一点就是危险信号。

# #5 切换到 swc/jest 以获得难以置信的性能提升。

随着`esbuild`的出现，前端世界对开发环境的速度要求越来越高。 [swc](https://github.com/swc-project/swc) 是用 rust 编写的超快速编译器。为什么不将同样的方法用于单元测试呢？

切换到`@swc/jest`，你可以立即看到性能的提高。更多信息，你可以参考[https://miyauchi.dev/posts/speeding-up-jest/](https://miyauchi.dev/posts/speeding-up-jest/)

# #6 内存泄漏

另一个导致测试缓慢和不稳定的罪魁祸首是内存泄漏，这一点经常被忽略，所以要采取措施来识别导致内存泄漏的测试。

有关识别测试内存泄漏的更多信息，您可以参考:[https://chanind . github . io/JavaScript/2019/10/12/jest-tests-memory-leak . html](https://chanind.github.io/javascript/2019/10/12/jest-tests-memory-leak.html)

# #6 React 测试库

寻找性能猪的另一个地方是你如何编写测试用例。使用不正确的选择器是测试缓慢的主要原因之一。很少有问题是可以解决的。

*   到处使用`.type`。只在需要的地方使用它，否则退回到`.paste`，因为它快得多。
*   你可以参考来自[https://kentcdodds . com/blog/common-errors-with-react-testing-library](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)的其他常见陷阱

确保使用正确的选择器和功能可以提高单元测试的质量和性能。

# #7 附加

下面列出了一些可以用来提高性能的额外技巧。

**findreatedtests**

这立即减少了运行测试所需的时间，因为使用这个标志将确保您只运行与您所做的代码更改相关的测试。因此，不是运行 1000 个以上的测试，您现在可能只为您更改的 20 个文件运行 20 个测试用例。

我们还可以使用`--changedSince`，它运行与提供分支或提交散列之后的更改相关的测试。

```
jest --changedSince <commit-id>
```

**JSDOM 变更**

将测试环境从 jsdom 更改为类似 LinkeDOM 的环境会将性能提高至少 2 倍。但是我不建议这样做，因为我还没有在专业场合使用过它。我建议您尝试一下，看看它在您的配置下如何工作。

# 影响

*   开发人员只需等待他们以前等待 PR 上的构建检查的一小部分时间，这提高了开发人员的生产力和时间管理。
*   CI 上的负载现在将大幅减少，从而节省更多成本。

希望这些步骤能帮助你大幅减少运行单元测试的时间。如果你能用这些建议做出巨大的改变，请留下你的成就评论。

本文最初发表于[https://kirananto.com/make-your-jest-test-upto-10x-faster/](https://kirananto.com/make-your-jest-test-upto-10x-faster/)