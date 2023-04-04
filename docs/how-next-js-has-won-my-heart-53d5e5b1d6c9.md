# 如何赢得我的心🥰

> 原文：<https://blog.devgenius.io/how-next-js-has-won-my-heart-53d5e5b1d6c9?source=collection_archive---------6----------------------->

![](img/9ce2d173cfd835b697c47b56da3358e5.png)

哦，我今年一定要上 Next.js！

抱歉，笨蛋。再见，盖茨比。

最近发布的 Next.js(版本 10.0.0)引入了许多我期待已久的激动人心的特性。这无疑使它对开发者和企业更有吸引力，因为它已经成为一个可靠的生产就绪的 web 框架，具有许多有前途的好处。下面是一些主要特性更新的概要，这些更新让我对 Next.js 了如指掌。

# I .自动图像优化

NextJS 10 引入了新的内置图像组件“next/image”，作为 HTML 的标签的扩展，提供了**自动延迟加载**和**响应图像**。

👇工作原理:

**在 HTML 中:**

```
<img src=”/my-image.jpg” ...>
```

**在 Next.js:**

```
import Image from 'next/image'
<**Image** src="/my-img.jpg" ...>
```

🤖 [*什么是懒加载？*](https://web.dev/lazy-loading/)

Next.js next/image 组件还**通过内置的图像优化自动生成更小尺寸的图像**，自动为 WebP 等现代图像格式的图像提供服务，提供比 JPEG 小 30%的图像(如果浏览器支持的话)。

# 二。国际化

不仅 Next.js 10 带来了对国际化路由和语言检测的**内置支持，而且它还支持两种最流行的国际化路由策略:“**子路径路由**和“**域路由**”，这两种策略都适用于 SSG 或 SSR。**

👇工作原理:

**子路径路由**示例(在 next.config.js 中):

例如，如果您有一个 *pages/blog.js* ，则以下 URL 可用，此设置将导致以下域路径:
-英语: **/blog**
-法语: **/fr/blog**
-韩语: **/ko/blog**

**域路由**示例(在 next.config.js 中):

“next.config.js”中的域路由示例

在此设置中，如果您有一个 *pages/blog.js* ，将自动生成以下 URL:
-英语:***yourdomain.com/blog***
-法语:***yourdomain.fr/blog***
-韩语:***yourdomain.kr/blog***

# 三。href 自动解算的 SSG/SSR 混合算法

你准备好了吗？
Next.js 现在可以在单个项目中**在构建时预渲染页面(SSG，静态站点生成)**和 **SSR(服务器端渲染)！**

此外，href 的新**自动解析是锦上添花:在 Next.js 10 中，对于大多数用例，您不再需要在链接中使用“as”属性！**

例如，在这个标签中:

```
 <Link href=”/categories/[slug]” *as=”/categories/books”*>
```

您可以删除作为装饰者的*。这种改变是完全向后兼容的——如果您当前同时使用“href”和“as ”,现有的行为将被保留。*

*—了解更多关于*[***href***](https://nextjs.org/docs/api-reference/next/link)的自动解析

除了上面提到的这些令人敬畏的特性，[next . js v . 10 的其他一些特性是](https://nextjs.org/docs/getting-started):

*   **Next.js Analytics**
*   **支持打字稿**
*   **快速刷新**
*   **内置 CSS 支持，允许你从 JS 文件导入 CSS 文件**
*   **Next.js 商务**
*   **API 路线**

现在是时候完成一些工作了！