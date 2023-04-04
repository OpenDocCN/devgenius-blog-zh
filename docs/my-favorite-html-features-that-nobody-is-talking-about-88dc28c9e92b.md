# 我最喜欢的没有人谈论的 HTML 特性

> 原文：<https://blog.devgenius.io/my-favorite-html-features-that-nobody-is-talking-about-88dc28c9e92b?source=collection_archive---------0----------------------->

## 你应该知道的有用的 HTML 特性和技巧。

![](img/b98bd3030c33442bf23ffea135d7fc08.png)

照片由[穆罕默德·拉赫马尼](https://unsplash.com/@afgprogrammer?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

HTML 是一种强制性的网络标记语言。如果你连起码的一些 HTML 基础知识都不知道，别跟我说你是个 web 开发人员。HTML 的好处是它有很多我们可以从中受益的标签、属性和特性。有时候，使用这种标记语言可以做很多事情，而不需要其他语言，比如 CSS 和 JavaScript。

在这篇文章中，我想和你分享我最喜欢的 HTML 特性，我没有看到很多人谈论这些特性。所以让我们开始吧。

# 颜色选择器功能

仅使用原生 HTML，我们就可以轻松地在网页上创建一个简单的颜色选择器。不需要 JavaScript 和 CSS。

你只需要使用标签`<input>`，并在属性`type`中给它一个值`color`。

这里有一个例子:

```
<label for="color-picker">Pick a color: </label>
**<input type="color" id="color-picker">**
```

正如您所看到的，我们还添加了 label 标签以方便访问。

看看下面的演示吧:

来自 [Codepen](https://codepen.io/) 的作者演示(外链)。

# 标签数据列表

标签`<datalist>`是真正让我惊讶的 HTML 标签之一。它允许您轻松地创建一个搜索栏，用户可以在其中搜索具有自动完成功能的元素。

为了实现这一点，您将需要一个具有属性`list`的 input 元素，然后创建一个 datalist 标记，其中包含一系列 option 标记。

这里有一个例子:

```
<form>
        <input **list**="**myFruits**">
        <**datalist** id="**myFruits**">
          <option value="Apple">
          <option value="Orange">
          <option value="Banana">
          <option value="Mango">
          <option value="Avocado">
        </**datalist**>
        <input type="submit" value="Submit">
 </form>
```

另外，请记住，您传递给标签`<datalist>`的 ID 应该与您传递给输入标签中的 list 属性的值相同。

查看下面的演示:

来自 [Codepen](https://codepen.io/) 的作者演示(外链)。

# 手风琴特征

你知道吗，你可以只用原生 HTML 而不用任何 JavaScript 来创建一个手风琴？

为此，你只需要使用标签`<details>`和`<summary>`。它们允许创建可以通过单击(折叠)来查看和隐藏的元素。

这里有一个例子:

```
<details>
  **<summary>**HTML**</summary>**
  <p>HTML is not a programming language(lol).</p>
</details>**<details>**
  **<summary>**CSS**</summary>**
  <p>CSS is Awesome</p>
**</details>**
```

为了更好地理解，请查看下面的演示:

来自 [Codepen](https://codepen.io/) 的作者演示(外链)。

# 隐藏的属性

另一个特性是属性`hidden`。它允许在网页上隐藏 HTML 元素。您可以将该属性添加到任何想要的 HTML 元素中。

这里有一个例子:

```
<h3 **hidden**>This element won't show up on the page.</h3>
```

# 缩写标签

HTML 中的标签`<abbr>`允许您在用户使用鼠标悬停在文本上时显示文本的缩写。

下面是代码示例:

```
<p>**<abbr title="Hyper text markup language">HTML</abbr>** is not a programming language.</p>
```

看看下面的演示吧:

作者来自 [Codepen](https://codepen.io/) 的一个演示(外链)。

# 所需的属性

HTML 中我喜欢的另一个特性是属性`required`。它允许您轻松地对表单输入进行一些 HTML 验证，如电子邮件验证、用户名验证和密码验证。

您只需要在输入元素上添加属性`required`。如果您想添加自己的正则表达式，也可以使用名为`pattern`的属性。

下面是一个代码示例:

```
<h1>The input required attribute</h1><form>
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" **required**>

  <label for="email">Email:</label>
  <input type="email" id="username" name="email" **required**>
  <input type="submit" value="Submit">
</form>
```

现在，如果您试图在没有输入有效数据的情况下提交表单，将会出现一个警告，告诉您输入有效的数据。所以所有的验证都是在幕后完成的，你只需要添加属性`required`。

下面是要检查的演示:

作者来自 [Codepen](https://codepen.io/) 的一个演示(外链)。

# 结论

正如你在上面看到的，这些是我最喜欢的 HTML 特性。它们非常有用，让你无需编写 JavaScript 或 CSS 代码就能实现不同的目标。

感谢您阅读这篇文章。我希望你觉得它有用:

**更多阅读:**

[](https://javascript.plainenglish.io/7-awesome-react-ui-libraries-for-all-frontend-developers-c7d45b7cecad) [## 面向所有前端开发人员的 7 个非常棒的 React UI 库

### 可以在 React 中使用的有用 UI 组件库的列表。

javascript.plainenglish.io](https://javascript.plainenglish.io/7-awesome-react-ui-libraries-for-all-frontend-developers-c7d45b7cecad) [](/4-ways-to-find-clients-as-a-freelance-developer-67a42b76ecc7) [## 自由开发人员寻找客户的 4 种方法

### 作为自由职业者如何寻找潜在客户？

blog.devgenius.io](/4-ways-to-find-clients-as-a-freelance-developer-67a42b76ecc7)