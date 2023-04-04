# JavaScript 最佳实践— SEO

> 原文：<https://blog.devgenius.io/javascript-best-practices-seo-631eee9d3ca0?source=collection_archive---------11----------------------->

![](img/fe43669068998955c09f8119cfb945c1.png)

马丁·莫雷诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 搜索引擎优化

SEO 让我们的网站出现在搜索结果的前几页。

通过这种方式，我们可以获得更大的影响力。

对于大量使用 JavaScript 的网站，我们需要做更多的工作来改进我们的网站。

Google 用爬虫处理 JavaScript 来索引页面内容。

它等待页面呈现，然后索引呈现的内容。

如果页面允许爬行，它就会这样做。

为了使渲染速度更快，我们可以在服务器端或伪装者我们的内容。

要轻松做到这一点，我们可以使用 Next 或 Nuxt 这样的框架来实现。

# 用独特的标题和片段描述我们的页面

我们应该添加独特的标题和片段，以便他们可以很容易地识别。

# 编写兼容的代码

JavaScript 为开发者提供了许多 API。

我们应该确保它们不会被弃用，并且与大多数浏览器兼容，以防止出现与浏览器相关的问题。

# 使用有意义的 HTTP 状态代码

Google 使用 HTTP 状态代码来发现在抓取页面时是否有错误。

我们应该使用有意义的层云代码来告诉 Google 是否应该抓取某个页面。

常见的包括 404 表示未找到，401 或 403 表示访问被拒绝。

301 或 302 是重定向响应。

500 系列错误是服务器端的错误。

# 避免单页应用中的软 404 错误

当发出 HTTP 请求时遇到 404 错误，我们应该重定向到一个未找到的页面。

或者我们必须使用 JavaScript 将`<meta name=”robots” content=”noindex”>`添加到错误页面。

这样，谷歌知道这是一个 404 页面。

重定向方法可以写成如下形式:

```
fetch(`/api/items/${itemId}`)
.then(response => response.json())
.then(item => {
  if (items.exists) {
    showItem(item); 
  } else {
    window.location.href = '/not-found';
  }
})
```

当项目不存在时，我们重定向到“未找到”。

或者，要创建 meta 元素，我们可以用 JavaScript 来完成:

```
fetch(`/api/items/${itemId}`)
  .then(response => response.json())
  .then(item=> {
    if (item.exists) {
      showItem(item); 
    } else {
      const metaRobots = document.createElement('meta');
      metaRobots.name = 'robots';
      metaRobots.content = 'noindex';
      document.head.appendChild(metaRobots);
    }
  })
```

如果没有找到条目，我们就创建 meta 元素。

# 使用历史 API 而不是片段

历史 API 允许我们使用它进入不同的页面。

它实现了我们的 web 应用程序的不同视图之间的路由。

这确保谷歌可以找到链接。

所以与其改变片段来导航:

```
<nav>
  <ul>
    <li><a href="#/home">home</a></li>
    <li><a href="#/profile">profile</a></li>
  </ul>
</nav><script>
window.addEventListener('hashchange', () => {
  const pageToLoad = window.location.hash.slice(1); 
  document.getElementById('placeholder').innerHTML = load(pageToLoad);
});
</script>
```

相反，我们通过编写来使用历史 APU:

```
<nav>
  <ul>
    <li><a href="#/home">home</a></li>
    <li><a href="#/profile">profile</a></li>
  </ul>
</nav><script>
function goToPage(event) {
  event.preventDefault(); 
  const hrefUrl = event.target.getAttribute('href');
  const pageToLoad = hrefUrl.slice(1);
  document.getElementById('placeholder').innerHTML = load(pageToLoad);
  window.history.pushState({}, window.title, hrefUrl)
}document.querySelectorAll('a').forEach(link => link.addEventListener('click', goToPage));</script>
```

我们在第二个例子中使用历史 API 来导航。

我们调用`preventDefault`来防止以默认方式进入`href`，这样我们就可以用 JavaScript 导航。

接下来，我们用`hrefUrl.slice(1);`删除前导斜杠

然后我们调用`pushState`来使用历史 API 导航。

这样，历史被保存，我们可以导航。

![](img/2f1c81e5ecd1c293f393e392371ccb30.png)

丹·图卡文在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用历史 API 来更新浏览器历史。

此外，我们可以看看在我们的客户端 web 应用程序中进行 SEO 的各种方法。