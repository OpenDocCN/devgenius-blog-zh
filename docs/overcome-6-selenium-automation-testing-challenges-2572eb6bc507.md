# 克服 6 项 Selenium 自动化测试挑战

> 原文：<https://blog.devgenius.io/overcome-6-selenium-automation-testing-challenges-2572eb6bc507?source=collection_archive---------13----------------------->

![](img/68ec91a95930fe23bae1b5d3117c90e3.png)

毫无疑问， [Selenium](https://www.bloglovin.com/@shormistha/overcome-6-selenium-automation-testing-challenges) 让 web 测试变得更加简单，对于世界范围内的许多 QA 团队和企业来说，但它也面临着相当大的挑战。也就是说，测试人员遇到的大多数这样的问题都有非常明确的解决方案，这就是为什么我们总结了一些常见的 [Selenium 自动化测试挑战](https://qaandsoftwareblog.blogspot.com/2020/07/overcome-6-selenium-automation-testing.html)，以及他们的解决方案。我们开始吧！

# Selenium 自动化测试挑战和解决方案

1.  [移动应用测试](https://blog.testproject.io/2020/07/01/how-to-overcome-selenium-automation-testing-challenges/#mobile-application-testing)
2.  [报告](https://blog.testproject.io/2020/07/01/how-to-overcome-selenium-automation-testing-challenges/#reporting)
3.  [弹出窗口和警报](https://blog.testproject.io/2020/07/01/how-to-overcome-selenium-automation-testing-challenges/#alerts)
4.  [页面加载](https://blog.testproject.io/2020/07/01/how-to-overcome-selenium-automation-testing-challenges/#page-load)
5.  [可扩展性](https://blog.testproject.io/2020/07/01/how-to-overcome-selenium-automation-testing-challenges/#scalability)
6.  [桌面应用测试](https://blog.testproject.io/2020/07/01/how-to-overcome-selenium-automation-testing-challenges/#desktop-apps)
7.  [结论](https://blog.testproject.io/2020/07/01/how-to-overcome-selenium-automation-testing-challenges/#conclusion)

![](img/a203cf95ace833721c330bb7324e2aa0.png)

# 1.移动应用测试

当使用 [Selenium](https://blog.testproject.io/category/selenium/) 时，我们可以在桌面的任何操作系统和浏览器上创建[自动化测试用例](https://blog.testproject.io/2019/03/07/what-test-cases-should-you-automate/)，但是我们不能同时使用 Selenium 执行移动测试。

***解决方案* :** 您可以利用 [Appium 移动应用自动化工具](https://blog.testproject.io/2020/02/05/the-best-appium-tools-for-mobile-developers/)等工具来处理使用 WebDriver 协议的 iOS 和 Android 应用。随着越来越多的用户选择移动设备，测试人员和开发人员利用 Appium 的功能也就不足为奇了。

还有基于 Selenium 和 Appium 的工具，如 [TestProject](https://testproject.io/) ，为需要测试 web 和移动应用的场景提供了一个很好的解决方案。在考虑移动自动化测试工具时，一定要看看这个速度基准:[最快的移动自动化测试工具——app ium vs . XCUITest vs . test project vs . ui automator vs . Espresso](https://blog.testproject.io/2020/04/08/fastest-mobile-automation-testing-tool-appium-vs-xcuitest-vs-testproject-vs-uiautomator-vs-espresso/)。

# 2.报告

报告被认为是 Selenium 自动化测试中面临的主要挑战之一，因为 Selenium 本身不支持有效的报告。您可以使用各种包或第三方工具，如 [TestNG](https://blog.testproject.io/2019/10/31/the-power-of-testng-test-next-generation/) 、 [Pytest](https://blog.testproject.io/2019/07/16/python-test-automation-project-using-pytest/) 等。然而，这些选项要求用户具有高级编程技能，并且它们并不总是足够精细。

***解决方案*** :使用 [TestProject](https://testproject.io/) 您可以动态地创建详细的报告，而不需要在您的 Selenium 代码或记录中明确提到任何报告步骤，也不需要任何额外的工具。这一切都为你做好了，你会得到[深入的报告](https://testproject.io/beautiful-test-analytics/)，里面有解决自动化测试问题和测量产品质量所需的所有必要的见解和趋势。

# 3.弹出窗口和警报

在测试自动化过程中，我们经常会收到提示或确认弹出警告，但是 Selenium 经常无法捕捉这样的弹出警告。自动关闭或接受这些警报可能是最棘手的工作。Selenium 不能处理基于 windows 的警报，因为它们是操作系统而不是浏览器的组件。然而，Selenium WebDriver 确实能够处理基于 web 的警报。

***解决方案*:**[Selenium web driver](https://blog.testproject.io/2020/06/30/selenium-4-new-window-tab-screenshots/)可以操作各种窗口。基于 Web 的警报通常可以用 [switchTo 方法](https://blog.testproject.io/2020/06/18/selenium-switch-methods-chapter-5/)来处理，以控制弹出窗口，同时允许浏览器在后台运行。

要管理任何类型的警告弹出窗口，您还可以应用 getAlert 函数。在运行脚本之前，您应该导入一个可以创建 WebDriver 脚本来处理警报的包。capable 接口带来了后续的命令: *void accept()、void dismiss()、getText()、void sendKeys(String String tosend)*。前两次点击弹出窗口上的“确定”&“取消”按钮。

# 4.页面加载

一些页面根据不同的用户行为和旅程加载不同的元素。有时，一些元素会基于用户过去的动作而出现。例如，如果您从下拉列表中选择一个国家，那么与该特定国家相关的城市将加载到城市下拉列表中。在执行 Selenium 测试时，脚本可能无法定位元素。

***解决方案* :** 为了解决这个特定的问题，我们可以在 Selenium 脚本中使用显式等待来为元素提供足够的时间来上传和定位元素。您还可以检查 TestProject 的[自适应等待能力](https://blog.testproject.io/2020/05/04/testproject-adaptive-wait-capability/)，并确保避免任何测试剥落。

# 5.可量测性

尽管 Selenium 使您能够在任何浏览器或操作系统上进行测试，但它仍然受限于一次可以运行多少测试以及执行测试的速度。没有 Selenium 网格，您只能一个接一个地执行测试。 [Selenium Grid](https://blog.testproject.io/2019/07/16/using-selenium-grid-and-remote-webdrivers/) 可以使用远程 WebDrivers 扩展您的测试过程，并支持并行运行测试。然而，你不能在多种浏览器操作系统组合中测试你的网络应用。您一次只能在特定的操作系统和浏览器(安装在您的机器上)上执行测试。

***解决方案*** :你可以使用云测试解决方案(TestProject 内置了与[酱实验室](https://docs.testproject.io/testproject-integrations/sauce-labs-integration)或[浏览器栈](https://docs.testproject.io/testproject-integrations/browserstack-integration)的集成)，同时跨多个操作系统和浏览器测试你的应用。您可能会发现这篇关于 5 种云解决方案的比较文章很有意思，值得一读:[在云中执行测试自动化的 5 大解决方案(综合比较)](https://blog.testproject.io/2020/02/04/top-5-solutions-executing-test-automation-cloud-comprehensive-comparison/)。

# 6.桌面应用程序测试

这是自动化测试面临的主要挑战之一。Selenium 不支持基于 windows 的[桌面测试自动化](https://blog.testproject.io/2016/12/22/9-free-desktop-test-automation-tools/)。它只支持基于 web 的应用程序。

***解决方案* :** 不完全是解决方案，但是我们需要记住，Selenium 不能自动化每一件事情。仍然有一些类型的任务需要手动执行。因此，如上所述，理解编程语言对于使用 Selenium 这样的工具至关重要。

# 结论

Selenium 自动化测试可能很棘手，但毫无疑问是 web 测试之王。虽然[自动化测试用例](https://www.bloglovin.com/@shormistha/overcome-6-selenium-automation-testing-challenges)无疑可以快速识别和解决应用代码库中的问题，但挑战并不那么容易解决。因此，需要考虑并相应地实施额外的工具或方法。

**在 Selenium 自动化测试** **期间遇到其他** [**挑战？在下面的评论里分享吧！**](https://qaandsoftwareblog.blogspot.com/2020/07/overcome-6-selenium-automation-testing.html)

***你可以关注我:***

*   **宗**:【https://dzone.com/users/3854036/shormistha.html】T2
*   Quora:[https://www.quora.com/profile/Shormistha-Chatterjee](https://www.quora.com/profile/Shormistha-Chatterjee)
*   **博主**:[https://shormistha4.blogspot.com/](https://shormistha4.blogspot.com/)，[https://qaandsoftwareblog.blogspot.com/](https://qaandsoftwareblog.blogspot.com/)
*   **布洛格洛文**:[https://www.bloglovin.com/@shormistha](https://www.bloglovin.com/@shormistha)