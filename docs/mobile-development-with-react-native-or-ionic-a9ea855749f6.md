# 使用 React 进行移动开发:本地还是离子

> 原文：<https://blog.devgenius.io/mobile-development-with-react-native-or-ionic-a9ea855749f6?source=collection_archive---------2----------------------->

![](img/8c304389c7ab54c0a83913995f4bc45a.png)

React 是最流行的 JavaScript 框架之一，用于在 web 上构建交互式 ui。我们在 [AWH](http://awh.net) 使用它是因为它的速度、简单和成熟的生态系统。组件驱动模型功能强大，易于开发人员掌握。我们喜欢用 React 开发 web 应用程序，但是我们如何将这些技能应用到移动应用程序开发中呢？

事实证明有几种方法，但我想看看两种:[反应原生](https://reactnative.dev/)和[离子框架](https://ionicframework.com/)。

# 如何搭建跨平台 App

在我们深入每个框架的细节之前，让我们后退一步，看看用于构建跨平台移动应用的两种主要方法。

第一个是我认为的混合方法，也是 Ionic 使用的方法。这种方法背后的想法是将应用程序的代码包装在一个轻量级的外壳中，并在某种画布上绘制 UI。对于 Ionic 来说，这个外壳被称为电容器，基本上是一个超级强大的浏览器，看起来和行为都像一个独立的应用程序。您的代码是用 HTML、CSS 和 JavaScript 编写的，并由浏览器呈现。

[Flutter](https://flutter.dev/) 和其他也使用了类似的方法，框架使用低级图形库和画布控件而不是浏览器将每个控件呈现到屏幕上。

另一个主要方法是我喜欢称之为“本地”方法。这就是 React Native 和 [Xamarin。表格](https://dotnet.microsoft.com/apps/xamarin/xamarin-forms)(即将发布。网 MAUI)使用。这里的想法是采用一个单一的代码库，并以一种框架可以将其转换成平台本机控件的方式编写您的 UI。在 Xamarin。在形式上你用 XAML，在反应上你用 JSX。例如，当您想要呈现一个按钮时，框架会在 iOS 上呈现一个 UIButton，在 android 上呈现一个 android.view.button。

每种方法都有其优点和缺点。通过检查 React Native 和 Ionic(它们都使用相同的核心 React 库)，我们可以看到每种方法的不同之处和相同之处。

为了便于比较，我使用两种框架创建了一个简单的应用程序。两者的源代码都可以在 GitHub 上找到。点击此处查看 [React 原生应用](https://github.com/amoscardino/PinmarkReactNative)点击此处查看 [Ionic Framework 应用](https://github.com/amoscardino/PinmarkIonic)。

# 反应自然

React Native 是脸书在 2015 年创建的一个移动应用 UI 框架。它位于核心 React 库之上，并不取代它。相反，如果你熟悉浏览器中的 React，它会取代 React DOM。React DOM 呈现 HTML 元素，React Native 呈现平台原生元素。

你仍然可以使用 JavaScript 和 JSX 编写组件，但是运行代码的不是浏览器。相反，React Native 使用一个后台进程来解释和执行您的 JavaScript 并呈现本机 UI 控件。尽管仍在使用 JSX，但你不用编写呈现 HTML 的组件。相反，您使用 React Native 提供的一组本机组件，这些组件对应于平台上的一些基本 UI 元素。React Native 的文档中有一个[所有这些组件的综合列表，](https://reactnative.dev/docs/components-and-apis)而且它们非常基础。像视图、按钮、平面列表等。

这些原生组件是基本的，因为它们意味着被用作最小的构建块，被组合成更大、更复杂的组件。使用 JavaScript 对象和样式表 API 对这些本地组件进行样式化。这些风格工作起来很像 CSS，但绝对是*而不是* CSS，这使得从 web 开发的过渡有点棘手。

除了核心的原生组件，React Native 没有包含太多其他内容。这是一个非常基本的框架，它依赖于你要么找到一个库，要么编写自己的库来实现功能。这是一种“不含电池”的方法。这就是[博览会](https://expo.io/)的用武之地。Expo 是一个工具和 API 的集合，使 React Native 更加完整和易于使用。CLI 对于引导、构建、测试和部署应用程序非常有用。SDK 还包括许多 API，用于仅使用 JavaScript 与本机功能进行交互；比如摄像头、陀螺仪、网络状态等等。

Expo 还提供了一些*可选的*云托管选项用于测试和分发。

一旦你把 Native 和 Expo 配对，它就变成了一个更完整的画面。添加一些库来填充常用功能，然后你就可以非常快速地拼凑出一个应用程序。对于上面提到的示例应用程序，我使用了一个名为 [React Native Elements](https://reactnativeelements.com/) 的库来提供一些基本的样式，使用 [React Navigation](https://reactnavigation.org/) 来提供一些基本的导航功能。

# 离子框架

Ionic Framework 是另一个从 2013 年就存在的移动 app UI 框架。最初它是建立在 AngularJS 和 Cordova 之上的，但是最近的版本现在是框架无关的，并且使用了一个叫做 Capacitor 的新包装器。Ionic 应用**是用 HTML、JavaScript 和 CSS 编写的 web 应用**。Ionic 类似于 Expo，因为它包含一个 CLI 来帮助启动新项目、运行、调试、测试和部署。它也是一个 UI 库，提供一组预建的 [web 组件](https://developer.mozilla.org/en-US/docs/Web/Web_Components)来模仿本地平台控件。

因为 Ionic 由 web 组件组成，所以它支持多种流行的前端框架。当前支持包括 Angular、Vue 和 React。也可以完全跳过框架，只使用普通的 JavaScript。由于这些组件是使用标准的 web 技术构建的，它们很容易像其他 web 应用程序一样使用 CSS 进行定制和样式化。

一些影子 DOM 的使用会使一些组件的样式比其他的更复杂，但是他们的文档做得很好[解释了如何解决这个问题](https://ionicframework.com/docs/theming/css-shadow-parts)并且标记了可能是问题的组件。

对于许多 web 开发人员来说，“Cordova”这个词让他们带着糟糕的记忆逃离了笨拙的 web 应用程序，它们被包装成了本地应用程序。Cordova 仍然由 Ionic 支持，但它已被有效地替换为[电容](https://capacitorjs.com/)。Capacitor 充当一个超级强大的 web 视图:一个外观和行为都像本机应用程序的浏览器。但它不是浏览器，因为它能够与平台本地代码交互。有一些官方插件可以与本地 API 交互，还有许多社区插件。遗留的科尔多瓦插件也支持和工作得非常好。这种互操作性意味着可以很容易地向您的应用程序添加插件，以允许应用程序与平台功能进行交互，如摄像头、陀螺仪、网络状态等，所有这些都在 JavaScript 中。

Ionic 是一个非常完整的框架。除了 UI 组件和 CLI 套件，它还包括一个 [React Router](https://reactrouter.com/) 的[自定义包装器](https://ionicframework.com/docs/react/navigation)，帮助您轻松配置应用内的导航。像 Expo 一样，Ionic 提供了一些云托管的解决方案来测试和部署你的应用程序，尽管它们也是可选的，并且框架可以很容易地在没有它们的情况下使用。

# 你应该使用哪一个？

和软件开发中的其他事情一样，这个问题没有完美的答案。两个框架都是成熟和稳定的。两者都可以与本地 API 接口，并且可以部署到两个主要的移动平台。

React Native 的优势在于它是由脸书支持和开发的，他还维护着 React 的主要项目。它也有 Expo，可以省去使用 React Native 进行设置和开发的许多麻烦。它的主要缺点是，虽然它确实使用了 React 组件模型，但您必须使用 React Native 提供的本机组件。由于缺少 HTML 和 CSS，对于已经知道并在浏览器中使用 React 的开发人员来说，这意味着更陡峭的学习曲线。

爱奥尼亚没有这个缺点。在 Ionic 中开发和为浏览器开发是完全一样的，因为你*是*为浏览器开发。Ionic 应用程序甚至可以部署为 web 应用程序(尽管您可能会失去一些原生功能)。因为 Ionic 使用的 React 设置与我们在 web 应用中使用的相同，所以它与我们已知的 React 库有更好的兼容性。Ionic 更完整的内置 UI 库对某些项目来说也是一个巨大的进步，但对其他人来说可能有点障碍，这取决于他们的 UI 需求。

如果我必须选择在任何给定的项目上使用什么，我可能会*倾向于【Ionic，因为它与其他 React web 项目相似，并且有更完整的 UI 解决方案。但这并不意味着它会自动成为我的选择。React Native 是一个应该考虑的强大框架。老实说，我们有点被宠坏了，因为 React 提供了多种创建移动应用的优秀解决方案。*

## ——安德鲁·莫斯卡迪诺， [AWH](http://awh.net) 的软件开发人员。我们正在帮助企业通过技术推动增长。