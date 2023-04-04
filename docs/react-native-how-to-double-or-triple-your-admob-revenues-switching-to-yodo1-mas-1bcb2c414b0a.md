# React Native:如何提高你的 AdMob 收入转换到 Yodo1 MAS 货币化平台

> 原文：<https://blog.devgenius.io/react-native-how-to-double-or-triple-your-admob-revenues-switching-to-yodo1-mas-1bcb2c414b0a?source=collection_archive---------5----------------------->

![](img/2f8db6781c03122afecf01afe869c362.png)

你成功了，你的免费游戏在 App 和 Play 商店上线了，你还从 AdMob 获得了收入，这太棒了！

谷歌用 AdMob 做了一个很棒的广告平台，在 React Native 中集成它相对容易，甚至用 Expo ( [expo-ads-admob](https://docs.expo.dev/versions/latest/sdk/admob/) )也可以。但是 AdMob 有一个重要的限制:你只能从谷歌广告网络获得广告，就像看一个频道卡住的电视一样！

为什么不让多个广告网络竞争你游戏的广告空间，让最高 eCPM 赢得这场战斗？让付费最高的广告胜出！

进入 MAS，一个自动为你管理多个广告网络和中介的货币化平台，他们争夺你的屏幕空间，你可以获得比 Google AdMob 高两到三倍的 eCPM！

Yodo1 开发并支持 MAS sdk，他们从中抽取部分广告收入，因此不涉及任何前期成本。

本指南主要关注集成的 React 原生部分，因为其他平台(Unity、Flutter、Native)在他们的网站上都有很好的记录。

# 第一步:注册

在 Yodo1 上注册一个账户，这是我推荐的链接，如果你想给我一些东西的话【https://mas.yodo1.com/register?referral_code=RP_COMA4VI

# 第二步:配置

使用他们的资源(他们制作了许多 howto 视频和 explainer)来了解这个平台是如何工作的，这非常简单。

你需要注册两个游戏(iOS 和 Android)来获得必要的密钥。

您还需要将您的手机添加到测试设备，这样您就可以在开发应用程序时显示测试广告。

iOS 和 Android 的集成分为两个部分:应用程序配置(iOS 中的 info.plist，Android 中的各种 gradle 构建文件)和广告工作所需的本机代码。对于第一部分，请参考 iOS android 的 Yodo1 MAS SDK 文档，这里我们将重点介绍 React Native。

第一个坏消息:如果你的项目使用 Expo，你需要退出。我自己对本地代码不是很熟悉，这一步非常可怕，但经过一些工作后，我发现整体影响很小，我仍然使用 Expo 进行通知、空中更新和其他 expokit 库。

# 步骤 React Native 中的集成

Javascript 代码由两部分组成:显示横幅的 React 组件和处理事件并调用 ads 函数的助手文件。

该组件仅在 iOS 中是必需的，在 Android 中，横幅会自动以模态显示在屏幕底部。使用 isInitialized prop 仅在获取 onMasInitSuccessful 事件的初始化事件或使用 helper 文件中的 async 函数后显示横幅。

```
await Yodo1MASAds.isInitialized()
```

这是帮助文件，以便与本机代码进行交互。这段代码使用 NativeEventEmitter 接收来自本机端的事件(即初始化成功、获得奖励等)和 NativeModules，以便调用本机函数，显示间隙广告或奖励广告。

您现在可以在应用程序初始化时调用 registerYodoAds，它将返回一个侦听器，您可以在应用程序关闭时使用 listener.remove()

# 第四步:安卓

按照 Yodo1 Android 集成在这里一步一步地配置应用依赖关系、gradle 设置和 metatada 配置[https://developers . yodo 1 . com/knowledge-base/react-native-integration/](https://developers.yodo1.com/knowledge-base/react-native-integration/)

如果你使用 Expo-av，需要注意一个冲突:在你的 app/build.gradle 中你需要排除 exoplayer:

```
implementation ('com.yodo1.mas:full:4.3.2') {exclude group: "com.google.android.exoplayer"}
```

否则 expo-av 的声音和视频将无法工作，并出现错误:

```
[Error: Encountered an exception while calling native method: Exception occurred while executing exported method loadForSound on module ExponentAV: Failed resolution of: Lcom/google/android/exoplayer2/SimpleExoPlayer$VideoListener;]
```

对于 Java 部分，您可以在 app/src/main/java/my/package 中添加两个文件

这是将实际的 MAS 类集成为 React 本机模块所必需的

这是处理 React Native 和 forwards 事件调用的实际模块，您需要添加您的应用程序密钥！

# 第五步:iOS

按照 MAS 的集成指南步骤 1 和 2 设置项目依赖性、安全性和元数据[https://developers . yodo 1 . com/knowledge-base/IOs-integration/](https://developers.yodo1.com/knowledge-base/ios-integration/)

注意:编辑广告网络 id 时，复制粘贴到您的 info.plist 中的正确格式如下:

```
<key>SKAdNetworkItems</key>
<array>
<dict>
<key>SKAdNetworkIdentifier</key><string>275upjj5gd.skadnetwork</string>
</dict>
<dict>
<key>SKAdNetworkIdentifier</key><string>294l99pt4k.skadnetwork</string>
</dict>
...
...
</array>
```

iOS 的工作方式与 Android 稍有不同，这里我们需要一个实际的组件模块来将横幅放置在视图层次结构中。您可以在 xCode 中从 ios 根文件夹中的 file，new，m file 添加此文件，以设置可以从 React Native 显示的组件:

添加另一个 m 文件，用于处理 react native 的方法调用并将事件转发给 React Native。可以改进这一部分，以区分具体事件(即横幅错误、间隙错误等)。

# 第六步:盈利！

如果您可以在 iOS 和 Android 中正确显示测试横幅和奖励广告，集成就完成了！

现在，如果有必要，您可以调整您的游戏/应用程序逻辑，以便在适当的时间显示广告，并正确处理用户奖励。

在将更新发布到商店之前，不要忘记在商店(ATT 问卷，prvacy 页面)中配置您的应用程序隐私，以反映从 MAS skd 收集的数据。您还需要处理您的商店页面中的 URL，以正确处理 app-adds.txt。

# 最终注释:

由于只在 Expo 中开发了 React 原生应用，这次 SDK 集成对我来说是一个相当大的挑战，但也是一个很好的学习机会，可以了解更多关于 React 原生、Xcode、Android Studio、React 原生模块以及我认为理所当然的应用开发的许多方面。

这是我的第一篇媒体文章，我希望你能发现更多关于 React Natvie 的有用信息，并在此过程中获得额外收入。