# Selenium 版本 4:自动化测试的全新方法

> 原文：<https://blog.devgenius.io/selenium-version-4-an-all-new-approach-to-automated-testing-3b5422e12cac?source=collection_archive---------2----------------------->

![](img/bd8c4cb09985cbc65e72f81f469e5e25.png)

Selenium 是对网站应用程序进行自动跨浏览器测试时最流行的工具套件。而且，WebDriver 的创建者和 Selenium 项目的核心贡献者，名叫 Simon Stewart，在 2018 年公开介绍了 Selenium 4。

从那时起，新版本的 Selenium 因其最新的特性和功能而获得了 s [elenium 测试服务](https://www.bugraptors.com/selenium-testing-services.php)的大量关注。此外，好消息是 Selenium 4 的测试版已经在[官方网站](https://www.selenium.dev/downloads/)上发布，你可以查看并下载开始使用。

您准备好了解 Selenium 版本 4 的新特性了吗？听起来很棒！我们先来了解一下它的基础。

当你想到“硒”的时候，你首先想到的是什么？因为大多数人，当他们听到 Selenium 时，他们首先想到的是 Selenium WebDriver，这确实发生在许多人身上，也许你也有同样的想法。

然而，有两个部分有助于组成 Selenium 测试项目，它们是 Selenium IDE 和 Selenium Grid。了解有助于组成 s [elenium 自动化测试](https://www.bugraptors.com/blog/cross-browser-automation-testing-perform)的点点滴滴是必要的，因为这个过程将帮助你理解 Selenium 的第四版，并获得自动化测试的新方法。

下面让我们读一点关于 Selenium WebDriver、Selenium IDE 和 Selenium Grid 的内容:

# 什么是 Selenium WebDriver？

因此，Selenium WebDriver 是一个开源 API，它让您可以像人类用户一样与计算机上的浏览器进行交互。它主要是为浏览器自动化而构建的。

# 什么是硒 IDE &硒格？

Selenium IDE 是最著名的记录和回放自动化工具。而 Selenium Grid 通过将测试分布在不同的机器上来帮助您节省时间。

现在，是时候让我们了解一下**——Selenium 4 有什么新功能？**

Selenium 4 是一个加载的 Selenium 测试框架，它有很多优秀的特性，比如改进的 Selenium 网格架构、相对定位器和非常有用的 Selenium IDE。

Selenium 4 中最重要的变化之一是 WebDriver APIs 的 W3C 兼容性，这与更少的不可靠和更稳定的跨浏览器测试相关。

**以下是您在使用 Selenium 4 时会遇到的一些常见变化:**

# Selenium WebDriver W3C 标准化

在 Selenium 3(Selenium 的早期版本)中，JSON Wire 协议负责维护 web 浏览器和测试代码之间的对话或通信。

因此，API 查询必须使用 W3C 协议进行编码和解码，这是一项复杂的任务和非常耗时的过程。这将在带有 WebDriver 的 Selenium 4 中改变，因为在这个版本中，WebDriver APIs 将遵循 W3C 标准化。

像 Chromedriver，geckodriver 这样的浏览器驱动会遵循 W3C 标准，这是 Selenium 4 中的 Webdriver，它会与 web 浏览器进行直接通信。

此外，Selenium vs web driver W3C 协议与 JSON Wire 协议密切相关。而且，您会发现它是 Selenium 4 中的默认协议，因为 JSON Wire 协议在 Selenium 的新版本中将被丢弃。

此外，在 Selenium 4 中取消了对 Opera 和 PhatomJS 的本机支持，因为不需要正在开发的 WebDriver 实现。

原因是 Opera 浏览器是基于 Chromium 的，计划在 Opera 上测试其实现的人不能选择它在 Chrome 浏览器上用 selenium 进行测试。另一方面，PhantomJS 用户可以选择在 Firefox 和 Chrome 上以无头模式进行测试。

因此，WebDriver W3C 协议是 Selenium 4 中最大的架构变化。因此，与 Selenium 的早期版本相比，WebDriver W3C 标准化为跨浏览器测试提供了稳定性。

# 增强和优化的 Selenium Grid 4

您用来分布测试执行的 Selenium 网格是基于 Hub-Node 架构的。因此，在执行自动化测试时，有一种将中心和节点分开的冲动，因为中心和节点被合并到一个单独的 jar 文件中。并且，在服务器启动后，它同时充当集线器和节点。

此外，Selenium Grid 4 的早期版本提供了对路由器、分发者进程、会话映射等的支持。由于更具可伸缩性和可跟踪性的基础设施，Selenium Grid 4 提供了对路由器、会话映射、分发器和节点等四个流程的支持。

这个网格现在将支持 IPV6 地址，并帮助用户使用 HTTPS 协议与网格通信。因此，使用改进的 Selenium 网格配置文件要容易得多，因为用户可以使用人类可以理解的 Tom 的显而易见的最小化(TOML)语言轻松地配置网格。

此外，Selenium 4 中的网格具有用户友好的图形用户界面，Selenium Grid 4 中的 Docker 兼容性使得在虚拟机上使用成为可能。

另外，用户可以灵活地选择在 Kubernetes 上部署 Selenium 网格。此外，您可以在 DevOps 过程中利用 Selenium Grid 4，因为它支持 Azure、AWS 等工具。

# 改进的硒 4 IDE

Selenium IDE 对于自动化测试工程师来说是无处不在的，因为他们知道如何用它来执行记录和回放测试。

它是 web 测试可用的最佳解决方案之一，可以帮助您获得开箱即用的结果。但是早期的 Selenium IDE 只能在 Firefox 扩展上使用，没有令人兴奋的特性。

有了 Selenium Version 4，Selenium IDE 可以成为 Firefox 和 Chrome 等主流浏览器的有用解决方案。此外，Selenium IDE 的 web 扩展或插件预计将很快出现在 MS Edge 商店中。

# 更好的文档

由于我们已经收到了 Selenium 4 的详尽的官方文档，内容布局将使您只需点击几下鼠标就能轻松获得所需的信息。

此外，您需要了解 Selenium 4 处于 Alpha 阶段，这意味着您将获得改进的文档，以了解 Selenium 4 中的新 API 和特性如何工作，以及如何在测试代码中使用它们。

此外，在 Selenium 4 文档中还介绍了 Selenium Grid 4 和 WebDriver W3C 协议的各个方面。同样，作为 Selenium 自动化测试公司的自动化测试人员，您可以使用文档来熟悉 Selenium 4 提供的新 API。

# Selenium 4 驱动程序更新

团队最终消除了一些你之前被警告过的方法。这项清理工作需要很长一段时间。然而，非常好的消息是，从驱动程序的接口、类到方法代码库都被彻底清理了。

**比如:**

迭代键盘的替换已经用动作和按键输入代替了。所有的修改列表你可以直接在官方文档中查看。如果您担心在替换一些东西后如何管理所有东西，那么没有必要担心，因为 Selenium 确实专注于维护与遗留版本的向后兼容性。

这意味着你可以很容易地将最新的版本添加到你的旧测试中，并且如果一些方法被团队否决了，你也可以顺利地工作。Rest，您在实现或使用 Selenium Version 4 时可以获得更好的体验。

# 自动化脚本的相对定位器

这是这个新版本中最令人惊奇的功能之一，以相对定位器而闻名，可以用于自动化脚本编写。您可能知道定位器，因为它们在 Selenium 中已经存在很多年了，通过链接文本、XPath、CSS 选择器等等进行查找。

定位器的问题在于，它要求你对页面的结构进行大量的研究，并且你必须知道 DOM 本身是如何排列的。

有时，由于人类语言的要求，使用定位器成为一项更具挑战性的任务。然而，你不需要担心，因为相对定位器也会减轻你的压力。

因为这是一种方法，在这种方法中，您根据元素相对于其他 web 元素的位置，使用相当容易理解的语法告诉 Selenium 需要交互的元素。

此外，这些概念长期以来都是测试自动化的一部分，可能会出现在供应商工具中，如 Quick Test Professional。而且，这些概念是许多其他开源和免费自动化框架的一部分，如 Taiko 和 Sahi。

此外，你会很高兴地知道，最后，它是你硒的一部分。如果你参加过任何关于自动化的会议，你可能知道一些寻找复杂 web 元素的最佳方法。定位器策略技术包括非常复杂的 CSS、XPath 或不同定位器的堆叠上传。

**例如，新相对位置的语法如下:**

//导入静态 org . open QA . selenium . support . locators . relative locator . with tagname；WebElement password field = driver . find element(by . id(" password "))；WebElement email address field = driver . find element(with tagname(" input ")。以上(password field))；

//导入静态 org . open QA . selenium . support . locators . relative locator . with tagname；WebElement email address field = driver . find element(by . id(" email "))；WebElement password field = driver . find element(with tagname(" input ")。下面(email address field))；

//导入静态 org . open QA . selenium . support . locators . relative locator . with tagname；WebElement cancel button = driver . find element(by . id(" cancel "))；WebElement submit button = driver . find element(with tagname(" button ")。toRightOf(cancel button))；

在这种语法中，相对定位器是将这种策略封装在代码中的一种尝试。要查找元素，您可以说“查找元素”，并告诉“哪种”元素是您要查找的？

例如，搜索“此处上方”和“此处下方”的输入元素，或者您可以找到“此处右侧”的输入元素，它会考虑所有过滤器并说，“好的，这是您想要或要搜索的元素。这是一个独特的优势，您可以获得使用更人性化的语言来查找元素。

所以，这种方法并不复杂。尽管如此，如果您不了解 box 模型，也不熟悉 Selenium 自动化测试，那么您需要学习用这些友好的定位器加载您的测试用例。

大多数时候，你可以从 WebDriver 中得到你想要做的事情。然而，如果您未能从 WebDriver 获得最佳响应，那么这可能是由于您的计算机擅长的指令，但人们不喜欢它们。

# 使用 Selenium 的 Chrome 调试功能

这是人们在使用 Cypress.io 这样的工具和 Chrome DevTool 协议调试信息时经常谈论的另一个特性。事实上，您需要理解它附带了许多基于当前调试协议的特性。

而且，最好的部分是 Selenium 现在是 chrome 浏览器 DevTools 调试协议中的一个新特性。同样，这是 Chrome 调试协议，也定义为 CDP，是调试工具用来与 Chrome 通信的底层协议。

此外，它的行为几乎像一个机器代码。例如，下面，我们给出了一个你可能想写的代码。

包主要导入(

“上下文”

" fmt "

" io/ioutil "

"日志"

“时间”

" github.com/mafredri/cdp "

" github . com/mafredri/CDP/dev tool " " github . com/mafredri/CDP/protocol/DOM " " github . com/mafredri/CDP/protocol/page " " github . com/mafredri/CDP/rpcc "

)

func main()

{

错误:=运行(5 *次。第二)如果 err！= nil { log。致命(错误)

}

}

func 运行(超时时间。持续时间)误差

{

ctx，取消:=上下文。WithTimeout(上下文。Background()、timeout) defer cancel() //使用 DevTools HTTP/JSON API 管理目标(如页面、webworkers)。

devt := devtool。新(" http://127.0.0.1:9222 ")

pt，err := devtGet(ctx，devtool。页面)

如果 err！=零

{

pt，err = devt。如果出错，创建(ctx )!= nil {返回错误}

}

你们中的许多人不喜欢遵循这种方法，但当手中没有其他选项时，你们必须选择。因此，流行的库将下划线协议包装在一个更加用户友好的语法层中。

另一方面，像 Puppeteer 这样的工具不使用原始的 Chrome 调试协议(CDP):因为它在原始的 CDP 之上开发了一个用户友好的 API 层，以便于交互。然而，这个工具的问题是它只使用 JavaScript 语言。

假设你可以选择使用不同的语言。在这种情况下，如果工具没有按照您的首选语言制作，您该怎么办。你能学习不同的编程语言吗？否则，你不能直接使用 Chrome 调试协议，因为它完全是用机器语言编写的。同样，您可能不喜欢编写需要直接 WebDriver 协议的代码。

然而，您会很高兴地知道 Selenium Contributors 在版本 4 中提供了易用性，并在版本 4 中采用了一些常见的用例，并在 CDP 的顶部实现了它们，这些用例只使用友好的 API，感觉像 Selenium-ish，并可以与它的任何语言绑定一起使用。

现在是时候将 Selenium CDP 地理定位代码片段与您在我们的博客中看到的原始 CDP 语法进行比较了。

导入 org . open QA . selenium . chrome . chrome driver；

导入 org . open QA . selenium . dev tools . dev tools；

public void geolocation test(){ chrome driver driver = new chrome driver()；

Map coordinates = new HashMap()

{{

put(“纬度”，50.2334)；put(“经度”，0.2334)；put("准确度"，1)；}};

driver . executecdpcommand(" emulation . setgeolocationoverride "，coordinates)；driver . get(" ")；}

与之前的 CDP 代码相比，这段代码看起来非常容易阅读和编写。因此，这是 Selenium 4 与旧版本相比的巨大差异。

Selenium 4 的主要好处之一是，它允许您对以前和最新版本的 Chrome 进行测试，包括最新和旧版本的 Edge 以及许多其他浏览器供应商的版本。因此，您的测试将按照您的预期顺利进行，最重要的是您需要在使用新版本的 API 或浏览器时下载它们。此外，它还让您能够使用关于安全性和性能的重要测试信息。下面是一些最好的 Chrome 开发属性，您可以在测试中探索和获取:

*   应用程序缓存
*   取得
*   网络
*   表演
*   仿形铣床
*   资源计时
*   安全
*   目标 CDP 域

# 硒的遥测特性

为了与软件测试趋势同步发展，开发人员还在新网格中加入了一种称为开放遥测的技术，您可以考虑将其用于分布式跟踪。

此外，支持开放遥测的其他工具也可以使用开放遥测数据，如蜂巢、Jaeger、Datadog 等。

如果您是系统管理员，您会喜欢这一点，因为您可以简单地将其集成到现有的基础架构中，甚至弄清楚正在发生的事情，并了解网格中正在发生的事情。此外，如果您在运行硒网格测试时需要更多的透明度，您可以很容易地做到。

# 硒网格和 GraphQL

在网格中，您可以找到一个新的、创新的前端控制台，它由 GraphQL 端点驱动。此外，您可以在本地机器上或以分布式方式高效地对硒网格运行 GraphQL 查询。不仅如此，它还允许您提取一大堆有用的信息。

下面是使用新的 GraphSQL API 运行网格中每个节点的查询时可以检查的语法。

curl -X POST -H“内容类型:应用程序/JSON”—数据“{查询:“{ grid { nodes { status } } }”}“”-s

# 西蒙关于硒的最好建议

西蒙认为，如果你想从自动化工作中获得最大的利益，你应该记住测试金字塔。如果您有几个小测试、集成测试、技术和端到端硒测试，您就做得很好。

然而，如果您有数十万个端对端测试和五个单元测试需要使用 Selenium 测试框架来运行，那么您可能会面临痛苦。

[硒第 4 版](https://www.bugraptors.com/blog/selenium-version-4-approach-to-automated-testing)