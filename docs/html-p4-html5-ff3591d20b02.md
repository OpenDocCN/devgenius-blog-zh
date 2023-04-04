# HTML — P4: HTML5

> 原文：<https://blog.devgenius.io/html-p4-html5-ff3591d20b02?source=collection_archive---------11----------------------->

![](img/cad2831daf1cc1888d40a62f0377c7e9.png)

HTML5 是 HTML 的最新版本，于 2014 年由 W3C 标准化。HTML5 是为了取代 HTML4 和 XHTML 而创建的。HTML5 为开发者提供了 web 应用的 API。它还为等式引入了新的语法。HTML5 增强了我们创建的内容的语义，尤其是当内容被分组在一起时。HTML4 或其前身中没有的某些标签包括:

```
<video>
<audio>
<section>
<article>
<header>
<footer>
<nav>
<aside>
<main>
```

W3 标准由万维网联盟(W3C)创建和更新。W3C 是一个国际合作组织，试图标准化我们开发网站的方式。强烈建议您查看 https://www.w3.org/TR/中列出的标准。它会让你成为一个更好的网站设计师，并有助于规范网络开发行业。

观察这些标签，您会注意到它们是以人类可读的格式编写的。如果您正在创建一篇文章，请将文本放在一个`<article>`标签中。标签可以放在其他标签中。例如，您可以从标记`<header>`开始，它表示其中的内容将位于屏幕的顶部。头里面是什么？你可以利用`<nav>`标签放置一个顶部导航。您将在后面的文章中学习如何设置导航栏的样式。

在导航之后，您可以添加一个`<section>`来保存相关内容的集合。您可以继续添加部分，直到到达底部。自然，网站的页脚位于页面的底部。您将添加`<footer>`标签，然后向其中添加正常观察到的页脚内容。

有了`<audio>`和`<video>`标签的加入，HTML5 接管 Flash 也就不足为奇了。视频和音频标签可以添加到部分或文章中。它们可以被添加到`<section>`或`<article>`标签之外，但是，请记住，我们正试图标准化我们处理网页设计的方式。

您可能想知道 HTML5 能否在旧版本的 Internet Explorer 上正确显示。尽管旧版本已经过时，但仍有相当多的版本存在。为了确保旧版本的 Internet Explorer 知道如何正确呈现 HTML5 页面，您需要在 head 标记之间包含一个“垫片”。

```
<head>
    <meta charset="UTF-8"> <title>Shim Include</title>
    <!--[if It IE 9]> 
        <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js". 
        </script>
    <![endif]--> 
</head>
```

这应该就是你需要知道的关于 HTML5 的全部内容。如果您有兴趣了解更多关于 HTML5 的知识，有数不清的关于它的书籍。

![](img/5cf38b405952e0743b5156188fd36dd8.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，[访问他的博客](https://www.dinocajic.com/)，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。