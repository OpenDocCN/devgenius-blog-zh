# 如何将 Swiper.js 与 React 一起使用？

> 原文：<https://blog.devgenius.io/how-to-use-swiper-js-with-react-e2025e92ead3?source=collection_archive---------1----------------------->

![](img/2d7f1fa7a7a9905084b237c52fbab517.png)

图片来自[https://github.com/nolimits4web/swiper](https://github.com/nolimits4web/swiper)

**目的**

Swiper 是一个非常有用的库，介绍了关于滑动的功能，像分页，导航，并且有各种功能。我将分享我关于 swiper 的知识。

**Swiper 是什么？**

下面的句子是由 swiper 摘录的

```
Swiper is the most modern free mobile touch slider with hardware accelerated transitions and amazing native behavior. It is intended to be used in mobile websites, mobile web apps, and mobile native/hybrid apps.Swiper is not compatible with all platforms, **it is a modern touch slider which is focused only on modern apps/platforms to bring the best experience and simplicity**.
```

以防万一，我试图在 Internet explorer 中使用 Swiper(通过使用 IE 标签，这是谷歌浏览器的扩展)，这是行不通的。

因此，如果你想使用 Swiper，你应该检查浏览器。如果你想以私人方式使用 Swiper，这可能没问题，因为目前，几乎所有人都使用现代浏览器(如谷歌浏览器)。然而，如果你想为工作或大公司创建应用程序，你应该检查浏览器，因为他们有时会使用 Internet explorer。(因为我现在在一家日本 IT 公司工作，一些客户现在正在使用 Internet Explorer)。

它是如何工作的？

我会展示我的投资组合网站图像。

![](img/68ed3438b22729e5b96364da56a802ec.png)

您可以选择不同的方式来更改您的多张照片(如翻转、立方体、cover flow)。

swiper.js 不仅有效，还提供了许多不同的功能。

在这篇文章中，基本上，我拿起 swiper 的基本面。如果你想更深入地了解它是如何工作的，可以看看 swiperjs.com 的演示。而且你也可以用 swiper.js 在 vanilla JS，Vue，Angular，Svelte。是的，这个库肯定是为现代浏览器开发的，因为这个库覆盖了 Svelte(我还没有覆盖 Svelte，但我听说它是一个高效开发的好编译器。所以当我了解到 Svelte 的时候，我会分享我的知识)。

**使用步骤**

1.  使用 NPM 安装 Swiper(NPM 安装 Swiper)
2.  导入 Swiper 核心和所需的样式。在我的例子中，我想使用基本的幻灯片，导航，分页，效果翻转。你可以选择任何你想要的选项。如果你想更深入地了解选项，请查看[swiperjs.com](http://swiperjs.com)API。

3.使用 Swiper 和 Swiper 幻灯片在文件中使用。您应该用 Swiper 标签来包装每个 Swiper 幻灯片标签。你可以添加许多选项，如间距，速度，循环，就像默认的那样。还可以添加您导入的功能，如效果翻转、导航、分页。在我的例子中，我想添加三张照片，所以我使用了三个 Swiper 标记，并准备了 data.tsx 文件夹来显示 Swiper slide 标记中的每张照片。

4.搞定了。使用 Swiper 可以轻松使用滑动功能(如果想了解我的整个文件，请查看我的 Github gist。而如果你想看我的全部代码和文件夹结构，请查看我的 GitHub repo)。

**我在介绍 Swiperjs 的时候出现了一个错误**

1.  在官方指南里面，下面的代码是用来导入的

```
import 'swiper/scss';
import 'swiper/scss/navigation';
import 'swiper/scss/pagination';
```

但是它不能正常工作。所以我改变了如下方式。也许这是因为 swiper 或其他依赖版本，但我不太确定为什么它不工作。

```
import "swiper/swiper-bundle.min.css";
import "swiper/swiper.scss";import "swiper/components/navigation/navigation.scss";
import "swiper/components/pagination/pagination.scss";
import "swiper/components/effect-flip/effect-flip.scss";
import "swiper/components/scrollbar/scrollbar.scss";
```

2.类似问题。一个官方指南，下面代码，是真的使用 Swiper。

```
<Swiper
      // install Swiper modules
      modules={[Navigation, Pagination, Scrollbar, A11y]} // I want to focus here
      spaceBetween={50}
      slidesPerView={3}
      navigation
      pagination={{ clickable: true }}
      scrollbar={{ draggable: true }}
      onSwiper={(swiper) => console.log(swiper)}
      onSlideChange={() => console.log('slide change')}
    >
```

但是我不能使用这种模块方式，所以我用 like below 方式代替模块。

```
SwiperCore.use([EffectFlip, Navigation, Pagination]);
```

**参考**

刷机官方网站:【https://swiperjs.com/ 

Github 账号:【https://github.com/aujourdui/portfolio 

Github 要点页面:[https://gist.github.com/aujourdui](https://gist.github.com/aujourdui)

感谢您的阅读！