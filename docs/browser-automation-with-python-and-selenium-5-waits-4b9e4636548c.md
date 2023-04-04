# 使用 Python 和 Selenium 实现浏览器自动化— 5:等待

> 原文：<https://blog.devgenius.io/browser-automation-with-python-and-selenium-5-waits-4b9e4636548c?source=collection_archive---------0----------------------->

## 确保它在那里

![](img/518a7dcbd68e182e616980438223b7f6.png)

照片由[迈克·范·登博斯](https://unsplash.com/@mike_van_den_bos?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在[上一篇](https://medium.com/python-in-plain-english/browser-automation-with-python-and-selenium-4-locating-elements-171ea5100490)文章中，我们研究了如何在与元素交互之前定位元素。在本文中，我们将探讨 Selenium 中的等待是如何确保元素在与它们交互之前出现在 DOM 中的。

HTML `document`的`document.readyState`属性描述了当前文档的加载状态。默认情况下，当就绪状态变为`complete`时，`driver.get`请求会返回给调用者。

Selenium WebDriver 有三种[页面加载策略。](https://www.selenium.dev/documentation/en/webdriver/page_loading_strategy/)

**正常**

等待整个页面加载完毕，这是由`[load](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event)`事件决定的。

**急切的**

等到初始 HTML 文档被完全加载和解析，没有样式表、图像和由`[DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event)`事件指定的子框架。

**无**

如果页面加载策略设置为 none，Selenium WebDriver 只会等到初始页面下载完毕。

当 Selenium WebDriver 加载页面时，它默认使用`normal`策略。

页面上的不同元素可以在不同的时间加载。这可能会导致定位元素失败。如果一个元素不在 DOM 结构中，当我们试图访问和操作它时，我们会得到一个异常。这些问题可以通过 Selenium 中的等待来解决。

在文档完成加载后添加的元素无法通过`find_*`方法找到。在这种情况下，我们的自动化代码将引发`NoSuchElementException`。等待允许自动任务执行在继续下一步之前经过一段时间。

有 3 种等待:

1.  含蓄的
2.  明确的
3.  流利的

## 隐式等待

使用隐式等待，WebDriver 在引发异常之前会在指定的时间内轮询 DOM。默认设置为 0。一旦设置，它在 WebDriver 实例的整个生命周期内都有效。

在下面的例子中， *id* 被故意错误地赋予了`find_element_by_id`方法。因为我们将隐式等待设置为 10 秒，所以我们将在等待 10 秒后得到异常。

```
# output after 10 seconds
Message: Unable to locate element: [id="typer"]
```

隐式等待很容易使用，但是由于等待间隔对所有元素都有效，并且在 WebDriver 实例的生命周期内，它们会增加总的执行时间。

Selenium 提供显式等待来避免这些硬编码全局等待的缺点。

## 显式等待

与硬编码的隐式等待不同，显式等待等待特定条件的发生。它也仅适用于给定的特定元素，而不是全局的所有元素。

以一定的频率(默认为 0.5 秒)调用该条件，直到等待超时。如果条件失败，则引发`TimeoutException`。

Selenium Python API 为常见情况提供了一些方法。

它们在`selenium.webdriver.support`包下的`expected_conditions`模块[中定义。](https://github.com/SeleniumHQ/selenium/blob/trunk/py/selenium/webdriver/support/expected_conditions.py)

预期条件列表

```
* alert_is_present
* element_located_selection_state_to_be
* element_located_to_be_selected
* element_selection_state_to_be
* element_to_be_clickable
* element_to_be_selected
* frame_to_be_available_and_switch_to_it
* invisibility_of_element
* invisibility_of_element_located
* new_window_is_opened
* number_of_windows_to_be
* presence_of_all_elements_located
* presence_of_element_located
* staleness_of
* text_to_be_present_in_element
* text_to_be_present_in_element_value
* title_contains
* title_is
* url_changes
* url_contains
* url_matches
* url_to_be
* visibility_of
* visibility_of_all_elements_located
* visibility_of_any_elements_located
* visibility_of_element_located
```

在[接下来的示例](https://github.com/coskundeniz/selenium-examples/blob/main/waits/explicit_wait.py)中，检查文本的结尾是否有动画元素。一旦满足条件，wait 返回 true，就可以打印全文。

```
# output
Full text typed: Ask Doctor Python to fix your code...
```

**自定义等待条件**

如果前面的方法不能满足您的要求，您还可以创建自己的自定义等待条件。可以使用带有`__call__`方法的类创建一个定制的等待条件，当条件不匹配时，该方法返回`False`。

下面的示例显示了一个自定义等待条件实现，该实现检查元素是否具有特定的 css 类。

因为隐式等待是全局应用的，所以建议不要混合隐式和显式等待。

## 流畅的等待

`FluentWait`定义等待条件的最长时间，以及检查条件的频率。Python API 没有`FluentWait`类，但是它用关键字参数支持它。

建议你看一下马丁福勒的[流畅界面贴。](https://www.martinfowler.com/bliki/FluentInterface.html)

## 要记住的事情

*   当 Selenium WebDriver 加载页面时，它默认使用`normal`页面加载策略，等待`load`事件从`driver.get`请求返回给调用者。
*   使用隐式等待，WebDriver 在引发异常之前会在指定的时间内轮询 DOM。一旦设置，它对 WebDriver 实例的整个生命周期和页面上的所有元素都有效。
*   显式等待等待特定元素的特定条件发生。

在下一篇文章中，我将讲述 URL、窗口和框架之间的导航。

## 参考

1.  [https://www.selenium.dev/documentation/en/webdriver/waits/](https://www.selenium.dev/documentation/en/webdriver/waits/)
2.  [https://www.softwaretestingo.com/selenium-wait-commands/](https://www.softwaretestingo.com/selenium-wait-commands/)
3.  [https://selenium-python.readthedocs.io/waits.html](https://selenium-python.readthedocs.io/waits.html)
4.  [https://github . com/selenium HQ/selenium/blob/trunk/py/selenium/web driver/support/wait . py](https://github.com/SeleniumHQ/selenium/blob/trunk/py/selenium/webdriver/support/wait.py)

谢谢你的时间。