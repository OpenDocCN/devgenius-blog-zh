# 如何获取 PHP 和 Laravel 中的 referrer URL？

> 原文：<https://blog.devgenius.io/how-to-get-the-referer-url-in-php-and-laravel-cbf2aa883a6?source=collection_archive---------4----------------------->

![](img/c5e6ea2105ea2b13441a702fc74305ba.png)

如果你有一个基于 PHP 或 Laravel 的应用程序，有一段时间想知道用户来自哪里，这对于营销来说是非常重要的。

维基百科是这么说的:

> HTTP referer 是一个可选的 HTTP 头字段，用于标识链接到所请求资源的网页地址。通过检查 referrer，新网页可以看到请求是从哪里发出的。

下面是你如何在 PHP 中不需要任何框架就能做到的:

```
$referer = $_SERVER['HTTP_REFERER'] ?? null;echo $referer;
```

**那么我们如何在 Laravel 应用程序中做到这一点呢？**

在 Laravel 中，您不仅可以使用前面的方法来实现这一点，还可以使用另一种方法来获得 referrer URL:

```
$referer = request()->headers->get('referer');echo $referer;
```

## 产量是多少？

用户点击你的网站链接的页面的 URL，意思是，他们来自哪里。如果用户在浏览器中直接输入你网站的 URL，将会返回`null`。