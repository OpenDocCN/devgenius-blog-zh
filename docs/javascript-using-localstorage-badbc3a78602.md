# JavaScript:使用本地存储

> 原文：<https://blog.devgenius.io/javascript-using-localstorage-badbc3a78602?source=collection_archive---------8----------------------->

![](img/b21e832447d7cc3f3ba73fdaef197318.png)

什么是 localStorage？这是一种在浏览器中本地存储数据的方式，没有任何截止日期。还有一种叫做 sessionStorage 的东西，它与此类似，但在页面会话结束后过期，也就是在页面关闭时终止。localStorage 存储的信息甚至可以在关闭页面或浏览器后继续存在。清除本地存储的一种方法是，假设你在浏览器中使用隐名模式，然后关闭所有打开的隐名浏览器/标签。

使用 localStorage 可以存储什么？

你可以储存各种各样的东西。您可以存储键/值对、字符串、数组和对象。

有四种方法。

`setItem()` —将密钥/密钥对添加到本地存储

`getItem()` —通过密钥从本地存储中检索密钥/密钥对

`removeItem()` —通过密钥从本地存储中删除密钥/密钥对

`clear()` —清除本地存储

这里有一个来自我的[作品集网站](https://jonathanwong110.github.io/JonathanWongPortfolio)的例子。

添加到本地存储:

`localStorage.setItem(‘theme’, ‘dark’);`

如果您想在浏览器上检查本地存储，请打开控制台或按。然后进入应用程序，寻找本地存储并双击它。

如果你使用的是 Chrome，你可以在 windows 上使用(`CTRL` + `Shift` + `J`)，或者在 macOS 上使用(`CMD` + `OPTION` + `J`)。

如果您想从 localStorage 中检索数据，可以使用这个。

```
const example = localStorage.getItem(‘theme’)
console.log(example)
```

从本地存储中删除:

```
localStorage.removeItem(‘theme’)
```

清除本地存储:

```
localStorage.clear()
```

因此，您可能会问自己将在什么上下文中使用 localStorage。让我们以网页上的夜间模式选项为例，这通常是通过本地存储来实现的。如果您的浏览器不要求用户登录，并且有夜间模式选项，您将如何在浏览器中保存该首选项，而不必在用户每次访问页面时进行调整？如果一个用户经常访问你的页面，他们不会希望每次在关闭页面/浏览器后都切换到夜间模式选项。这样，localStorage 会记住在夜间“设置”页面。

说到夜间模式，现在让我们以我自己的[作品集网站](https://jonathanwong110.github.io/JonathanWongPortfolio)为例，深入研究如何使用 JavaScript 部署夜间模式。

```
const toggleSwitch = document.querySelector(‘input[type=”checkbox”]’)
const currentTheme = localStorage.getItem(‘theme’)
const element = document.bodyif (currentTheme) {
 document.documentElement.setAttribute(‘data-theme’, currentTheme);
 if (currentTheme === ‘night’) {
  toggleSwitch.checked = true;
 }
 if (localStorage.theme === “night”) {
  element.classList.toggle(“night-mode”)
 }
}function switchTheme(e) {
 if (e.target.checked) {
  document.documentElement.setAttribute(‘data-theme’, ‘night’);
  localStorage.setItem(‘theme’, ‘night’);
  element.classList.toggle(“night-mode”)
 } else {
 document.documentElement.setAttribute(‘data-theme’, ‘light’);
 localStorage.setItem(‘theme’, ‘light’);
 element.classList.toggle(“night-mode”)
 }
}toggleSwitch.addEventListener(‘change’, switchTheme, false);
```

因此，首先，我创建了一个开关，以便用户可以在开启/关闭夜间模式之间切换，创建了一个变量来检索当前主题(作为检查网页当前设置为何种模式的方法，即夜间模式)，然后为 document.body 设置一个变量，因为这是我们将更改以反映夜间模式的变量。

下一个条件块，如果正在使用夜间模式，是什么使拨动开关持续。

现在最令人兴奋的部分是主题切换功能。如果开关被打开，我们就在这里部署夜间模式。如果已经进入夜间模式，它将主题设置为夜间，并向 localStorage 添加一个键/值。如果它被切换回光模式，我们改变我们的初始键/值来反映这一点。

最后，我们添加一个事件监听器来监听切换开关中的变化。

总之，localStorage 是 JavaScript 中一个强大的工具。我用的例子只是一个你可以使用它的实例，可以用在很多其他方面。请随意查看我的[投资组合网站](https://jonathanwong110.github.io/JonathanWongPortfolio)并切换夜间模式开关。让它开着，然后关闭页面或浏览器，看看它是否仍然存在。

[](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) [## 窗口.本地存储

### 只读 localStorage 属性允许您访问的源对象；存储的数据保存在…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)