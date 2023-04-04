# 构建轻量级单页面应用程序

> 原文：<https://blog.devgenius.io/building-lightweight-single-page-apps-6e85925248d5?source=collection_archive---------20----------------------->

我最近的项目是构建一个单页面应用程序。在这篇文章中，我将分享我的策略和一些建议。

![](img/45066fc33bcfb4d83afabf67d8b3882b.png)

Codr 并不是从一个单页面应用程序开始的。每一页实际上，就像任何其他常规网站一样，只是另一页。但是在速度测试和优化离线使用后，我不得不调整我的策略。我必须将单个页面转换成一个单页应用程序；使它更快，更友好，消耗更少的带宽和存储。

在单页应用中，每次导航不需要完全刷新/重新加载网站；相反，只需要加载页面的一小部分并显示给用户。有许多现有的包可以帮你做到这一点(jquery routing、Vue、Reach 等等)。但我更喜欢保持简单轻便。下面是我如何使用普通 JavaScript & jQuery 实现的。

**spa.js**

```
$(window).on('hashchange', function(e) {
    codrRouter();
});function codrRouter() {
  try {
      codrRouter_impl()
  } catch (err) {
      console.error(err)
      $.get('./views/500.html', function(pageContent) {
          $('.content').html(pageContent);
      }).fail(failedGet)
  }
}
```

我们定义了一个页面路由器(codrRouter ),它将成为 SPA 内导航的主干。注意，我们使用“hashchange”事件来触发导航。这意味着我们所有的页面都由 URL 中的#hashtag 标识符来标识。

```
function codrRouter_impl() {
  var page = window.location.hash; if (page === '#' || page === '') { $.get('./views/home.html', function(pageContent) {
      $('.content').html(pageContent);
    }).fail(failedGet) } else if (page === '#challenges') { $.get('./views/challenges_levels.html', function(pageContent) {
      $('.content').html(pageContent);
    }).fail(failedGet) } else { $.get('./views/404.html', function(pageContent) {
      $('.content').html(pageContent);
    }).fail(failedGet) }
}function failedGet() {
  const refresh = '<a class="refreshpage" href=".">refresh page</a>'
  $('.content').html('Oops, make sure you are online.<br>' + refresh);
}
```

现在我们实现路由器和可能的路由。正如您所看到的，每个匹配的路由都发出一个 GET 请求来获取一些 html 内容。此内容只是部分的，它将取代我们的现有代码”。内容”元素，它只是一个占位符。最后一步是确保所有的 a-href 链接都是 hashtags/anchors。

您还可以通过简单地触发 hashchange 事件，以编程方式将用户导航到不同的页面:

```
window.dispatchEvent(new HashChangeEvent("hashchange"));
```

就这么简单，不需要使用任何第三方库。:)