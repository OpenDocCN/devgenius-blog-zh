# Javascripts forEach 循环的 3 个糟糕用例

> 原文：<https://blog.devgenius.io/3-bad-use-cases-of-javascripts-foreach-loop-add3600a8895?source=collection_archive---------2----------------------->

![](img/5c778a5a9af96be527b4f0ad906f69f9.png)

保罗·埃施-洛朗在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

forEach 循环是大多数 JS 开发人员工具箱的一部分。但是一定要这样吗？我会说不。我还没有发现 forEach 是唯一合理的解决方案的情况，我发现自己越来越少使用它。一些开发人员喜欢 forEach 循环的可读性，这一点很有道理，因为这是一个主观问题。我个人认为“for of”循环更具可读性。

```
*elements*.forEach(function(*el*) {
 // *code*
});for (*el* of *elements*) {
 // *code*
}
```

让我们来看看 forEach 循环不适合的情况。

## 如果你想短路或者脱离回路

短路意味着终止或跳过循环的迭代。在 forEach 循环中，终止循环的唯一方法是引发异常。由于这一限制，您应该期望运行时与循环数组的大小线性匹配。例如，搜索元素因此不是 forEach 循环的好用例。相反，使用数组方法“find”、“findIndex”或可以终止的 for 循环。

## 性能至关重要时

在 forEach 循环中，我们在每次迭代中都调用回调函数。与 for 循环相比，这个额外的作用域会导致速度变慢。for 循环使用一个初始化语句和一个在每次迭代中计算的条件。这意味着与以前相比成本更低。

很难说确切的数字，因为每个平台都有所不同，但我看到了从 2 倍到 20 倍的性能差异。此外，您还可以对 for 和 forEach 进行性能改进。一个例子是“反向 for 循环”,其中条件不需要包括数组长度检查。

## 当你想变换一个数组时

forEach 循环在副本上工作，所以即使您试图改变元素，数组也保持不变。考虑到流行的不变性编码实践，这并不是最好的主意。

对于转换，您可以使用 map 和 filter，它们都返回一个新数组。forEach 循环总是返回 undefined。使用 forEach 来达到同样的结果更加笨拙。

当你想通过循环得到一个累加值时，大声喊出来减少。

希望你在阅读这篇文章时学到了一些东西，并祝你在未来的编码会议中愉快🚀
如果你想说代码，在推特上找我:@barelydaniel