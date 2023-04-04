# CSS 文件—变量

> 原文：<https://blog.devgenius.io/the-css-files-variables-41ea10ce6dd8?source=collection_archive---------4----------------------->

![](img/b6a24e8eff6f702e969793c3b4323bdb.png)

编写大量 CSS 的最大问题之一是保持一致性。例如，在一个大的代码库中，一种颜色可以在数百个地方使用数百次。这种重复使维护变得困难，因为像改变颜色这样的简单设计调整可能导致需要在许多许多地方进行更改。

像 [Sass](https://sass-lang.com/) 和 [Less](https://lesscss.org/) 这样的 CSS 预处理器试图通过包含变量来解决这个特殊的问题(以及其他问题)。变量允许您在一个地方设置颜色和大小等公共值，然后在需要使用这些值时引用变量。现在只需要在一个地方进行简单的颜色改变。

然而，现代的 CSS 非常强大，CSS 现在有了对变量的原生支持。你不需要任何构建工具或管道；它们只是语言的一部分。您编写的代码就是浏览器使用的代码。形式上，变量是“自定义属性”，但通常简称为变量。让我们探索如何使用它们，以及它们如何使编写 CSS 成为一种更好的体验。

# 声明和使用变量

“自定义属性”这个名字的一个显著特点是它们*是* CSS 属性。因为它们是属性，所以必须像任何其他属性一样在 CSS 规则中声明:

```
/* Nope! */
--brand-color: #003d7d;.foo {
    /* Yep! */
    --brand-color: #003d7d;
}
```

为了区分变量和标准属性，它们必须以-。任何名字都可以使用，只要你只使用字母，数字和破折号。请记住，它们是区分大小写的，所以— foo 和— Foo 不是同一个变量。

变量也是 CSS 属性正常级联的一部分。这实际上意味着您希望在文档树的最高元素上声明全局变量，这个元素几乎总是 html。但是，如果您有适用于 html 元素的样式，并且希望将变量声明分开，那么通常的做法是使用:root 伪类:

```
:root {
    --brand-color: #003d7d;
}
```

变量的值可以是许多不同的东西，因为基本上任何有效的 CSS 值都可以是变量的值。大小和颜色只是开始，因为整个边界和背景值也可以存储。您也可以使用一个变量的值来设置另一个变量。可能性是无限的。

```
:root {
    --brand-color: #003d7d;
    --spacing: 4px;
    --spacing-large: calc(var(--spacing) * 2);
    --border: 2px solid var(--brand-color);
    --info-icon-bg-image: url('data:/...');
}
```

在上面的代码示例中，您可能注意到了我们如何访问变量值:var()函数。该函数的第一个参数应该是您需要从中获取值的变量。第二个(可选)参数用作后备值。如果你不确定一个变量是否已经被设置，这是很有用的，并且可以用于[各种有趣的把戏](https://css-tricks.com/a-complete-guide-to-custom-properties/#h-the-initial-and-whitespace-trick)。

```
:root {
    --some-color: #003d7d;
}.widget {
    background-color: var(--some-color);
    font-size: var(--font-size, 16px);
}
```

在这个例子中，由于我们没有声明一个 font-size 变量，var()将返回 16px，因为这是后备值。注意，只有第一个值被认为是要评估的变量，和变量值一样，回退值*可以包含逗号*。例如，var( — foo，red，blue)定义了红色、蓝色和*的回退，而不是*两个独立的回退值。

# 变量作用域

我之前提到过变量是正常级联的一部分。这意味着在根作用域声明的变量是全局的，适用于页面上的所有内容，但是级联还实现了两个惊人的特性。

首先，您可以在较小的范围内改变变量。想象一下，一个页面上的所有按钮都使用一种全局颜色。如果你想让一个特定类型的小部件中的所有按钮都有不同的颜色，你可以在小部件类中重新声明这个变量，这个小部件中的所有按钮都将使用重新定义的颜色。

```
<style>
:root {
    --btn-color: #003d7d;
}.widget {
    /* Redefine the value here so any buttons within a .widget will use this color */
    --btn-color: #00aeea;
}.btn {
    color: var(--btn-color);
    border: 1px solid var(--btn-color);
}
</style><button class="btn">Normal Button</button><div class="widget">
    <button class="btn">Button with a different color</button>
</div>
```

这开启的第二个特性是能够将变量的范围扩大到特定的组件。如果您在. widget 规则上设置了一个类似于-widget-color 的变量，那么只有. widget 元素的后代元素才能使用-widget-color 变量。试图在声明该值的范围之外访问该值将导致没有值被使用(不是错误)。

# 变量的优势

变量是 CSS 的一个很好的补充。它们有很大的潜力通过统一不同的价值来简化大型代码库。它们还可以简化规则，允许您根据元素的状态或类更改特定的值，而不需要重新声明整个属性。因为变量可以在不同的地方更改，包括在@media 查询中，所以可以很容易地将系统亮/暗模式设置集成到您的站点中。结合强大的 calc()函数，您可以将几个简单的变量变成一个全面的设计系统。

它们是 CSS 预处理器的替代品吗？不。Sass 和 Less 仍然有许多 CSS 所缺乏的独特功能。它们允许嵌套规则以减少选择器的重复。它们具有 CSS 中难以复制的颜色功能。他们仍然可以做浏览器不能做的事情，但是他们需要某种构建过程来生成可以使用的 CSS。

总而言之，预处理器仍然有它们的位置，但是本地变量扩展了 CSS 的能力，而不需要任何额外的工具。

# 了解更多信息

*   [自定义属性完全指南——CSS 技巧](https://css-tricks.com/a-complete-guide-to-custom-properties/)
*   [使用 CSS 自定义属性(变量)— MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)
*   [实用 CSS 自定义属性使用模式— CSS 窍门](https://css-tricks.com/patterns-for-practical-css-custom-properties-use/)

## ——安德鲁·莫斯卡迪诺，AWH[的软件开发员。我们正在帮助企业通过技术推动增长。](http://awh.net)