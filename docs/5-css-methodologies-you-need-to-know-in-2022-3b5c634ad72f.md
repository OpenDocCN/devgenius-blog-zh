# 2022 年你需要知道的 5 种 CSS 方法论

> 原文：<https://blog.devgenius.io/5-css-methodologies-you-need-to-know-in-2022-3b5c634ad72f?source=collection_archive---------1----------------------->

![](img/4755f82fecbc5706497eafe0e0047578.png)

在大型、复杂、快速迭代的系统中，CSS 是出了名的难以维护。CSS 中缺乏内置的作用域机制是原因之一。

在 CSS 中，一切都是全局的。在 CSS 得到它自己的作用域机制之前，我们需要设计自己的系统来将样式锁定到 HTML 文档的特定部分。CSS 方法是解决方案。

本文中，我们就来看看 2022 年你需要知道的 CSS 方法论！

## 1.面向对象的 CSS

[OOCSS](http://oocss.org/) 概念帮助我们编写灵活、模块化和可互换的组件。

例如，按钮元素的样式可能通过两个类来设置，这两个类是您给定的类:

- ".按钮”——提供按钮的基本结构。
——“的意思。灰色-BTN”-应用颜色和其他视觉属性。

CSS:

```
.button {
 box-sizing: border-box;
 height: 50px;
 width: 100%;
}.grey-btn {
 background: #EEE;
 border: 1px solid #DDD;
 box-shadow: rgba(0, 0, 0, 0.5) 1px 1px 3px;
 color: #555;
}
```

HTML:

```
<button class=”button grey-btn”>
 Click me!
</button>
```

## 2.原子 CSS

[原子 CSS](https://acss.io/) 是一种 CSS 架构方法，它支持小型、单一用途的类，这些类的名称基于视觉功能。

示例:

使用十六进制值设置颜色。通过将不透明度值附加到十六进制颜色来创建 Alpha 透明度。

```
<div class=”Bgc(#0280ae.5) C(#fff) P(20px)”>
 Lorem ipsum
</div>
```

## 3.不列颠帝国勋章

[Block Element 修饰符](http://getbem.com)是帮助你在前端开发中创建可重用组件和代码共享的方法论。

示例:

```
<form class=”loginform loginform — errors”>
 <label class=”loginform__username loginform__username — error”> 
 Username <input type=”text” name=”username” />
 </label>
 <label class=”loginform__password”>
 Password <input type=”password” name=”password” />
 </label>
 <button class=”loginform__btn loginform__btn — inactive”>
 Sign in
 </button>
</form>
```

`. loginform `类是由三个元素组成的块:

loginform__username:接受用户名
loginform__password:接受密码
loginform__btn:允许用户提交 web 表单

## 4.SMA CSS

SMACSS 是一种检查你的设计过程的方法，也是一种将那些僵化的框架融入灵活的思维过程的方法。

示例:

假设我们的布局叫做`. l-footer`。我们在里面有一个搜索表单模块。用户已经至少提交了一次搜索表单。

```
<section class=”l-footer”>
 <form class=”search is-submitted”>
 <input type=”search” />
 <input type=”button” value=”Search”>
 </form>
</section>
```

## 5.系统 CSS

[系统 CSS](https://www.yumpu.com/en/document/read/47573458/systematic-css) 分享了许多你可以在 OOCSS、BEM、SMACSS、SUIT CSS 和其他 CSS 方法论中找到的原则和思想。系统化的 CSS 旨在成为现有 CSS 方法的更简单的替代方案。

示例:

下面是呈现导航栏和搜索表单的两个小部件的标记:

```
<! — navigation bar → 
<div class=”NavBar”>
 <ul>
 <li><a href=”./”>Home</a></li>
 <li><a href=”about.html”>About</a></li>
 <li><a href=”learn/”>Learn</a></li>
 <li><a href=”extend/”>Extend</a></li>
 <li><a href=”share/”>Share</a></li>
 </ul>
</div><! — search form → 
<div class=”SearchBox”>
 <form action=”search.html” method=”get”>
 <label for=”input-search”>Search</label>
 <input name=”q” type=”search” id=”input-search” />
 <button type=”submit”>Search</button>
 </form>
</div>
```

内容——以小部件和裸 HTML 元素的形式——然后被放置在布局中。最后，添加修饰符类来改变事物的默认表示。

## 结论

通过提供一种基于类的方法，将大型 web 设计分成许多小型的、模块化的、不同的组件，所有 CSS 技术都解决了 CSS 中的可伸缩性和可维护性挑战。

每个 UI 模块都可以在整个设计中重用，如果 CSS 方法相同，甚至可以从一个项目移植到下一个项目。CSS 方法不仅仅解决 CSS 可伸缩性问题。

## 感谢您的阅读

如果你喜欢这篇文章，请订阅我的[时事通讯](https://newsletter.abhiraj.co)永远不要错过我的博客、产品发布会和技术新闻，并在 [Twitter](https://twitter.com/rainboestrykr) 上关注我，获取关于网络开发资源的每日线程。