# 没有这三个工具，我想不出 react 的原生开发

> 原文：<https://blog.devgenius.io/without-these-three-tools-i-can-not-think-of-react-native-development-4efb53dd6298?source=collection_archive---------3----------------------->

![](img/917e9a2c5696d5ab7a2c78626255d91c.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com/photos/pKRNxEguRgM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

如果我的时间没有投入到产品开发中，那就是浪费。作为开发人员，生成构建并分发它需要很多时间，这完全是浪费；我的机器在构建过程中速度变慢；因此，我非常依赖这些工具。

让我们深入研究一下。

首先，我想谈谈代码推送

## **代码推送:帮助你将应用程序直接更新到用户手机上**

Code Push 是一款为 Cordova 和 ReactNative app developer 设计的工具，用于将移动应用直接更新到用户手机上。

它具体做的是准备 javascript 包并推送到用户设备。不需要应用更新，不需要在 TestFlight 和 PlayStore 上构建和等待应用处理。

是不是很神奇？

假设您正在开发一个应用程序，测试团队发现了一些小的错误/改进，您想再次向团队发布新版本，但是为 android 和 iOS 创建一个全新的包至少需要一个小时。

想象一下，如果你能在几分钟内发送这些更新；这将为两个团队节省时间。您所要做的就是点击一个与平台相关的命令，Codepush 将处理编译和交付。

在过去的两年里，我个人一直在使用代码推送，它在交付应用程序和专注于开发方面帮助了我很多。CodePush 可以对 JavaScript、HTML、CSS 和图像进行任何更改。

可以了解更多关于[码推](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/)的内容。

## **应用中心:构建、测试&分发**

是第二受欢迎的工具。App Center 是针对 iOS、Android、Windows 和 **macOS** 应用的一体化移动开发生命周期解决方案(复制自 AppCenter 网站；呵呵)。

基本上，AppCenter 允许你直接构建、测试和分发你的 iOS 和 Android 应用到各自的用户或平台商店。

配置 AppCenter 很简单；遵循这三个步骤，你就准备好了

1.  ***连接你的知识库***；AppCenter 支持 GitHub、Bitbucket、azure devos*等。*
2.  ****配置您的分发渠道:*** 您可以为您的应用程序连接 AppStore 或 PlayStore，或者您可以创建群组，以便 AppCenter 在您分发应用程序时向相应的用户发送邀请。*
3.  ****配置分支:*** 一旦您连接了您的存储库，您将不得不使用钥匙链、p12 证书和构建频率(如自动、手动和最后分发渠道)来配置您想要的分支。*

*此外，当 AppCenter 接管 CodePush 时，您可以从 AppCenter 门户管理您的 CodePush 版本。*

## ***像专业人士一样调试脚蹼。***

*当涉及到调试你的应用程序时，RN 为你提供了 React 本地开发工具，Chrome Inspect，日志等等。*

*但是问题是如果你想运行开发工具，你必须打开终端并启动 react-dev tools；开发工具的另一个问题是，如果你想检查组件，你必须在模拟器上摇动或点击 CMD+D (iOS)来打开开发者菜单，打开显示检查，然后检查它对我来说太多了。*

*另一方面，inspect 打开 chrome 调试器，然后再打开一个控制台对我来说太多了。*

**因此 FlipperJs，*允许你查看崩溃日志，控制台日志，网络，检查布局等同于 Chrome Inspect 元素和 Flipper 中的整个 React Devtools 插件。一个工具可以满足所有的需求，除此之外，你还可以用其他插件来扩展。*

**就是这样！感谢阅读。如果您有任何疑问，请在下面评论。**