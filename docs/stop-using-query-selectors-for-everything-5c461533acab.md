# 停止对所有事情使用查询选择器

> 原文：<https://blog.devgenius.io/stop-using-query-selectors-for-everything-5c461533acab?source=collection_archive---------2----------------------->

![](img/d2a4d01ffd237889b2eaa3a8d6cc3f8f.png)

最近，我的一个学徒就一个学校项目向我寻求帮助和建议。

快速浏览了一下它的代码后，我立刻被大量使用`querySelector`和`querySelectorAll`方法来通过 id 或类**检索元素而震惊了。**

当我问他为什么不用经典的方法`getElementById`或`getElementsByClassName`时，他回答说是因为老师给的建议。

他的老师建议他使用`querySelector`和`querySelectorAll`方法，借口是结果与经典方法相同，并且这些所谓的经典方法将不再用于`querySelector`和`querySelectorAll`方法。

**那么，用某种特定的东西去换取某种更全球化的东西，是一件好事吗？这些方法相等吗？**

*我们一起来看，这几种方法在三点上是大相径庭的:* ***可读性*** *，* ***结果*** *和* ***性能*** *。*

# 可读性原则

想象一下，必须通过元素的标识符或类来检索元素，以便对其执行更改。

你觉得这个:`const el = **querySelector**("#post-3773");`，比那个:`const el = **getElementById**("post-3773");`更有意义吗？

还是你觉得这个:`const els = **querySelectorAll**(".new-article.edited");`，比那个:
`const els = **getElementsByClassName**("new-article edited");`更有意义？

> “任何傻瓜都能写出计算机能理解的代码。优秀的程序员会写出人类能理解的代码。”
> 
> 马丁·福勒

正如我在以前的文章中多次提到的，当用 JavaScript 编码时，如果我们不优先考虑人工阅读代码而不是机器执行代码，我们很快就会得到难以阅读和调试的代码。

老实说，在通过 id 或类名检索元素时，通过 id/类名获取元素的**比简单的**查询选择器更有意义。

# 不同的结果

当你使用`getElementById`或`querySelector`时，结果是一样的:它是一个[元素](https://developer.mozilla.org/en-US/docs/Web/API/Element)。

> 所以，不用担心。

但是当你使用`getElementsByClassName`和`querySelectorAll`时，结果就完全不同了。

`getElementsByClassName`方法将返回一个[动态 HTML 集合](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)，而`querySelectorAll`方法将返回一个[静态(非动态)节点列表](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)。

> 这两个系列有什么区别？

尽管`NodeList`对象与`HTMLCollection`对象几乎相同，但它包含的内容却有很大不同。

虽然`HTMLCollection`对象将只包含元素节点(基于*标签*，但是`NodeList`对象将包含所有类型的节点(*元素、注释、文本等*)。

此外，了解所谓的**活动**节点集合和所谓的**静态**节点集合之间的区别非常重要。

当一个集合是 **live** 时，意味着 DOM 的任何变化都会**影响集合的内容**(*更新*)。另一方面，当集合被说成是静态的时，这意味着 DOM 中的任何变化都不会影响集合的内容。

在选择迭代每个元素时，最好记住这一区别。

# 演出

同样重要的是要知道`querySelector`和`querySelectorAll`方法比`getElementById`和`getElementsByClassName`等更经典的方法效率低得多。

## getElementById VS querySelector(最快的方法:效率超过 60%的 getElementById)

[](https://www.measurethat.net/Benchmarks/Show/16208/2/getelementbyid-vs-queryselector-simple-comparison) [## 基准测试:getElementById VS querySelector(简单比较)—MeasureThat.net

### 用户代理:Mozilla/5.0(Macintosh；英特尔 Mac OS X 10 _ 15 _ 7)apple WebKit/605 . 1 . 15(KHTML，像壁虎一样)版本/15.1…

www.measurethat.net](https://www.measurethat.net/Benchmarks/Show/16208/2/getelementbyid-vs-queryselector-simple-comparison) 

## getElementsByClassName VS query selectorall(最快的方法:getElementsByClassName，效率超过 305%)

[](https://www.measurethat.net/Benchmarks/Show/16209/0/getelementsbyclassname-vs-queryselectorall-simple-compa) [## 基准测试:getElementsByClassName VS query selectorall(简单比较)—MeasureThat.net

### 用户代理:Mozilla/5.0(Macintosh；英特尔 Mac OS X 10 _ 15 _ 7)apple WebKit/605 . 1 . 15(KHTML，像壁虎一样)版本/15.1…

www.measurethat.net](https://www.measurethat.net/Benchmarks/Show/16209/0/getelementsbyclassname-vs-queryselectorall-simple-compa) 

当需要通过标识符或类名检索元素时，与选择器方法相比，所谓的经典方法的效率是毋庸置疑的。

# 更进一步

如果你想更进一步，还有其他方法来检索节点(元素或其他)。

您可以使用`**getElementsByTagName()**`方法通过标签检索一个 *live* `HTMLCollection`。查看此[链接](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByTagName)，了解更多信息。

您可以使用`**getElementsByName()**`方法通过 name 属性检索一个 *live* `NodeList`。查看此[链接](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByName)，了解更多信息。

**感谢您的阅读🙏。**

随意**分享**和/或[**关注本账号**](https://medium.com/@techbrant) **。那会很有帮助😁**

*作者:* ***benlmsc***

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io/)*。*