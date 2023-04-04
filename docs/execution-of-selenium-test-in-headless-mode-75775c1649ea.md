# 在无头模式下执行 Selenium 测试

> 原文：<https://blog.devgenius.io/execution-of-selenium-test-in-headless-mode-75775c1649ea?source=collection_archive---------4----------------------->

在以前的博客中，我们已经看到了一些与用于 UI 自动化的 selenium web 驱动程序相关的概念。你可以在这里阅读

[Selenium Web 驱动简介](https://monilnigdi.medium.com/introduction-to-selenium-web-driver-9b3fbe227d27)

在本文中，我们将看到在 headless 模式下使用 Selenium Web 驱动程序执行用户界面测试的情况。

当您在目标浏览器上开始执行 UI 测试时，test 做的第一件事是启动目标浏览器，在该浏览器上 test 启动被测应用程序，并继续测试的其余部分。但是，如果执行发生在后台，而没有启动任何浏览器呢？您可以平静地处理另一项任务，而不会受到每次测试启动的浏览器实例的干扰。

那么没有浏览器到底是什么，有没有可能执行它，尤其是在 UI 自动化的情况下。

是的。这是可能的。在这种情况下，我们将不得不在 headless 模式下执行我们的测试自动化。无头模式通常在没有任何 GUI 的情况下在目标浏览器上执行测试。该执行将像任何其他后台进程一样发生。

Selenium 支持在无头模式下执行。所有支持的浏览器都有各自的能力在无头模式下执行测试。

首先，在无头模式下执行测试需要什么

1.  多任务:如果您的测试用例正在执行，同时您正在使用同一台机器为自动化开发做贡献，那么每次测试都会启动一个新的浏览器窗口，它会在您的屏幕上弹出，足以分散您对自动化任务的注意力。因此，如果您有 150–200 个测试执行，随机出现的弹出窗口显然会让您对开发自动化感到不舒服。在这种情况下，如果您在 headless 模式下执行测试，您可以专注于其他任务，测试用例的执行将在后台进行。
2.  减少执行时间:在无头模式下，测试执行比启动浏览器的正常执行花费更少的时间，因为在启动实际浏览器后，页面不会像正常执行那样完全呈现
3.  降低 CPU 使用率:每当我们启动浏览器时，CPU 使用率都会突然增加。当我们在 headless 模式下执行相同的测试时，CPU 的使用似乎相对较少。
4.  在 CI/CD 执行中有帮助:由于上述所有原因，这是在 CI/CD 执行中执行测试的首选方式，因为大多数这些执行都是夜间执行，没有人会实际监视完整的执行。
    另一个原因是，公司专门用一台机器来执行 CI/CD 管道，该管道将始终启动和运行。如果我们使用 HTMLUnit 驱动程序执行 headless 模式，我们不需要在那台机器上安装任何真正的浏览器。

我们将看到执行无头执行的不同方式。

1.  HtmlUnitDriver
2.  《人鬼情未了》
3.  Watir-webdriver
4.  僵尸
5.  幻象

除此之外，我们将在本文中看到 HTMLUnit 驱动程序的无头执行以及它如何与真正的浏览器一起工作。

HTMLUnitDriver: HTMLUnit 是一个基于 Java 的 web 浏览器实现，没有 GUI。你可以查看它的 [git 库](https://github.com/HtmlUnit)。以下是使用 HTMLUnitDriver 进行无头执行的优点

1.  它被认为是 WebDriver 最快的轻量级实现。
2.  与其他浏览器相比，它的执行速度更快，因为不需要在执行机器上进行安装。
3.  由于第二点，使用 HTMLUnitDriver 的 Headless 是使用 CI/CD 执行的首选。
4.  您可以使用 HTMLUnitDriver 在选定的浏览器版本上执行测试

让我们看看如何将 HTMLUnitDriver 与 Selenium 一起使用:

如果您使用的是较旧的 selenium 版本，即 2.53 或更低版本，那么不需要添加任何 jar 或依赖项，因为它内置了对 HTMLUnitDriver 的 headless 支持。当我在 Gradle 项目上工作时，我将使用下面的 Selenium 依赖项。

```
implementation group: 'org.seleniumhq.selenium', name: 'selenium-java', version: '2.53.1'
```

复制下面的代码

```
// Creating an instance of the HTMLUnitDriver
WebDriver driver = new HtmlUnitDriver();

// Navigate to Google
driver.get("http://www.google.com");

// Locate the element SearchBox using locator strategy name with value q
WebElement element = driver.findElement(By.*name*("q"));

// Enter a search query
element.sendKeys("Selenium");System.*out*.println("Test Executed");
```

如果您执行这个脚本，您将看不到浏览器的启动和进一步的操作，但您的测试仍将被执行。我在最后打印了一条确认信息。

默认情况下，HTMLUnitDriver 不支持 JavaScript 代码。如果您在脚本中使用 JavaScriptExecutor 执行任何 Javascript 操作，它将会失败。为此，您必须在创建驱动程序实例时通过传递布尔值来启用 javascript。

```
WebDriver driver = new HtmlUnitDriver(true);
```

如果你想在一个特定的浏览器版本上执行你的测试，你也可以提到浏览器版本，例如，如果你想在 Firefox 版本 38 上执行你的测试，你可以把它作为一个参数添加进去。

```
WebDriver driver = new HtmlUnitDriver(BrowserVersion.*FIREFOX_38*,
        true);
```

但是考虑到人们将使用 Selenium 的最新版本，即 3.159，我在使用 HTMLUnitDriver 时遇到了一个错误。所以我将使用一个真实的浏览器在 headless 模式下执行 Selenium 测试。

我将展示如何在 Chrome 和 FireFox 的无头模式下执行 Selenium 测试

这里我使用网络驱动管理器来自动下载浏览器驱动。你可以阅读这篇文章来更好地理解它。

[**驱动简介&Selenium 中的 Web 驱动管理器**Web 驱动](https://monilnigdi.medium.com/introduction-to-drivers-web-driver-manger-in-selenium-web-driver-d433d780ad79)

使用 Chrome 在 headless 模式下执行测试。我们必须使用一个名为 ChromeOptions 的类，并将参数作为 headless 传递。然后将这个 ChromeOption 类的对象传递给驱动程序实例，如下所示

```
WebDriver driver;WebDriverManager.*chromedriver*().setup();ChromeOptions chromeOptions = new ChromeOptions();chromeOptions.addArguments("--headless");driver = new ChromeDriver(chromeOptions);driver.get("http://www.google.com");WebElement element = driver.findElement(By.*name*("q"));element.sendKeys("Selenium");
```

如果你使用的是 Selenium 3.x，有一个专门的方法，你可以使用 ChromeOptions 类的 setHeadless()并传递布尔参数 true。

```
WebDriver driver;WebDriverManager.*chromedriver*().setup();ChromeOptions chromeOptions = new ChromeOptions();chromeOptions.setHeadless(true);driver = new ChromeDriver(chromeOptions);driver.get("http://www.google.com");WebElement element = driver.findElement(By.*name*("q"));element.sendKeys("Selenium");
```

我们可以使用 Firefox 浏览器执行相同的操作

```
WebDriver driver;WebDriverManager.firefoxdriver().setup();FirefoxOptions firefoxOptions = new FirefoxOptions();firefoxOptions.setHeadless(true);driver = new FirefoxDriver(firefoxOptions);driver.get("http://www.google.com");WebElement element = driver.findElement(By.*name*("q"));element.sendKeys("Selenium");
```

在无头模式下执行测试有一定的限制。

1.  它不会复制精确的用户行为，因为在无头模式下，网页不会完全呈现所有 UI 依赖项，而这些依赖项将在实际浏览器中加载网页时呈现。
2.  万一失败，我们将不得不写额外的日志或代码来获取日志或失败的截图。这将使框架变得不稳定，调试变得困难。在这种情况下，在真正的浏览器中用 headed 模式执行测试用例将会很有帮助，因为我们可以观察测试用例的执行并发现问题

这是关于在无头模式下的执行测试。在决定执行无头模式还是有头模式之前，设定你的目标。

快乐学习..！！