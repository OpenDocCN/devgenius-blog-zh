# 理解声明式编程与命令式编程

> 原文：<https://blog.devgenius.io/understanding-declarative-vs-imperative-programming-eb982bac9d00?source=collection_archive---------5----------------------->

你可能听说过这些术语，尤其是涉及到 React 这样的东西时。根据文档，react*是声明性的*，但是它真正的意思是什么呢？让我们来看看这两种范式的区别。

![](img/d5f1a464e496c8cd03205b4a1cf5d6aa.png)

照片由[埃米尔·佩龙](https://unsplash.com/@emilep?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 什么是命令式编程？

根据我们的好朋友维基百科，我们有如下定义:

> 在计算机科学中，命令式编程是一种编程范式，它使用改变程序状态的语句。

本质上，这意味着我们正在发布指令，描述我们希望计算机如何执行它的任务。命令式编程利用相对低级的函数和 API 调用来绝对控制如何做某事的实现细节。

以下 JavaScript 是一些命令式代码的示例:

```
const domNode = document.getElementById('foo');
domNode.style.color = 'red';
```

在这里，我们告诉浏览器如何获取某个元素，以及如何将其颜色更改为红色。什么，怎么做。

# 声明式编程呢？

相比之下，声明式编程是一个更高级的概念。我们告诉系统*做什么*做什么，但不严格地说怎么做。实现细节从我们这里抽象出来了，但是它们仍然存在。不要犯错误；你不能告诉一台计算机做什么，而不告诉它如何做，在链条的某个地方。

声明式编程的一个简单示例是下面的 HTML:

```
<div>
  <h1 id="foo">My Heading</h1>
</div>
```

我们告诉浏览器我们想要它做什么，但是它如何解释 HTML 并将其呈现到我们的屏幕上的实际实现是抽象的，不在我们的控制之内。

# React 是声明性的

所以当人们说反应是陈述性的，这对我们意味着什么？想想也有道理。React 与 HTML 有很多相似之处，以及过去做事的方式。我们可以构建完整、复杂的应用程序，大多数情况下无需直接操作 DOM。当然，仍然有一些边缘情况，直接接触 DOM 并使用 refs 是最好的方法，但是这种情况很少。

大多数情况下，在为 React 编写代码时，您将通过嵌套组件来组合应用程序。其中一些组件将包含业务逻辑，但即使这样，您也很少会编写命令性代码。

考虑下面的 React 代码:

```
const foo = () => {
  return <h1 style={{ color: 'red' }}>My Heading</h1>;
}
```

在这里，我们告诉 react 生成这个组件，并给它红色的文本，但是实际的命令调用并不关我们的事。我们告诉 React 要做什么，它会计算出*如何*。

# 声明性编码的其他示例

特定领域语言(DSL)本质上通常是声明性的。这是因为 DSL 的目的是提供一个接口，其中可用的指令在域的上下文中是有意义的。考虑 SQL，一种用于与数据库交互和查询数据库的 DSL。我们可以有这样的 SQL 语句:

```
SELECT * FROM `users` WHERE `users.date_of_birth` IS NULL;
```

在这里，我们告诉数据库“让我得到所有具有空“出生日期”字段的用户”，因为 DSL 是如此精细地调整，它读起来几乎像普通英语(尽管对于一些更高级的查询不能这样说)。

这里重要的是，我们告诉数据库*什么*要做，在后端实现 DSL 的工程师们已经完成了找出*如何做*的艰苦工作。

根据不同的情况，总是需要声明性的*和*命令性的编码。不要错误地认为你需要选择一个并坚持下去。理解这两个范例之间的差异以及它们是如何结合在一起的，是了解何时何地使用它们的第一步。