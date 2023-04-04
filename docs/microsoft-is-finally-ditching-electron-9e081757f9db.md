# 微软终于抛弃了电子

> 原文：<https://blog.devgenius.io/microsoft-is-finally-ditching-electron-9e081757f9db?source=collection_archive---------0----------------------->

微软最近宣布，他们的团队已经拥有了高达 2.5 亿的活跃用户。不是 Word 或 Excel，而是 Teams 是微软办公套件的摇滚明星。但是，它一直受到性能问题的困扰，因为它消耗大量的系统资源。在内存较少的计算机上运行团队是一场噩梦。

在由海洋蓝和珍珠白小部件组成的超现代、流畅的用户界面背后，它运行着一个以 [Chromium](https://www.chromium.org/chromium-os) 实例形式存在的恶魔，它控制着主机，无意中让它们的风扇运转起来。

![](img/b6252efb618f3710ec3a3782059c7372.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6281710) 的[欧宰尔·阿迈德](https://pixabay.com/users/uzair_ahmed-20868928/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6281710)

# 微软的声明

微软团队的高级副总裁[宣布](https://twitter.com/rishmsft/status/1408085784016539653)团队将转向他们自己的 Edge Webview2 渲染引擎，放弃电子以寻求性能提升。据宣传，由于这种转变，团队消耗的内存将减少 2 倍。它将被称为 Teams 2.0，可能在 2022 年底与 Windows 11 一起发布。

# 电子问题

电子驱动的知名[应用](https://www.electronjs.org/apps)数不胜数。电子框架帮助 web 开发人员将他们的 web 应用程序转移到桌面平台，避免了特定平台的复杂性。由于 Chrome OS 的一个独特实例运行在每个电子应用程序的后端，运行两个以上这样的应用程序会消耗主机的能量。

在 Electron 上进行大量处理的 Teams 已经成为占用内存和降低计算机速度的同义词。微软甚至有一个文档[页面](https://docs.microsoft.com/en-us/microsoftteams/teams-memory-usage-perf)解释为什么微软团队可能有高内存使用率。

# Webview2 上团队的起源

不能认为 Webview2 是电子版的替代品；它不是一个像电子一样的包装器，可以在桌面平台上快速发布网络应用。最初的 Webview (Webview1)使用微软的 Edge 渲染引擎，而 Webview2 使用 Chrome 渲染引擎。Webview2 已经被 Outlook 用作微软“一个 Outlook”项目的一部分。

![](img/03f899c79950512e00e5189ecd8faa32.png)

图片来源:微软

与 electronic 不同，WV2 监控 Chromium 的行为，以检测有多少系统内存可用，并有效地利用这些内存来优化渲染体验。如果其他应用或服务需要系统内存，Chromium 就会将这些内存让给这些进程。这大大提高了内存较少的低端计算机的性能。

WV2 可以认为是一个像 app 的窗口一样的控件；呈现网页的控件。事实上，Webview2 控件允许在本机应用程序中嵌入 web 技术(HTML、CSS 和 JavaScript)。对于一个团队规模的应用程序来说，要过渡到 WV2，Electron 提供的许多抽象必须重写。因此，团队本质上会变得更接近原生 Windows 应用。

# 前进的道路

团队在音频和视频方面做了很多事情，微软认为最好是在本机级别上卸载一些工作，Webview2 促进了这些工作，这些工作无法通过电子抽象有效地完成。

团队基本上对电子来说太大了。早在 2017 年，电子是桌面和网络上的一个伟大和唯一的选择，但优化是技术进步的唯一途径。这可能是不断变化的跨平台框架中的一个关键里程碑。这也可能只是一个团队的具体变化，只有微软才能完成。