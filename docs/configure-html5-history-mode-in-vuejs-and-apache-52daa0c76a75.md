# 在 VueJS 和 Apache 中配置 Html5 历史模式

> 原文：<https://blog.devgenius.io/configure-html5-history-mode-in-vuejs-and-apache-52daa0c76a75?source=collection_archive---------1----------------------->

## 如何通过设置`html5 history mode`删除您的路线 URL 中不需要的 hashbang。

![](img/31899a453f1227bee84b66c5767326fe.png)

Jan baborak 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

使用框架构建 app 时，有时需要配置一些路由。如果您正在使用类似 AngularJs 或 VueJs 这样的框架，您可能会在您的 routes 的 URL 中获得一个免费的 hashbang ( `#!`)，这可能会很烦人。

这意味着我们将得到`#!/myUrl`，当我们想要`/myUrl`的时候。

解决这个问题的方法是在 routes 文件中设置一个 html5 历史模式。你可能会在网上找到答案，例如，在 StackOverflow 的[这篇文章](https://stackoverflow.com/questions/34623833/how-to-remove-hashbang-from-url)中。然而，当我在代码中实现它时，我遇到了问题。这就是为什么我想和你分享我是如何做到的，我是如何解决这些问题的，希望能对任何人有所帮助。

根据您使用的框架，实现这一点的方式会有所不同。我将在 VueJs 中解释我是如何做的。

# 首先，让我们从 URL 中移除 hashbang。

在 VueJs 中，要移除 hashbang，你需要在你的`router.js`中设置模式为`history`。该模式将利用来自 Vue 的`history.pushState`来实现我们的导航，而无需重新加载页面:

```
const router = new VueRouter({
  routes: […],
  root: '/',
  mode: 'history'
});
```

如果你现在尝试运行你的应用，hashbang 就会消失。但是如果你试图刷新任何不是根网址的网址，你会得到一个空白页面或者一个错误。

这就是我如何在开发和生产模式下解决这个问题的:

# **发展中**

我在编码时使用浏览器同步来热重新加载我的应用程序，所以我必须在我的`gulpfile.js`中配置与`browser-sync`相关的任务来使用 npm 包`connect-history-api-fallback`。

因此，我在 gulpfile.js 中添加了相应的任务:

```
var historyApiFallback = require('connect-history-api-fallback');gulp.task('browser-sync', function () {
  return browserSync.init({
    open: true,
    port: 3001,
    startPath: 'index.html',
    notify: false,
    server: {
      baseDir: ['.'],
      middleware: [ historyApiFallback() ]
    }
  });
});
```

注意写着`middleware: [historyApiFallback]`的那一行。这就是你要求`browser-sync`使用 html5 历史模式来实现我们的导航，而不需要重新加载页面。

但是在生产中，我们需要小心地根据这种模式配置我们的应用程序 webserver 否则，每次我们在根(`index.html`)以外的任何地方刷新页面，我们很可能会得到一个 404。

# **生产中**

如果您还记得在托管应用程序的服务器上添加一些配置，以允许应用程序在所有路线上运行，并能够随时刷新页面，这将是最好的。

我在我的 ubuntu 服务器上使用 apache 来为我所有的站点提供服务，所以我做的是在我的应用程序的根目录下添加一个`.htaccess`文件，路径和我的`index.html`一样。我听说有些人把它添加到 apache 配置的根目录中。

看起来是这样的:

```
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

重启 apache 服务(`service apache2 restart`)并刷新您的站点。

所有的路线现在都可以工作了。

# 可能误差

如果在进行这些更改后，您在控制台中得到一个错误消息，说:

`**Uncaught SyntaxError : Unexpected Token <**`

这可能是因为浏览器期望 JavaScript，但是它返回 HTML 作为结果。

你应该看看你的`index.html`和上面定义的所有路径。您可能没有指向正确的路径，因此您的服务器不知道如何安装应用程序。

比如不要设置`src=”main.js”`而是`src=”/main.js”`。
这最后一点也拯救了我的一天！

希望这一切对你有所帮助！