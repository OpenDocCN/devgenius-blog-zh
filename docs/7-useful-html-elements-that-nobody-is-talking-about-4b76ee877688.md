# 没人谈论的 7 个有用的 HTML 元素

> 原文：<https://blog.devgenius.io/7-useful-html-elements-that-nobody-is-talking-about-4b76ee877688?source=collection_archive---------0----------------------->

## 作为一名 web 开发人员，你应该知道的一系列很棒的 HTML 元素和标签。

![](img/1dc6a1d0b5bc4e6381ca0dc49d4b0488.png)

穆罕默德·拉赫马尼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

HTML 是一种非常重要的网络标记语言。每个网站或 web 应用程序都使用 HTML 来定义 web 内容的结构和含义。

因此，通过使用 HTML 或所谓的“*超文本标记语言*”，你将能够定义和组织网页上的内容，以便在网络浏览器上显示。

HTML 的好处是它非常简单易学。它拥有您需要使用的所有元素和功能。因此，当你创建网站或网络应用程序的框架时，HTML 将一直存在。

这种语言有许多强大的元素和标签，您可以在代码中使用。还有很多 HTML 元素，我没看到很多开发者用。有些人，甚至不知道他们。

这就是为什么我想写这篇文章，列出一些你可能不知道的非常有用的 HTML 元素。所以让我们开始吧。

# 1.缩写元素

当您想要在网页上显示文本缩写时，HTML `<abbr>`元素非常有用。当用户将鼠标悬停在某个缩写上时，它允许您显示该缩写的含义。

您所要做的就是使用标签`<abbr>`并赋予它作为标题属性的文本含义。

看看下面的代码示例:

```
<p><**abbr** **title**="JavaScript and XML">JSX**</abbr>** is very awesome.</p>
```

您可以在下面的代码栏中预览该示例:

作者的例子来自 Codepen(外部网站)。

# 2.仪表元件

HTML 中的元素`<meter>`也是另一个开发人员不太了解的元素。它允许您轻松地突出显示和定义进度条中的数量值。所有这些都不需要使用任何 CSS 或 JavaScript。

看看下面的代码示例:

```
<label>Low<label>
**<meter** min="0" max="100" low="30" high="75" optimum="80" **value="20"></meter>**<label>Medium<label>
**<meter** min="0" max="100" low="30" high="75" optimum="80" **value="60"></meter>**<label>High<label>
**<meter** min="0" max="100" low="30" high="75" optimum="80" **value="85"></meter>**
```

要理解 HTML 中的 meter 元素是如何工作的，您可以查看下面的 Codepen 示例:

作者的例子来自 Codepen(外部网站)。

# 3.字段集元素

HTML 元素`<fieldset>`允许你在一个表单中放置和分组元素。除此之外，它还可以在不使用任何 CSS 的情况下在元素周围绘制边框。

下面是代码示例:

```
<form>
 **<fieldset>**
   <label for="fname">Full Name:</label>
   <input type="text"><br><br>
   <input type="submit" value="Submit">
 **</fieldset>**
</form>
```

所以你只需要使用标签`<fieldset>`并将其他 HTML 代码放入其中，这样所有的元素都在一个边框内。这将很容易将元素分组并在它们周围绘制边框。

您可以查看下面的 Codepen 示例:

Codepen 作者的例子。

# 4.细节元素

你想用 HTML 而不是 CSS 或 JavaScript 创建一个手风琴吗？嗯，HTML 中的元素`<details>`将允许您这样做。

你所要做的就是使用标签`<details>`并在标签内使用标签`<summary>`。这将允许用户通过单击鼠标来查看和隐藏元素。

下面是一个代码示例，这样您可以了解更多:

```
**<details>**
  **<summary>**What is JavaScript?**</summary>**
  <p>JavaScript is a programming and scripting language.</p>
**</details>**
```

看看下面的 Codepen 示例，并尝试单击元素:

Codepen 作者的例子。

# 5.元素数据列表

你知道吗，只用 HTML 就可以在输入中轻松创建下拉搜索文本？元素`<datalist>`让你轻松实现。

元素提供了一个选项列表，您可以在输入中搜索这些选项。最棒的是，当你搜索选项时，它有一个自动完成功能。所有这些都只需要原生 HTML。

你只需要创建一个输入元素，并赋予它一个名为`list`的属性，该属性可以是你想要的任何值。然后将`<option>`标签放入元素`<datalist>`中。

下面是代码示例:

```
<input **list="fruit"**>
**<datalist** **id="fruit"**>
    <option value="Banana">
    <option value="Orange">
    <option value="Apple">
    <option value="Mango">
    <option value="Avocado">
  **</datalist>**
```

注意，Datalist ID 必须与我们在 input 标记中添加的 list 属性具有相同的值。

如果您想更好地理解元素`<datalist>`是如何工作的，请看下面的 Codepen 示例:

作者的例子来自 Codepen(外部网站)。

# 6.HTML 颜色选择器元素

要创建一个只使用原生 HTML 的颜色选择器元素，您所要做的就是编写一个 input 元素，然后给它一个值为 *color* 的 type 属性。

看看下面的代码示例:

```
<input **type="color"** id="color-picker">
```

您可以在下面的示例中查看它:

作者的例子来自 Codepen(外部网站)。

# 7.进度元素

HTML 中的元素`<progress>`允许你不使用 CSS 或 JavaScript 就能轻松创建简单的进度条。

看看这个代码示例:

```
<**progress value="45"** max="100">45%</**progress**>
```

所以你可以，你只需要定义你想要的进度值，然后你还要加上一个最大值，我想这个最大值总是 100。

查看下面的 Codepen 示例:

Codepen 作者的例子。

# 结论

正如你在上面看到的，这是 7 个有用的 HTML 元素的列表，大多数开发者可能不知道。那是因为 HTML 教程里没人讲这些。

这些元素非常强大和有用，它们让你只用 HTML 就能创造出令人惊叹的东西。

*感谢您阅读本文。此外，如果你发现我的内容有用，而你不是一个媒体成员，你可以抓住你的媒体成员* [***这里***](https://mehdiouss.medium.com/membership) *(媒体推荐链接)获得无限制的* ***访问媒体上的所有内容*** *并支持我们作为作家。*

[](https://mehdiouss.medium.com/membership) [## 通过我的推荐链接加入 Medium-Mehdi Aoussiad

### 阅读 Mehdi Aoussiad(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接支持…

mehdiouss.medium.com](https://mehdiouss.medium.com/membership) 

**更多阅读:**

[](/11-useful-github-repositories-to-become-a-pro-react-developer-530c565eac2a) [## 成为专业 React 开发人员的 11 个有用的 GitHub 库

### 2022 年每个 web 开发者都应该知道的 Awesome React GitHub 仓库。

blog.devgenius.io](/11-useful-github-repositories-to-become-a-pro-react-developer-530c565eac2a) [](https://javascript.plainenglish.io/11-useful-frontend-web-developer-web-apps-to-boost-productivity-c8b2afeab251) [## 11 个有用的前端 Web 开发人员 Web 应用程序来提高工作效率

### 2022 年每个网络开发者都应该使用的令人敬畏的网络应用。

javascript.plainenglish.io](https://javascript.plainenglish.io/11-useful-frontend-web-developer-web-apps-to-boost-productivity-c8b2afeab251)