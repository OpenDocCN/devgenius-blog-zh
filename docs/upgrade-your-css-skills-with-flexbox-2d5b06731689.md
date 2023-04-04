# 使用 Flexbox 升级您的 CSS 技能

> 原文：<https://blog.devgenius.io/upgrade-your-css-skills-with-flexbox-2d5b06731689?source=collection_archive---------5----------------------->

![](img/1dc1b49de3feab2e69f7ed844c740f25.png)

照片由 [Esther Jiao](https://unsplash.com/@estherrj?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

作为一名训练营的毕业生，快速掌握软件开发可能是一项艰巨的任务。学习每一种新工具并开发越来越复杂的应用程序是一个挑战。当你学习不同的工具来完成相同的任务时，保持这些技能是一个完全不同的挑战。

对我来说，这个挑战在 CSS 中最为普遍。在学习编码的时候，我认为很容易感觉 CSS 不是最重要的语言。公平地说，如果您一开始就不知道如何开发应用程序，那么应用程序的样式并不重要。在我的训练营中，我并没有为我的第一个项目考虑太多 CSS，因为它只是一个命令行应用程序。后来，我设法学会了一些基本的东西，并拿它们来玩。然而，随着我接手越来越多时间紧迫的项目，我总是优先考虑最大化特性，而不是开发一个愉快的 CSS 体验。这导致了在开发的最后几个小时利用 bootstrap 将导航条、卡片和其他基本的样式组件组合在一起。虽然 bootstrap 有其优势，但我觉得它让我在继续发展其他技能时有了 CSS 拐杖。

我对 bootstrap 的依赖达到了让我害怕在自己的 CSS 中尝试匹配相同样式的地步。令人欣慰的是，在我完成训练营之后，我花时间重温了 CSS，并把这一技能提升到与其他技能相同的水平。那时我发现了 Flexbox。Flexbox 是一种 CSS3 布局模式，使您能够以一种简洁高效的方式轻松排列容器中的项目。它释放了 bootstrap 的许多优势，并将控制权交给了您。

Flexbox 取代了对 CSS 浮动的需求，并简化了子定位、容器响应和元素排序。这意味着 flexbox 是开发移动友好的应用程序的一个很好的工具，同时把大量的工作从你的手中拿走。尽管 Flexbox 不是捷径，但它是真正的 CSS。Flexbox 侧重于改变项目的高度和宽度，以最大限度地适应其容器的可用空间。它是与方向无关的，所以与垂直偏向的块模型和水平偏向的内联模型不同，flexbox 对两者都适用。Flexbox 针对小规模布局进行了优化，CSS Grid 将成为大规模布局的工具，但这是另一篇博客的内容。

那么这一切意味着什么呢？这意味着 Flexbox 是一个简单而强大的 CSS 布局模式，它将简化在网页上排列定位元素的行为。首先，您需要将父元素的显示设置为 flex 或 in-line flex:

```
.parent{ 
   display: flex;
}
```

这将允许所有子元素水平显示，而不是垂直显示。您还可以设置 flex-direction 属性，并将其分配给“行”(默认)、“行反转”、“列”和“列反转”。将“column-reverse”添加到父 div 将使子元素再次以相反的顺序垂直显示。

```
.parent{
   display: flex;
   flex-direction: row-reverse;
}
```

例如，如果你的伸缩方向是“行”,默认情况下，flexbox 会将所有子元素放在一行中。或者，您可以将 flex-wrap 设置为“wrap”或“wrap-reverse ”,以便让 flexbox 让元素溢出到下一行，这样每个元素都可以显示为所需的大小。为了利用这一点，您可以将子 div 的 flex-basis 设置为您希望该元素占据的宽度的百分比。如果您为无换行 flex 容器中的所有子元素分配一个总计超过 100%的 flex-basis，则元素将根据它们的宽度如何相互关联而占据相应的宽度。但是，如果您将 flex-wrap 设置为“wrap ”, flex-basis 宽度超过 100%的集装箱将显示在下一行。请注意，flex-basis 不考虑边距，因此具有 50% flex-basis 和较大边距的 2 个元素在 flex-wrap 设置为 wrap 的情况下不适合一行。此外，您可以将 flex-direction 和 flex-wrap 属性与 flex-flow 相结合，并将其赋给一个值，如“行换行”。

```
.parent{
  display: flex;
  flex-flow: row wrap;
}.child-one{
   flex-basis: 50%;
}.child-two{
   flex-basis: 60%; 
   /* will cause child to display on next line, because combined width is 110% */
}
```

如果您想为 flexbox 容器中的元素添加更动态的边距，您可以将父元素的 justify-content 设置为“空格分隔”或“空格分隔”，这将为每个元素添加该边距。如果不需要边距，可以将同一属性设置为 flex-start、flex-end 或 center，这将使所有元素分别位于一行的开始、结束或中心。

关于前面提到的 flex-basis，还有 flex-grow 和 flex-shrink 属性。它们接受一个整数，并定义相对于其他元素，允许它们增长或收缩的可用空间的比例。如果一个 div 设置为 flex-grow:1；而另一个是 flex-grow:2；第二个 div 将占用两倍于第一个 div 的可用空间。也就是说，有一个简写属性“flex”，它粗略地设置了 flex-basis、flex-grow 和 flex-shrink 的值，并且是推荐使用的属性。

```
.child{   
   flex: none | [ <flex-grow> <flex-shrink>? || <flex-basis> ]
}
```

一个更独特的属性是 order 属性。Order 被分配给子元素，它规定了元素定位相对于 flex 容器的所有子元素的顺序。因此，HTML 文件中的第三个 div(在 flex 容器中，order 属性设置为 1)将首先显示在 flex 容器中。

我将在这篇博客中介绍的最后一个属性是 align-items。此属性用于指示 flexbox 容器中的所有项目如何适合该行。这些选项包括弹性起点、弹性终点、中心、拉伸和基线。Flex-start 将从父元素的顶部开始定位每个元素，而 flex-end 将从底部开始定位。拉伸将拉伸每个元素以垂直填充容器。“居中”和“基线”类似，但区别在于“居中”会将每个项目沿父项的横轴对齐，而“基线”会将每个子项的基线与父项的横轴对齐。

一旦你熟悉了这些属性，你就能轻松地在网页上制作出响应性的导航条、侧边菜单和其他容器布局，或许对引导项的使用会少得多。我相信你会发现 flexbox 是一个有用的工具，但是等你学会了网格就知道了！