# 使用自定义 CSS 变量

> 原文：<https://blog.devgenius.io/using-custom-css-variables-aa688728c30a?source=collection_archive---------27----------------------->

![](img/45d917d78490773d18df5d745175661a.png)

杰斯·贝利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果您使用过任何样式库或框架，如 Bootstrap 或 Semantic UI，您将会使用它们的许多内置快捷方式来快速将一致的样式添加到您的项目中。

这些框架已经完成了大部分繁重的工作，并允许您快速启动并运行——但是当您被迫使用某些您并不真正想要的默认样式时，您可能还会觉得有点烦人。例如，你可能不想使用他们的配色方案，或者他们特别选择的按钮样式。如果您是这些框架的高级用户，您将知道如何深入研究文件系统并更改默认设置，但即使这样，如果工作的话，也会感觉工作量很大。如果我们可以创建自己的默认值并使用它们，而不必导入我们不需要的整个框架，这不是更容易吗？

好吧，你猜怎么着，我们可以！CSS 变量出现已经有一段时间了，但最初只有在使用 SASS 或更低版本的预处理程序时才可用。自从 CSS 3 以来，我们不需要任何框架，它们已经存在了足够长的时间，现在已经有了很好的跨浏览器支持——此外还有额外的好处，比如能够用 JavaScript 设置和读取它们(这是用预处理器变量做不到的)。

语法有点难看，但是一旦你知道了就足够简单了，通过设置特定的变量值并在样式表的其余部分引用这些值，你可以节省大量的时间。颜色是一个很好的例子，说明它们非常方便——如果你想改变你的配色方案，你可以只改变你的变量的值，而不必经历和改变你在样式中设置的每一个颜色。

让我们看一个例子:

## 1)设置变量

使用 **:root** 选择器设置变量值，在您想要设置的变量名前使用两个破折号定义:

```
:root{
  --theme-primary: #FFFFFF;
  --theme-secondary: #1F1F24;
  --text-primary: #55555E;
  --text-secondary: #9696A0; --box-padding-sm: 1rem 2rem;
  --box-padding-lg: 2rem 4rem;
}
```

## 2)使用它们

通过使用 **var()** 语法调用预定义的变量来设置 css 属性:

```
h1 {
  color: var(--text-primary);
}p {
  color: var(--text-secondary);.small-box {
  padding: var(--box-padding-sm);
  background-color: var(--theme-primary);
}.large-box {
  padding: var(--box-padding-lg);
  background-color: var(--theme-secondary);
}
```

就这么简单！基本上，您可以使用自己的自定义 CSS 变量构建自己的样式库，而无需向项目中添加任何大型框架或库。