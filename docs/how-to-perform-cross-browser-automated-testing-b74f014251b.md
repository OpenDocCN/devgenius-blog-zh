# 如何进行跨浏览器自动化测试？

> 原文：<https://blog.devgenius.io/how-to-perform-cross-browser-automated-testing-b74f014251b?source=collection_archive---------19----------------------->

![](img/57e325d03f20678b0c81cbb58c0f0d36.png)

跨浏览器测试是一种用于在多种浏览器中测试网站或应用程序行为的技术。有必要确保您的 web 应用程序或网站在所有浏览器上都能准确运行，因为浏览器呈现网站的方式不同，在这种情况下，布局、特性或功能可能会出现差异和错误。它包括检查您的应用程序在多个 web 浏览器之间的兼容性。跨浏览器测试包括在使用不同的 Web 浏览器访问 Web 应用程序时测试客户端和服务器端的行为。

随着各种 web 浏览器的出现，最终用户使用各种 web 浏览器来访问 web 应用程序。基于 Web 的应用程序与 Windows 应用程序完全不同。最终用户可以在任何浏览器中打开 web 应用程序。因此，在多种浏览器上测试 web 应用程序变得至关重要。客户端组件，如 JavaScript、AJAX 请求、小程序、Flash、Flex 等。可能在不同的浏览器上表现不同。此外，不同浏览器的请求处理过程也各不相同。

**如何自动化跨浏览器测试？**

I)在 Selenium WebDriver 的帮助下，我们可以自动化测试用例，并跨各种浏览器(如 Internet Explorer、Firefox、Chrome 和 Safari 浏览器)同时运行这些测试用例。

ii) [带 Selenium WebDriver 的 TestNG 框架](https://www.bugraptors.com/blog/introduction-testng-framework-benefits-junit-framework)用于在同一时间同一台机器上使用不同浏览器执行相同的测试用例。

Selenium WebDriver 可用于在 Chrome、Firefox、Internet Explorer、Safari 和 Opera 等各种浏览器上运行浏览器兼容性测试。WebDriver API 像真实用户一样驱动 web 浏览器。Firefox 驱动默认有一个“selenium-server-standalone.jar”库，而 Chrome、IE、Safari 和 Opera 确实需要外部库。为了执行[跨浏览器自动化测试](https://www.bugraptors.com/automation-testing-services.php)，需要为各种浏览器初始化 WebDriver。让我们讨论各种浏览器以及为不同浏览器初始化 WebDriver 的步骤。

**1。Mozilla Firefox:**‘selenium-server-standalone’库与 Firefox 驱动程序捆绑在一起，用于在 Firefox 浏览器中初始化和运行测试。在启动 Firefox 驱动程序的新实例时，Firefox 驱动程序会作为文件扩展名添加到 Firefox 配置文件中。要启动 Mozilla Firefox，代码是:

web driver driver = new Firefox driver()；

**2。Google Chrome:** Chrome Driver 是一个外部库文件，它利用 WebDriver 协议在 Google Chrome web 浏览器中运行 Selenium 测试。要启动谷歌浏览器，代码是:

system . set property(" web driver . chrome . driver "，" C:\ \ chrome driver . exe ")；

web driver driver = new chrome driver()；

**3。ie 浏览器:** IEDriverServer 是一个可执行文件。它使用 WebDriver wire 协议来控制 Windows 中的 IE 浏览器。它支持从 6 到 10 的 IE 版本。要启动 Internet Explorer，代码是:

system . set property(" web driver . ie . driver "，" C:\ \ iedriverserver . exe ")；

desired capabilities DC = desired capabilities . internet explorer()；

DC . set capability(InternetExplorerDriver。引入 _ 剥落 _ 被 _ 忽略 _ 安全 _ 域，真)；

web driver driver = new InternetExplorerDriver(DC)；

**4。苹果 Safari:** 就像 FirefoxDriver 一样，SafariDriver 也在内部捆绑了最新的 Selenium 服务器，从而在没有任何外部库的情况下启动苹果 Safari 浏览器。SafariDriver 支持 Safari 浏览器 5.1.x 版本，只能在 MAC 上运行。要启动 SafariDriver，代码是:

web driver driver = new safari driver()；

**5。Opera:** OperaPrestoDriver(以前叫 OperaDriver)只适用于基于 Presto 的 Opera 浏览器。目前不支持 Opera 及以上版本。但是，最近发布的基于 Blink 的 Opera 浏览器(Opera 15.x 及以上版本)是使用 OperaChromiumDriver 处理的。要启动 OperaChromiumDriver，代码是:

desiredpabilities capabilities = new desiredpabilities()；

capabilities . set capability(" Opera . binary "，" C://Program Files(x86)//Opera//Opera . exe ")；

capabilities . set capability(" opera . log . level "，" CONFIG ")；

WebDriver driver = new OperaDriver(能力)；

**跨浏览器自动化测试的优势**

I)使用 Selenium 可以实现不同类型浏览器的自动化。

ii)通过将 Selenium 与 TestNG 集成，可以执行多浏览器测试。

iii)测试脚本是可重用的，并且可以跨各种环境、参数和场景使用。

iv)自动化测试提供了手工测试无法达到的一致性水平。

这篇文章最初发表在[bug raphorn](https://www.bugraptors.com/blog/cross-browser-automation-testing-perform)上。